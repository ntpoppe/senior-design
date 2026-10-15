# Chess Blunder-Risk Analyzer: User Stories

Nate Poppe, Braden Hayes

## Stakeholder Map

- Primary: Online chess player. Imports their own games and reviews how risky each move was.
- Secondary: Model maintainer. Retrains the risk model and has to show that each new version is an improvement.
- Hidden (compliance): Lichess fair-play moderator. Never uses the tool, but has to deal with any in-game engine help it makes possible.

## User Stories

US-01 (primary): As an online chess player,  
I want to find the moves in my recent games that carried the highest blunder risk and what made each one risky,  
so that I have a specific list of position types and clock situations to practice.

US-02 (secondary): As the team member who retrains the blunder-risk model,  
I want to trace every stored risk score to the model version that produced it,  
so that I have a way to compare a new version against the old one on the same games before releasing it.

US-03 (hidden): As a Lichess fair-play moderator,  
I want to be sure the tool refuses to analyze games that are still being played,  
so that I have no new source of in-game engine assistance to police.

## INVEST Check

We checked each story against the six INVEST criteria. US-01, US-02, and US-03 pass all six.

## Use Cases

### UC-01: Analyze Recent Games for Blunder Risk

Expands: US-01  
Primary actor: Online chess player  
Secondary actors: Lichess API, Stockfish engine, risk-model service

Preconditions:
1. The player is signed in.
2. Their Lichess username has at least 1 public rated game.
3. The status page reports the engine and model service as running.

Main success flow:
1. The player enters their Lichess username, a time control, and the number of games to analyze (1–50).
2. The system confirms the account exists and shows how many matching games are available.
3. The player confirms the import.
4. The system imports the games, shows "n of N imported" as it goes, scores every move, and lists each game's 3 highest-risk moves.
5. The player selects a flagged move.
6. The system opens the replay at that position with its risk score, at least 2 plain-language risk factors, and the engine's best move.

Alternate flow A1 (at step 1): The player uploads a PGN file of 1–50 games instead of entering a username. The system reports how many games were read and how many were skipped as invalid, then continues at step 4.

Exception flow E1 (at step 4): Lichess refuses a request because the rate limit was exceeded. The system stops the import, keeps every game saved so far, and shows a message that the import was interrupted. When the player resumes the import later, the system continues from the last saved game.

Postcondition: Every imported game and a 0–100 risk score for every move are stored under the player's account, with no duplicate games.

## Acceptance Criteria

AC-01.1 (main flow)  
Given a signed-in player whose Lichess account has 20 or more public rated games,  
When the player confirms analysis of their 20 most recent games,  
Then all 20 games are listed with their 3 highest-risk moves within 180 seconds of confirming.

AC-01.2 (main flow)  
Given an analyzed game is open in replay,  
When the player selects a move scored 70 or higher,  
Then the board shows that exact position and at least 2 risk factors, each 25 words or fewer and containing no internal feature names.

AC-01.3 (exception flow E1)  
Given an import of 30 games is in progress,  
When Lichess refuses a request because the rate limit was exceeded,  
Then the import stops, every game saved before the refusal is still viewable, a message appears within 2 seconds, and resuming later re-downloads 0 already-saved games.
