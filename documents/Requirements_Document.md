# Chess Blunder-Risk Analyzer: Requirements Document

Braden Hayes, Nate Poppe

CS5001 Senior Design


## 1. Version and Change Log

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 10/29/26 | Pulled requirements together from our user stories, constraints essay, D0 to D2, and project plan. NFR-01 to NFR-03 keep their D2 IDs. | Nate Poppe |
| 0.2 | 10/29/26 | Redid priorities, since D2 had everything as Must. Moved REQ-03, REQ-05, REQ-06, REQ-08, REQ-15, and NFR-01 to Should. Dropped the "live risk score" wording from our project description (W-01). | Braden Hayes, Nate Poppe |
| 1.0 | 10/30/26 | Added REQ-19 because CON-03 had no requirement behind it. No design changes since the Week 7 review. Peer review changes PR-01 and PR-02 are REQ-16 and REQ-17. | Braden Hayes |
| 1.1 | 10/30/26 | Changes from our advisor review. Updated OQ-03 to compare linear regression against other model types. | Braden Hayes |

## 2. Purpose and Scope

This document describes what the Chess Blunder-Risk Analyzer needs to do and who it is for. How we plan to build it is in our Technical Specification.

The system lets a player sign in with their Lichess account and import between 1 and 50 of their own finished games, either straight from Lichess or by uploading a PGN file. Stockfish analyzes each move the player made, and our model gives each move a blunder-risk score from 0 to 100. For each game, the player can see their riskiest moves, a few plain-language reasons each one was risky, and what the engine would have played instead. Every score is saved with the model version that made it. Patterns across a player's games are the lowest priority.

The Won't list in Section 7 covers what we aren't doing. Most importantly, the system will never analyze a game that is still being played.

## 3. Stakeholders and User Goals

These come from our Week 4 user stories (`documents/user-stories.md`).

| Stakeholder | Type | Goal | Story |
|---|---|---|---|
| Online chess player | Primary | Find the riskiest moves in their recent games and why they were risky, so they know what kinds of positions and clock situations to practice. | US-01, UC-01 |
| Model maintainer | Secondary | Know which model version produced every stored score, and compare a new version to the old one on the same games before switching. | US-02 |
| Lichess fair-play moderator | Hidden | Be sure the tool won't analyze games that are still going, so it can't be used to cheat. | US-03 |

## 4. Glossary

| Term | Meaning |
|---|---|
| Player | A signed-in user who imports and looks at their own games. |
| Model maintainer | The team member who trains new model versions and decides when to switch to one. |
| Fair-play moderator | Lichess staff who enforce the rule against engine use during games. |
| Finished game | A game with a final result. Any other game is in progress. |
| Blunder | A move that makes the player's position a lot worse according to Stockfish. The exact cutoff is still being decided (OQ-01). |
| Risk score | The model's estimated chance of a blunder in a position, times 100 and rounded (0 to 100). |
| Risk factor | A short sentence, 25 words or fewer, that explains why a move was risky. It comes either from the model or from a fixed rule. |
| Feature name | A variable name used inside the model, like `clock_ratio`. These should never show up in a risk factor. |
| Forced move | A move where only one legal move was possible. We analyze these but don't score them. |
| Engine best move | The move Stockfish would have played in the position. |
| Model version | One trained model. Only one version is active at a time. |
| Import | One request to bring games in, either from Lichess or a PGN file. An import is running, interrupted, or done. |
| Interrupted import | An import that stopped early, for example from a rate limit or a crash. Saved games are kept, and resuming only fetches older games. |
| Time control | How fast the game was: bullet, blitz, rapid, or classical. |
| Fallback value | A flagged value we fill in when a PGN game is missing clock times or ratings. |
| Pattern | A situation, like a game phase or clock range, where a player's average risk is high. |
| PGN, FEN, UCI | Standard chess formats for a whole game, a single position, and talking to an engine. |
| MoSCoW | Our priority scale: Must, Should, Could, and Won't (for this release). |

## 5. Functional Requirements

Verification methods are Test (automated test), Demo (demonstration), and Inspection.

| ID | Requirement | Priority | Source | Verification |
|---|---|---|---|---|
| REQ-01 | The system shall require the player to sign in with Lichess, and shall return a 401 error for any request without a valid session, except for sign-in and status requests. | Must | UC-01 precondition 1 | Test |
| REQ-02 | The system shall only accept a Lichess import if the username is 2 to 30 letters, numbers, underscores, or hyphens, the time control is bullet, blitz, rapid, or classical, and the number of games is 1 to 50. Anything else shall be rejected with an error naming the bad field before Lichess is contacted. | Must | UC-01 step 1, EC-01, EC-24 | Test |
| REQ-03 | Before the player confirms a Lichess import, the system shall show how many matching games are available, or a "player not found" message if the account doesn't exist. | Should | UC-01 step 2 | Test |
| REQ-04 | The system shall only save and analyze finished standard chess games with at least 2 moves. No game that is still in progress shall ever be saved or sent to Stockfish. | Must | US-03, EC-05 | Test |
| REQ-05 | The system shall accept a PGN file of 1 to 50 games up to 5 MB, import every valid game, skip broken ones without failing the whole import, and tell the player how many games were read and how many were skipped. | Should | UC-01 alternate flow A1, EC-03 | Test |
| REQ-06 | While an import is running, the system shall show "n of N imported" and update it at least every 2 seconds. | Should | UC-01 step 4 | Demo |
| REQ-07 | The system shall save at most one copy of a game for each player. Importing a game the player already has shall add nothing new. | Must | UC-01 postcondition, EC-07 | Test |
| REQ-08 | If Lichess rate-limits us or the connection drops, the system shall mark the import as interrupted, keep every game saved so far, show a message within 2 seconds, and re-download none of the saved games when the player resumes. | Should | UC-01 exception flow E1, AC-01.3 | Test |
| REQ-09 | The system shall give every non-forced move the player made a whole-number risk score from 0 to 100. Forced moves are saved without a score. | Must | US-01, EC-10, EC-14 | Test |
| REQ-10 | The Games page shall list the player's analyzed games from newest to oldest, each with its 3 highest-risk moves. If a game has fewer than 3 scored moves, all of them are shown. If two moves tie, the earlier one is shown. | Must | US-01, AC-01.1, EC-22 | Test |
| REQ-11 | When the player picks a move in Replay, the system shall show the board in the exact saved position before that move, along with the move's risk score and the engine best move. | Must | US-01, AC-01.2 | Test |
| REQ-12 | For every move with a risk score of 70 or more, the system shall show 2 or 3 risk factors. Each factor shall be 25 words or fewer, shall not contain a feature name, and shall not be based on a fallback value. | Must | AC-01.2, PR-02, EC-18, EC-20 | Test |
| REQ-13 | Every saved risk score shall point to a real model version. Old scores shall be kept when a new version becomes active, and the player shall be able to view a game's scores from any saved version. | Must | US-02, EC-08 | Test |
| REQ-14 | Before a new model version is made active, the system shall score the saved games with it using the saved engine results (no new Stockfish runs) and show its results next to the current version's on the same moves. | Should | US-02 | Demo |
| REQ-15 | The system shall report whether Stockfish and the model are running. If either one is down, it shall turn down new imports with a message saying which one. | Should | UC-01 precondition 3, EC-17, EC-33 | Test |
| REQ-16 | If an import has been marked running but hasn't saved a game in 5 minutes, the system shall mark it interrupted within 6 minutes of the last save, and also when the server starts up, so the player can resume it. | Should | PR-01, EC-38 | Test |
| REQ-17 | If a PGN game is missing clock times, ratings, or a time control, the system shall use the fallback rules from D2, flag the filled-in values, and still score the game. | Should | PR-02, EC-12, EC-39 | Test |
| REQ-18 | The Patterns page shall show up to 5 situations where the player's average risk is highest. Each is based on at least 10 moves, and the page says the results are partial if an import is still running. | Could | Goal statement, EC-19 | Test |
| REQ-19 | Every risk score shown shall be labeled "estimated blunder risk," and the Replay page shall have one line saying the score is a probability and not a guarantee. | Should | CON-03 | Inspection |

## 6. Non-Functional Requirements

The reference machine mentioned below is the one described in NFR-03.

| ID | Category | Requirement | Priority | Source | Verification |
|---|---|---|---|---|---|
| NFR-01 | Reliability | Analyzing the same game again with the same model version, Stockfish version, and search depth shall give the same engine best moves, evaluations, and risk scores for every move. | Should | US-02 (D2 NFR-01) | Test |
| NFR-02 | Security | A request for another player's import, game, or moves shall return a 404, the same as an ID that doesn't exist. A test with two players shall find no records crossing between them. | Must | D2 NFR-02, EC-28 | Test |
| NFR-03 | Portability | The system shall start with one `docker compose up` command on a computer with 4 CPU cores, 16 GB of RAM, and no GPU. A fresh install following our user guide shall take 30 minutes or less. | Should | D0 hardware section (D2 NFR-03), CON-01 | Demo |
| NFR-04 | Performance | On the reference machine, importing and analyzing a player's 20 most recent games shall list all 20 games with their 3 riskiest moves within 180 seconds of confirming, in 3 out of 3 timed runs. | Must | AC-01.1 | Test |
| NFR-05 | Performance | The games list and game moves requests shall respond within 500 ms for 95% of requests, for a player with 50 analyzed games, while one import is running on the reference machine. | Should | US-01 | Test |
| NFR-06 | Security | Sessions shall use signed, HttpOnly cookies that expire after 7 days, and the system shall not store any passwords. After running the full test suite, a search of the logs shall find no session cookies, sign-in tokens, or secret keys. | Should | UC-01 precondition 1, EC-28, EC-36 | Test, Inspection |
| NFR-07 | Usability | At least 4 out of 5 first-time users, with no help, shall be able to sign in, import 10 games, and open a flagged move in Replay within 5 minutes. | Should | US-01, UC-01 | Demo |
| NFR-08 | Reliability | If Stockfish crashes or times out on a position, only that position shall be affected. It is retried once and then marked as failed, and the rest of the game still gets scored. | Should | EC-13, D0 I5 | Test |
| NFR-09 | Reliability | Every error path in D2 Figures 2 and 3 shall write a log line that names its import, game, and move. Logs rotate daily and are kept for 14 days, and the app keeps running with console-only logging if the disk fills up. | Could | D2 Logging, EC-34, EC-37 | Test |

## 7. MoSCoW Summary

Must (11): REQ-01, REQ-02, REQ-04, REQ-07, REQ-09, REQ-10, REQ-11, REQ-12, REQ-13, NFR-02, NFR-04

These make up our minimum working product. A player can sign in, import finished Lichess games without duplicates, get every move scored, see each game's riskiest moves with reasons, and get results within 180 seconds. Scores stay tied to model versions, and players can't see each other's data.

Should (15): REQ-03, REQ-05, REQ-06, REQ-08, REQ-14, REQ-15, REQ-16, REQ-17, REQ-19, NFR-01, NFR-03, NFR-05, NFR-06, NFR-07, NFR-08

Could (2): REQ-18, NFR-09. If we fall behind, these are the first things cut, which matches risk R5 in our project plan.

Won't (this release):

| ID | What we won't do | Why |
|---|---|---|
| W-01 | Score or show risk for any game still being played, including the "live risk score" from our first project description | CON-02, US-03 |
| W-02 | A browser extension or overlay on Lichess or chess.com | CON-02 |
| W-03 | Importing directly from chess.com. Those players can upload a PGN. | Only one API to support |
| W-04 | Scoring the opponent's moves | Doubles engine work without helping the player |
| W-05 | Our own username and password accounts | Lichess sign-in proves account ownership |
| W-06 | Imports over 50 games, or training models inside the app | Training happens offline |
| W-07 | Explanations written by an AI text generator | A fixed phrase table keeps factors short and checkable |
| W-08 | Cloud hosting or a separate job queue service | CON-01, and D0 ruled out microservices |

## 8. Inherited Constraints

CON-01 through CON-04 come from our Week 3 constraints essay (`documents/design-constraints-essay.md`). CON-05 and CON-06 come from the outside services and libraries we picked in D0 and D1. The Technical Specification explains what each one rules out or forces.

| ID | Type | Constraint |
|---|---|---|
| CON-01 | Economic | We have no budget. We only use free tools like Stockfish and open-source libraries, and everything runs on our own computers instead of paid cloud servers. The model has to be small enough to run on a regular CPU. |
| CON-02 | Ethical | Lichess and chess.com both ban using an engine during rated games. Our tool can only be used to review games after they are over. |
| CON-03 | Ethical | The risk score is a probability, not a guarantee, and it has to be shown that way so players don't rely on it too much. |
| CON-04 | Professional | Explaining risk well takes real chess knowledge. Decisions about what makes a position risky and how factors are worded depend on Braden's chess experience. |
| CON-05 | External | Lichess only allows one request at a time, and a 429 error means we have to wait before trying again. We can only get public games and info the player signs in for. |
| CON-06 | Legal | Stockfish and python-chess are both GPL-3.0 licensed. |

## 9. Assumptions and Open Questions

| ID | Item | Owner | Due |
|---|---|---|---|
| A-01 | Lichess exports include clock times and a final status for finished games. | Nate Poppe | 11/13/26 |
| A-02 | Almost all players have a Lichess account, so requiring it for PGN uploads is fine. | Nate Poppe | 11/13/26 |
| OQ-01 | What counts as a blunder (the evaluation drop cutoff)? | Braden Hayes | 11/13/26 |
| OQ-02 | Where does our training data come from, and how do we split it? | Braden Hayes | 11/13/26 |
| OQ-03 | Which model type do we use? We will compare a linear regression baseline against other types, such as logistic regression and tree-based models. | Braden Hayes | 11/13/26 |
| OQ-04 | What accuracy or calibration does the model need to hit? Once agreed, it becomes a new NFR. | Braden Hayes | 11/13/26 |
| OQ-05 | Can we find a search depth that meets the 180-second target on the reference machine? | Braden Hayes | 1/22/27 |
| OQ-06 | Does the Lichess `until` option work the way resume needs it to? | Nate Poppe | 11/13/26 |
| OQ-07 | Is running everything on one local machine with Docker okay for the expo demo? | Nate Poppe | 11/13/26 |
