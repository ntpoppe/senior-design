# D1_Detailed_Design: Chess Blunder-Risk Analyzer

**Team:** Braden Hayes, Nate Poppe

> **Note:** Several details are still being discussed with our advisor, mainly the training label, training data, model choice, and hosting. This document is our current plan; nothing here is final until after that meeting.

## Header, scope, and conventions

**Goal statement:** The goal of the Chess Blunder-Risk Analyzer is to develop a machine-learning-based system that estimates the likelihood that a chess player will make a blunder from a given position. The system will analyze completed chess games using factors such as player rating, clock time, game phase, position characteristics, and engine-derived information. It will then present move-level risk scores and broader patterns that help players identify situations in which they are more likely to make mistakes.

**Scope:** This document details three D0 components: Game and Results Storage, Game Import and Validation, and Chess Feature Extraction. The User Interface, Blunder-Risk Model, and Risk Explanation and Analytics are deferred to D2.

**Figure conventions:**

- Boxes are tables in our database. Each box lists only the columns that affect how the system behaves, with the column type on the right.
- Key tags: PK = primary key, FK = foreign key, UK = unique together, idx = indexed.
- Lines are relationships between tables, read in the direction of the italic label.
- Cardinality is shown at both ends of each line with crow's-foot symbols, repeated as text: 1 = exactly one, 0..1 = zero or one, 0..N = zero or more, 1..N = one or more.

Requirement IDs (US, UC, AC) come from our User Stories. Interface IDs (I1 to I9) come from D0.

## Data model

We are using a relational database (PostgreSQL). Our data is naturally connected: users have games, games have moves, and moves have scores. Several of our rules are easiest to enforce in the database itself. A game is never saved twice for the same user, every score belongs to a known model version, and an interrupted import never leaves half a game behind.

**Structural decisions**

| Kind | Decision | Reason | Req. |
|---|---|---|---|
| Entity vs. attribute | Move is its own table, not a list inside Game | Replay, analysis, and scores all need to point at one specific move | US-01, AC-01.2 |
| Entity vs. attribute | ModelVersion is its own table, not a text field on each score | Every score is guaranteed to belong to a real model version | US-02 |
| Entity vs. attribute | ImportJob is its own table | Tracks each import's progress so an interrupted import can pick up where it left off | AC-01.3 |
| Entity vs. attribute | MoveAnalysis is separate from Move | Engine results are expensive to produce; keeping them lets a new model rescore old games without rerunning the engine | US-02, I5 |
| One-to-many vs. many-to-many | User to Game is one-to-many | If two users import the same game, each gets their own copy, which keeps data private per account | UC-01 |
| One-to-many vs. many-to-many | Move and ModelVersion are many-to-many, linked through RiskScore | Each move can be scored by every model version, so versions can be compared on the same games | US-02 |
| One-to-many vs. many-to-many | Move to MoveAnalysis is zero-or-one | Moves with only one legal option, or where the engine fails, have no analysis | I5 |
| Index | Game (user_id, source_game_id), unique | Prevents the same game from being imported twice | AC-01.3 |
| Index | Game (user_id, played_at) | Lists a user's recent games and tells an interrupted import where to resume | US-01, AC-01.3 |
| Index | RiskScore (game_id, model_version_id, score) | Finds a game's riskiest moves quickly | AC-01.1 |
| Index | ModelVersion (is_active), unique when true | Makes sure only one model version is active at a time | US-02 |

## Core algorithms

Two parts of the system can break the product if they are slow or wrong. **A1** makes sure games are imported completely and only once. **A2** analyzes every move with the chess engine, and it is the slowest step in the pipeline.

### A1: Resumable game import

**What it solves:** A player's recent finished games are downloaded and saved. If the import is interrupted partway through, already-saved games are kept, and resuming does not download them again (AC-01.3). Games still in progress are never saved (US-03).

| Inputs | Outputs |
|---|---|
| The player's Lichess username | The import's status (running, interrupted, or done) |
| The time control to import (bullet, blitz, rapid, or classical) | How many games were saved |
| How many recent games to import, or an uploaded PGN file instead of a username | The saved games and their moves |

**How it works:** Games arrive from Lichess newest first. Each game is checked to make sure it is finished, is standard chess, and has at least two moves. It is then saved together with its moves in one step, so a game is either fully saved or not saved at all. If the import is interrupted, it is marked as interrupted. When resumed, it only asks Lichess for games older than the oldest one already saved.

**Complexity:** O(g × m), where g is the number of games and m is the moves per game, so the work grows in a straight line.

- Realistic size: 50 games of about 80 moves each is about 4,000 moves to check and save, which takes well under a second. Most of the time goes to downloading from Lichess.
- At 100 times the size: 5,000 games is about 400,000 moves. That is still a straight line, but it doesn't matter, because imports are capped at 50 games.

**Why this approach:** Re-downloading everything on resume would waste time and break AC-01.3. Saving the whole import in one step would lose every game if anything went wrong. Asking for a list of game IDs first and then fetching each game separately would mean many more requests to Lichess.

**Edge cases:**

- If the username doesn't exist, the import is rejected before it starts (UC-01).
- If the player has no matching games, the import finishes with zero games saved.
- If a game was already saved, it is skipped (UC-01).
- If a game is still in progress, it is skipped and never analyzed (US-03).
- If the import is interrupted, the user is told and can resume it (AC-01.3, I2).
- If one game in an uploaded PGN file is broken, it is skipped and the rest are still imported (UC-01).
- If the user presses resume twice, the second request is refused.

### A2: Engine analysis

**What it solves:** Each move the player made is run through Stockfish, and the result is turned into the information the model needs. This must finish quickly enough that the user isn't left waiting (AC-01.1). Rerunning a game must always give the same result, so different model versions can be compared fairly (US-02).

| Inputs | Outputs |
|---|---|
| The board position before each of the player's moves | The engine's best move for that position |
| The clock time remaining at each move | The engine's evaluation of the position |
| How deeply the engine should search | Whether the analysis succeeded, was skipped, or failed |
| How many engines to run at once | The features passed to the model |

**How it works:** Several copies of Stockfish run side by side, one per spare CPU core. Each searches a position to the same fixed depth, so results don't change between runs. Positions that come up more than once reuse the earlier result. A move with only one legal option skips the engine entirely. If the engine crashes or takes too long, the position is retried once and then marked as failed (I5).

**Complexity:** O(p × d ÷ e), where p is the number of positions, d is how deep the engine searches, and e is how many engines run at once. Everything besides the engine is fast by comparison.

- Realistic size: 50 games with about 40 player moves each is about 2,000 positions. With 4 engines running at once, each engine handles about 500.
- At 100 times the size: about 200,000 positions would take far too long to wait for. That only matters for building a training set, which won't be done this way. Normal use only analyzes a player's recent games.

**Why this approach:** Searching for a fixed amount of time per move would give different results depending on how busy the computer is, which breaks US-02. Searching to a fixed depth in moves, rather than a fixed amount of work, can take wildly different amounts of time on complicated positions. Analyzing the opponent's moves too would double the work for nothing, since only the player's moves are scored.

**Edge cases:**

- If a game has no moves by the player, there is nothing to analyze.
- If a move had only one legal option, it is not scored, since there was no decision to get wrong.
- If the engine crashes or freezes, the position is retried once and then marked as failed (I5).
- If two moves tie for third-riskiest, the earlier move is shown (AC-01.1).
- If a game has fewer than three scored moves, only those are shown.
- If Stockfish isn't running, new imports are refused until it is back.

## Build-versus-reuse decisions

Each library we reuse was checked for how mature and actively maintained it is, its license, whether it performs well enough for our use, and how well it fits a Python stack.

| Piece | Build or reuse | Library or service | License | Reason |
|---|---|---|---|---|
| Fetching games | Reuse | Lichess API | Free (Lichess terms) | The source of player games with clock times |
| Sign-in | Reuse | Lichess sign-in (OAuth) via Authlib | BSD-3-Clause | We should not build our own authentication |
| Chess rules and game files | Reuse | python-chess | GPL-3.0 | Mature and widely used; rewriting chess rules is out of scope |
| Chess engine | Reuse | Stockfish | GPL-3.0 | The strongest free engine |
| Web API and input checking | Reuse | FastAPI and Pydantic | MIT | Input checking and API docs come from one definition |
| Database access | Reuse | SQLAlchemy and Alembic | MIT | Mature and handles database changes over time |
| Dates and sorting | Reuse | Python standard library and PostgreSQL | PSF / PostgreSQL | Already built in |
| Import and resume (A1) | Build | None | N/A | Specific to how our imports work |
| Running engines side by side (A2) | Build | Python asyncio (built in) | PSF | A small amount of code; no extra service needed |
| Turning analysis into features | Build | None | N/A | The core of our project |

## API contract

| Endpoint | What it does | What you send | What you get back | When it fails |
|---|---|---|---|---|
| `POST /v1/imports` | Starts importing a player's games from Lichess | **username** (required, 2 to 30 characters), **time control** (required: bullet, blitz, rapid, or classical), **number of games** (required, 1 to 50) | The import's ID, its status, and how many games have been saved so far | **400** if a field is missing or invalid; **401** if the user isn't signed in; **404** if the username doesn't exist; **409** if the user already has an import running; **503** if the chess engine isn't running |
| `POST /v1/imports/pgn` | Starts an import from an uploaded PGN file | **file** (required, a .pgn file) | The import's ID, its status, how many games were read, and how many were skipped | **400** if the file isn't a PGN; **413** if the file is too large; **422** if no game in the file could be used |
| `GET /v1/imports/{id}` | Checks on an import's progress | **id** of the import (required) | The import's status, how many games are saved, and how many are analyzed | **404** if the import doesn't exist or belongs to someone else |
| `POST /v1/imports/{id}/resume` | Resumes an interrupted import | **id** of the import (required) | The import's ID and its new status (running) | **404** if the import doesn't exist; **409** if the import isn't interrupted |
| `GET /v1/games/{id}/moves` | Gets every move in a game with its risk score, for replay | **id** of the game (required), **model version** (optional, defaults to the current one) | Each move's number, board position, clock time, risk score (0 to 100, or empty if not scored), and the engine's best move | **404** if the game or model version doesn't exist |

**Example request and response:**

```http
POST /v1/imports
Content-Type: application/json

{"username": "example_player", "perf_type": "blitz", "max_games": 20}
```

```json
HTTP/1.1 202 Accepted

{"job_id": "3f9c2a1e-8b4d-4f6a-9e21-5c7d0b8a4e12", "status": "running", "imported_n": 0}
```

**Versioning:** The API is versioned by the `/v1` at the start of each path. Removing or renaming a field, changing what a field means, or adding a new required field is a breaking change and would go under `/v2`. Adding optional fields or new endpoints is not.

## Technology choices

Each paragraph covers the five criteria: **skill fit**, **licensing**, **community**, **performance**, and **cost/hosting**.

**Database: PostgreSQL.** *Skill fit:* Nate writes SQL daily against MySQL at his LCS co-op, so the schema and queries carry over directly. *Licensing:* free, permissive license. *Community:* one of the most widely used databases, with strong Python support. *Performance:* our data is small per user, and the indexes above keep lookups fast. *Cost/hosting:* free, and runs in a Docker container. *Passed over:* MySQL, which Nate uses daily at LCS. It would work, but PostgreSQL has stronger built-in support for flexible JSON data like engine features, and it is the more common choice alongside Python data tools.

**Backend: Python with FastAPI.** *Skill fit:* Nate built a machine learning pipeline in Python at his KLH Engineers co-op, and the model will also be in Python, so one language covers the whole backend. *Licensing:* free and permissive (PSF and MIT). *Community:* python-chess and the machine learning libraries are all Python-first. *Performance:* the heavy work happens inside Stockfish, so Python mostly waits on the engine and the network. *Cost/hosting:* free, one container. *Passed over:* ASP.NET, which Nate uses at LCS, because the model would then have to run as a separate Python service.

**Front end: React with TypeScript.** *Skill fit:* Nate writes TypeScript daily in Angular at LCS, so typing and tooling carry over; React's component model is the main new piece. *Licensing:* MIT. *Community:* the largest front-end ecosystem, and most maintained chessboard components are built for React. *Performance:* one board and a move list is a light load. *Cost/hosting:* served by the backend, no extra cost. *Passed over:* Angular, because its chessboard libraries are less maintained.

**Hosting: Docker Compose on a local machine (tentative).** *Skill fit:* Nate used Docker extensively at KLH Engineers to run machine learning pipelines and host applications. *Licensing:* Docker Engine and Compose are free (Apache-2.0). *Community:* official images and plenty of documentation for this setup. *Performance:* the demo runs on the same machine we test on, so what we measure is what we show. *Cost/hosting:* free, and the same setup can move to a small cloud server if we prefer.
