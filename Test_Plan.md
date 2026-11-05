# Chess Blunder-Risk Analyzer Test Plan

Braden Hayes and Nate Poppe

## 1. Test scope

We will test the app during implementation in spring 2027. Nate will lead testing. Each of us writes the cases for the components we own.

| Requirements | Component and owner |
|---|---|
| REQ-01, NFR-06 | Nate: Authentication. Test Lichess sign-in, missing or bad sessions, and cookie expiry. |
| REQ-02, REQ-03, REQ-05, REQ-06, REQ-08, REQ-15, REQ-16, CON-05 | Nate: Game Import. Test input limits, PGN uploads, progress, rate limits, resume, stalled jobs, and refusing imports when a service is down. |
| REQ-04, REQ-07, REQ-13, NFR-02, CON-02 | Nate: Storage. Test which games get saved, duplicates, model versions, and keeping each player's data separate. |
| REQ-09, REQ-14, REQ-17, NFR-01, NFR-08 | Braden: Feature Extraction and Model. Test scoring, forced moves, missing data, repeat runs, engine failures, and version comparison. |
| REQ-12, REQ-19, CON-03, CON-04 | Braden: Risk Explanation. Test factor rules, score labels, and whether factors are accurate chess advice. |
| REQ-10, REQ-11, NFR-05, NFR-07 | Nate: User Interface. Test game order, replay, read speed, and first-time users. |
| NFR-03, NFR-04, CON-01, CON-06 | Both: Deployment. Test install, 20-game timing, free tools, and licenses. |

REQ-18 (Patterns page) and NFR-09 (advanced logging) are deferred since they are Could features. The Won't items (W-01 to W-08) are not tested, such as live-game scores, a browser overlay, and chess.com imports. We test how our app uses Stockfish and the Lichess API, but not their internal behavior.

## 2. Verification and validation strategy

We will verify each requirement by comparing results with known expected values. Most cases use fixture games where we already know the right positions, scores, and counts. For errors, we cause the failure on purpose (a Lichess 429, an engine crash, a missing model file) and check what the app keeps and shows afterward.

Five first-time users will test NFR-07 by signing in, importing 10 games, and opening a flagged move without help. Our advisor will review whether the scores and explanations make sense, and Braden will check that the factors are accurate for each position.

For the model, we will compare a linear baseline with one other model on the same held-out games. The metrics are Brier score, log loss, and average precision. Each model runs three times with seeds 17, 42, and 73, and training and test games never overlap. The target (proposed NFR-10) needs advisor approval on Nov. 13.

| Layer | Approx. count | Coverage focus |
|---|---|---|
| Unit | 13 | Input limits, score mapping, ranking, factor rules, labels. |
| Integration | 20 | Sign-in, storage, duplicates, player separation, versions, import errors. |
| System | 11 | Replay, progress, repeat runs, install, timing, read speed. |
| Acceptance | 5 | Live Lichess import, five users, model review. |

## 3. Measurable success criteria

| Metric | Target | How we will measure it | Requirement |
|---|---|---|---|
| Sign-in | 100% of requests without a valid session rejected. | Call each protected route with no, expired, and tampered cookies. | REQ-01 |
| Import limits | Bad input rejected before any Lichess call. | Submit boundary values and count Lichess calls. | REQ-02 |
| Eligible games | Only finished standard games with 2+ moves saved. Zero engine calls on live games. | Import a mix of eligible and ineligible games. | REQ-04, CON-02 |
| Duplicates | Zero duplicate games per player. | Count rows after repeat imports. | REQ-07 |
| Scoring | Every non-forced move gets one score from 0 to 100. | Feed known probabilities and read the scores. | REQ-09 |
| Games and replay | Newest first, top 3 moves each, exact board and score shown. | Compare the page with seeded data. | REQ-10, REQ-11 |
| Factors | Every score of 70+ shows 2 or 3 factors of 25 words or fewer. | Count factors and words. | REQ-12 |
| Versions | Every score points to a real model version. Old scores kept. | Read scores by version. | REQ-13 |
| Player separation | Zero records shared between two test players. | Request one player's data as the other. | NFR-02 |
| 20-game timing | 180 seconds or less in 3 of 3 runs. | Time from confirming the import until results show. | NFR-04 |
| Read speed | At least 950 of 1,000 reads take 500 ms or less during an import. | Time each request with a script. | NFR-05 |
| Usability | At least 4 of 5 users finish unaided in 5 minutes or less. | Time each user. | NFR-07 |
| Engine failures | One retry on the failed position. Every other move scored. | Force an engine crash. | NFR-08 |

## 4. Test cases

Start each case with the setup in section 5. IDs match the Week 9 traceability matrix. Letters (A to C) split one matrix ID into three cases.

| ID | Description | Steps | Expected Result | Status |
|---|---|---|---|---|
| TC-01-A | Sign in (REQ-01) | 1. Sign in as A through the test Lichess login. 2. Open the Games page. | A lands on the Import page with a session cookie. Games loads. | Not Run |
| TC-01-B | No session (REQ-01) | 1. Clear cookies. 2. Call each protected route. | Every call returns 401. Nothing is saved. | Not Run |
| TC-01-C | Bad session (REQ-01) | 1. Retry with an expired cookie, then an edited one. 2. Send the login callback with the wrong state value. | All rejected. No session is created. | Not Run |
| TC-02-A | Valid imports (REQ-02) | 1. Import with usernames of 2 and 30 characters, counts of 1 and 50, and each of the 4 time controls. | All accepted with the requested count. | Not Run |
| TC-02-B | Bad game count (REQ-02) | 1. Submit counts of 0, 51, blank, and "abc" one at a time. 2. Check the Lichess call log. | Each is rejected with an error naming the count. Zero Lichess calls. | Not Run |
| TC-02-C | Bad username or time control (REQ-02) | 1. Submit usernames of 1 and 31 characters, "bad!", and blank. 2. Submit time control "unknown". | Each is rejected with an error naming the field. Zero Lichess calls. | Not Run |
| TC-03 | Game count preview (REQ-03) | 1. Set the test Lichess to have 12 games for A and enter A on Import. 2. Enter a username that doesn't exist. | Shows 12 before confirming. The fake name shows "player not found" and no import starts. | Not Run |
| TC-04-A | Finished games (REQ-04) | 1. Import a win, a loss, and a draw from F-elig. | All 3 are saved. Only A's moves are analyzed. | Not Run |
| TC-04-B | Live game (REQ-04, CON-02) | 1. Import an ongoing game from Lichess. 2. Upload the same game as a PGN. 3. Check the database and engine log. | Both are rejected. Nothing saved and zero engine calls. | Not Run |
| TC-04-C | Wrong variant or too short (REQ-04) | 1. Import one non-standard game, a 0-move game, a 1-move game, and one valid game together. | Only the valid game is saved and analyzed. | Not Run |
| TC-05 | PGN limits (REQ-05) | 1. Upload files with 1, 50, and 51 games. 2. Upload one file at exactly 5 MB and one just over. 3. Upload 2 good games and 1 broken one. | 1, 50, and 5 MB accepted. 51 and over 5 MB rejected. The mixed file reports 3 read, 2 imported, 1 skipped. | Not Run |
| TC-06 | Import progress (REQ-06) | 1. Send 10 games, one per second. 2. Watch the counter. 3. Leave the page halfway and come back. | "n of 10" updates at least every 2 seconds. The import keeps going and ends at 10. | Not Run |
| TC-07-A | Same import twice (REQ-07) | 1. Import F20 as A and note the row counts. 2. Import F20 again. | Row counts don't change. A has exactly 20 games. | Not Run |
| TC-07-B | Overlapping resume (REQ-07) | 1. Save games 1 to 5 of F20. 2. Resume with games 5 to 20. | Exactly 20 games. Game 5 has one set of moves. | Not Run |
| TC-07-C | Same game at the same time (REQ-07) | 1. Insert the same game for A from two transactions at once. | One game with one full move set. No partial rows. | Not Run |
| TC-08 | Rate limit and resume (REQ-08, CON-05) | 1. Make Lichess return a 429 after 5 games. 2. Wait 60 seconds, then resume. 3. Check the request log. | Interrupted message within 2 seconds. No automatic retry and never more than 1 request at a time. Resume skips the 5 saved games and ends at 20. | Not Run |
| TC-09-A | Probability to score (REQ-09) | 1. Set the model to return 0, 0.204, 0.70, and 1.0 for four moves. 2. Read the saved scores. | Scores are 0, 20, 70, and 100, one per move. | Not Run |
| TC-09-B | Forced move (REQ-09) | 1. Analyze the F-forced position, which has one legal move. | Saved as "forced" with no score and no engine call. | Not Run |
| TC-09-C | Model error (REQ-09) | 1. Make the model fail on 1 of 3 moves. | The failed move has no score. The other 2 are scored and the error is logged. | Not Run |
| TC-10-A | Order and top 3 (REQ-10) | 1. Seed games from Jan. 1, 2, and 3, each with moves scored 20, 90, 70, and 80. 2. Open Games. | Games show Jan. 3, 2, 1. Each shows 90, 80, 70. | Not Run |
| TC-10-B | Fewer than 3 scores (REQ-10) | 1. Seed games with 0, 1, and 2 scored moves. 2. Open Games. | They show 0, 1, and 2 moves. Nothing repeated or made up. | Not Run |
| TC-10-C | Tied scores (REQ-10) | 1. Seed moves 2, 4, 6, and 8 with scores 90, 80, 80, and 80. | Shows moves 2, 4, and 6. The earlier move wins a tie. | Not Run |
| TC-11-A | Correct board (REQ-11) | 1. Replay F-board moves after castling, en passant, and promotion. 2. Compare with the saved position. | Pieces, side to move, and castling rights match exactly. | Not Run |
| TC-11-B | Score and best move (REQ-11) | 1. Click each scored move in an F-board game. | Score and highlighted engine move match the database. | Not Run |
| TC-11-C | Fast clicking (REQ-11) | 1. Delay move X's response by 2 seconds. 2. Click X, then Y right away. 3. Wait 3 seconds. | The board still shows Y. X's late response is ignored. | Not Run |
| TC-12-A | High-risk factors (REQ-12, CON-04) | 1. Generate explanations for 20 seeded moves scoring 70+ and one scoring 69. 2. Count factors and words. 3. Braden checks each factor against the position. | Every 70+ move has 2 or 3 accurate factors of 25 words or fewer. The 69 needs none. | Not Run |
| TC-12-B | Bad factor text (REQ-12) | 1. For a score of 80, give a 26-word factor, one containing "clock_ratio", and two valid ones. | Only the 2 valid factors show. | Not Run |
| TC-12-C | Missing clock data (REQ-12) | 1. Mark clock and rating as missing for a score of 80. 2. Generate the explanation. | No factor mentions clock or rating. Still 2 or 3 valid factors. | Not Run |
| TC-13-A | Read by version (REQ-13) | 1. Seed V1 and V2 scores for the same moves. 2. Request each version, then no version. | Each request returns that version. No version returns the active one. | Not Run |
| TC-13-B | Switch mid-import (REQ-13) | 1. Start an import on V1. 2. Make V2 active halfway through. | The whole import stays on V1. Old V1 scores don't change. | Not Run |
| TC-13-C | Missing version (REQ-13) | 1. Save a score with a fake model ID. 2. Request a version that doesn't exist. | The database rejects the score. The request returns not found. | Not Run |
| TC-14 | Compare versions (REQ-14) | 1. Analyze F20 with V1. 2. Run the V2 comparison with the engine log on. | V1 and V2 show side by side on the same moves. Zero new engine calls. | Not Run |
| TC-15 | Service down (REQ-15) | 1. Start the app without the model file. 2. Check status and try an import. 3. Repeat with a bad Stockfish path. | Status names what's down. The import is refused with a message naming it. | Not Run |
| TC-16 | Stalled import (REQ-16) | 1. Set a running import's last save to 299 seconds ago, then 360. 2. Restart the app with an import still running. | Not touched at 299. Marked interrupted by 360 and after restart. Saved games kept. | Not Run |
| TC-17 | Missing PGN data (REQ-17) | 1. Upload F-missing games with no clocks, no ratings, and no time control. | Games still scored. Filled-in values are flagged. | Not Run |
| TC-19 | Score labels (REQ-19, CON-03) | 1. Check raw model outputs for F20. 2. Open Games and Replay and check every score. | Outputs are between 0 and 1. Every score says "estimated blunder risk" and Replay has the probability note. | Not Run |
| TC-20 | Same results (NFR-01) | 1. Analyze F20 three times, clearing the cache and restarting between runs. | Best moves, evaluations, and scores match in all 3 runs. | Not Run |
| TC-21-A | Own data (NFR-02) | 1. Seed games for A and B. 2. Sign in as each and list games. | Each player sees only their own games. | Not Run |
| TC-21-B | Other player's data (NFR-02) | 1. As A, request B's import and game and try to resume B's import. | All return not found. B's data doesn't change. | Not Run |
| TC-21-C | Private vs. missing (NFR-02) | 1. As A, request B's game ID and a random fake ID. | Both return the same not-found response. | Not Run |
| TC-22 | Fresh install (NFR-03, CON-01, CON-06) | 1. On a clean staging computer with no GPU, follow the user guide and time it. 2. Check dependencies for paid services and license notices. | Running in 30 minutes or less with one `docker compose up`. Zero paid services and GPL-3.0 notices included. | Not Run |
| TC-23-A | 20-game timing (NFR-04) | 1. Clear the staging database and cache. 2. Import F20 and time it. 3. Repeat twice. | All 20 games and top 3 moves show in 180 seconds or less in 3 of 3 runs. | Not Run |
| TC-23-B | Real Lichess account (NFR-04) | 1. On pilot, import 20 games from a real test account three times. | 180 seconds or less each run. A Lichess outage marks the run Blocked. | Not Run |
| TC-23-C | Long games (NFR-04) | 1. Repeat TC-23-A with F20-long. | 180 seconds or less in 3 of 3 runs. No games skipped. | Not Run |
| TC-24 | Reads during import (NFR-05) | 1. Seed 50 analyzed games. 2. Run an import while sending 1,000 Games requests. | At least 950 succeed in 500 ms or less. | Not Run |
| TC-25 | Sessions and secrets (NFR-06) | 1. Sign in and inspect the cookie. 2. Move the clock 7 days ahead. 3. Search the logs for cookies and keys. | Cookie is signed and HttpOnly and expires by 7 days. Zero secrets in the logs. | Not Run |
| TC-26 | First-time users (NFR-07) | 1. Give each of five users only the task. 2. Time sign-in, a 10-game import, and opening a flagged move. | At least 4 of 5 finish unaided in 5 minutes or less. | Not Run |
| TC-27 | Engine crash (NFR-08) | 1. Make the engine crash twice on one F20 position. | That position is retried once, then marked failed. Every other move is scored. | Not Run |
| TC-35 | Model comparison (proposed NFR-10) | 1. Train the baseline and the candidate with seeds 17, 42, and 73. 2. Score the held-out games. | The candidate beats a constant guess on Brier score and log loss, and beats the blunder rate on average precision, in all 3 runs. | Not Run |

## 5. Test environment

| Environment | Setup |
|---|---|
| Development | Our laptops with current versions of Ubuntu (or WSL), Python, Node, PostgreSQL, Stockfish, Chrome, and Firefox. Confirm versions at S1. Use a separate test database and a fake Lichess API to cause errors like a 429 or a dropped connection. |
| Staging | The same build in Docker Compose on a 4-core, 16 GB computer with no GPU and at most 3 engine workers. Clear the database and cache before each timed run. |
| Pilot | The staging build with real Lichess sign-in. Use it for TC-23-B and the five-user test. Reset the test account before each user. |

Before each case, reset the test database and create two test players, A and B. F20 has 20 finished games of 40 to 80 moves. F20-long has 20 games of 100 to 160 moves. F-elig has ongoing, non-standard, and very short games. F-board has castling, en passant, and promotion. F-forced has one legal move. F-missing leaves out clocks, ratings, or time control. All fixture files are committed to the repo.

Before each round of tests, record the software versions, engine depth, model version, and seed in `test_manifest.json`.

## 6. Pass and fail criteria

Mark a case Pass if every expected result is met. Mark it Fail if any result or time limit is missed, any data is lost, or one player can see another player's data. Use Blocked when something prevents the test from running and Not Run before the first attempt.

The plan passes when all 49 cases pass and the advisor has approved the model target. The timing tests must pass in 3 of 3 runs. A failed, blocked, or unrun case keeps the plan from passing.

## 7. Test schedule

| Milestone | Test activity | Owner | Evidence |
|---|---|---|---|
| F1, Nov. 13 | Agree on blunder labels, data split, and model target with the advisor. | Braden | Meeting notes. |
| S1-S2, Jan. 22 to Feb. 5 | Unit and integration tests for sign-in, import limits, scoring, storage, and player separation. | Nate: auth and storage. Braden: scoring. | Test results and saved fixture data. |
| S3, Feb. 19 | Test resume, stalled imports, and missing data. Run the first model comparison. | Nate: import. Braden: model. | Request logs and model results. |
| S4-S5, Mar. 5 to 12 | System tests for replay, progress, repeat runs, and sessions. | Nate: UI. Braden: repeat runs. | Screenshots and test results. |
| S6, Apr. 2 | Factor tests and the five-user test. | Braden: factors. Nate: users. | Factor review notes and user times. |
| S7-S8, Apr. 16 to 23 | Full retest, timing and read-speed tests, clean install, and sign-off. | Both | Timing logs, defect list, and sign-off. |

## 8. Definition of done

**Feature checklist**

- [ ] The component tests pass, including the edge or failure case.
- [ ] The other team member has reviewed the change and its documentation.
- [ ] Any defect that keeps the feature from meeting its requirement is fixed and retested.

**Project checklist**

- [ ] All 49 cases pass and the results are recorded.
- [ ] The install steps, test plan, and traceability matrix are up to date.
- [ ] Both team members have reviewed the open issues and signed off.

Sign-off: Nate Poppe ________ Braden Hayes ________ Date ________

AI use: An AI tool was used to improve the wording, format this document, and generate the PDF from Markdown.
