# Chess Blunder Predictor - Delivery Plan

Braden Hayes and Nate Poppe. Baseline: 2026-10-28.


## 1. Task List

# Task List

Project: Chess Blunder Predictor (Chess Blunder-Risk Analyzer in D0-D2).

T1. Specify the Blunder-Risk Model training-data source, blunder label, prediction-time features, and evaluation criteria with the advisor - Owner: Braden Hayes.

T2. Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, startup validation, and contextual error logs - Owner: Nate Poppe.

T3. Develop Game and Results Storage with database migrations, per-game transactions, duplicate prevention, and model-versioned scores - Owner: Nate Poppe.

T4. Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels, missing-data rules, and leakage-controlled training, validation, and test splits - Owner: Braden Hayes.

T5. Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, bounded parallel workers, and failure recovery - Owner: Braden Hayes.

T6. Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks, progress tracking, duplicate prevention, and interrupted-import recovery - Owner: Nate Poppe.

T7. Develop and evaluate a baseline and candidate Blunder-Risk Model using the agreed validation criteria and probability-calibration checks - Owner: Braden Hayes.

T8. Develop the User Interface sign-in and import pages with input validation, availability counts, progress messages, and resume controls - Owner: Nate Poppe.

T9. Develop Blunder-Risk Model batch inference with feature-schema validation, missing-value preprocessing, versioned model loading, and isolated move failures - Owner: Braden Hayes.

T10. Develop the User Interface Games and Replay pages with the three highest-risk moves, exact board positions, risk scores, explanations, and engine-best moves - Owner: Nate Poppe.

T11. Develop Risk Explanation and Analytics with stored plain-language risk factors, rule-based fallbacks, and player-pattern aggregation after core explanations pass verification - Owner: Braden Hayes.

T12. Test User Interface, Game Import and Validation, and Game and Results Storage together for end-to-end workflows, interrupted imports, versioned results, and user-data isolation - Owner: Nate Poppe.

T13. Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out prediction results, reproducibility checks, explanation constraints, and the 20-game analysis target - Owner: Braden Hayes.

T14. Document and validate Configuration and User Interface deployment and operation with a reproducible local setup, demo dataset, and user guide - Owner: Nate Poppe.


## 2. Milestones List

All course deliverables preceding this planning submission (C1-C7) are complete. Canvas dates are due dates, not actual completion dates. Team targets are not published course deadlines.

| ID | Date | Status | Basis | Milestone | Demonstrates | Verification | Verifier |
| --- | --- | --- | --- | --- | --- | --- | --- |
| C1 | 2026-09-02 | Complete | Canvas | Professional Biography | Introduces each member and relevant skills. | Check both submitted biographies and Canvas receipts. | Instructor; each member |
| C2 | 2026-09-09 | Complete | Canvas | Team Formation and Project Description | Records the two-person team and project purpose. | Compare the roster and supplied project-description.md with the submission. | Instructor; both members |
| C3 | 2026-09-18 | Complete | Canvas | Advisor Selection and Design Constraints Essay | Identifies the advisor and the constraints governing design. | Check advisor confirmation and the submitted constraints essay. | Instructor; advisor |
| C4 | 2026-09-28 | Complete | Canvas | User Stories and Use Cases | Defines the user workflow and acceptance criteria. | Check submitted stories and criteria referenced by D2. | Instructor; both members |
| C5 | 2026-10-02 | Complete | Canvas | High-Level System Design (Design D0) | Defines six system modules, interfaces, and primary owners. | Inspect the supplied D0 artifact and its repository version. | Instructor; advisor |
| C6 | 2026-10-08 | Complete | Canvas | Detailed System Design, Part 1 (Design D1) | Defines storage, import, feature extraction, and API contracts. | Inspect the supplied D1 artifact and its diagrams. | Instructor; advisor |
| C7 | 2026-10-14 | Complete | Canvas | Detailed System Design, Part 2 (Design D2) and Peer Review Notes | Closes the design with model/UI behavior, edge cases, and review responses. | Inspect supplied D2 and its peer-review section; confirm separate notes in the repo. | Instructor; advisor |
| C8 | 2026-10-28 | Planned | Canvas | Task List, Milestones, Timeline, and Effort Matrix | Establishes this consistent, resourced delivery baseline. | Submit the six-part PDF; verify Tasklist.md and Planning sources in the repo. | Instructor; both members |
| C9 | 2026-10-30 | Planned | Canvas | Requirements Document and Technical Specification Document | Freezes completed-game scope and measurable system contracts. | Compare requirements to D0-D2; resolve live-score wording and record open ML decisions. | Instructor; advisor |
| C10 | 2026-11-04 | Planned | Canvas | Test and Validation Plan Draft | Maps requirements to fixtures, tests, metrics, and acceptance evidence. | Review test cases for timing, resume, isolation, model versions, and held-out evaluation. | Instructor; both members |
| C11 | 2026-11-06 | Planned | Team target | Ethics review package (roadmap Week 11) | Documents fair play, data handling, and limits of predicted probabilities. | Advisor reviews completed-game-only policy, data provenance, and uncertainty wording. | Advisor; both members |
| C12 | 2026-11-13 | Planned | Team target | Communication package (roadmap Week 12) | Makes the project understandable through a short narrative and draft slides. | Nate checks usability narrative; Braden checks modeling claims; advisor reviews. | Advisor; both members |
| C13 | 2026-11-18 | Planned | Team target | Oral presentation readiness 1 (roadmap Week 13) | Demonstrates a coherent explanation of the design and delivery plan. | Run a timed rehearsal and record feedback plus assigned corrections. | Advisor; both members |
| C14 | 2026-11-25 | Planned | Team target | Oral presentation readiness 2 (roadmap Week 14) | Shows that rehearsal feedback has been incorporated. | Repeat the timed presentation; close major feedback items. | Instructor; advisor |
| C15 | 2026-11-25 | Planned | Team target | Week 14 fall report | Preserves the design baseline and reproduces the current planning tables. | Check all six components, identical task IDs, updated status, and source/PDF agreement. | Instructor; advisor |
| C16 | 2026-12-08 | Planned | Canvas | Project Advisor Assessments | Provides the advisor with evidence of fall design progress and readiness. | Prepare evidence by Dec 4; advisor completes the course assessment by Dec 8. | Advisor; instructor |
| F1 | 2026-11-13 | Planned | Internal | Modeling protocol agreed | Resolves data source, label, pre-move feature boundary, and evaluation protocol. | Advisor reviews T1 decision log, sample rows, and proposed model-selection criteria. | Advisor; Nate Poppe |
| S1 | 2027-01-22 | Planned | Internal | Startup and engine feasibility demonstrated | Authentication/configuration/logging run; fixed settings produce features on sample PGNs. | Run T2/T5 smoke tests and early 20-game timing probe; record hardware and engine settings. | Both members; advisor reviews evidence |
| S2 | 2027-02-05 | Planned | Internal | Stored-game and training-data baseline ready | Schema and a bounded training dataset support repeatable development. | Apply migrations on a fresh database; audit label samples, missingness, and split leakage. | Both members; advisor reviews data protocol |
| S3 | 2027-02-19 | Planned | Internal | Resumable import and candidate model demonstrated | Validated games survive interruption and a candidate improves on the agreed baseline. | Inject an import interruption; inspect duplicate counts and validation metrics/calibration. | Both members; advisor reviews model evidence |
| S4 | 2027-03-05 | Planned | Internal | Import-to-score pipeline demonstrated | The import UI connects to versioned inference and displays correct progress. | Run completed-game import through stored scores; reject incompatible feature batches. | Both members |
| S5 | 2027-03-12 | Planned | Internal | Pre-break baseline frozen | A tagged working baseline can be resumed after midterms and spring break. | Run smoke tests, back up DB/model artifacts, and document outstanding defects. | Both members |
| S6 | 2027-04-02 | Planned | Internal | Replay and explanations demonstrated | Replay shows the selected board, top-risk moves, and readable factors. | Verify exact FENs and at least two supported factors of at most 25 words on acceptance fixtures. | Both members; advisor reviews demonstration |
| S7 | 2027-04-16 | Planned | Internal | Acceptance candidate approved | Core requirements pass integrated, statistical, and performance checks. | Run the acceptance suite and held-out evaluation; time 20 games against 180 seconds; close release blockers. | Both members; advisor reviews evidence |
| S8 | 2027-04-23 | Planned | Internal | Final implementation and handoff ready | A repeatable local release, user guide, evaluation report, and demo are ready before finals. | Install on a clean environment, rehearse the demo, verify artifact hashes and documentation. | Advisor; both members |


## 3. Table 1 - Timeline

Each task includes 20% effort contingency within its dated work window, followed by a separate protected gate block.

| ID | Statement | Owner | Start | Completion | Dependencies | Base hours | 20% contingency | Budget hours | Path | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| T1 | Specify the Blunder-Risk Model training-data source, blunder label, prediction-time features, and evaluation criteria with the advisor. | Braden Hayes | 2026-10-29 | 2026-11-10 | C7 | 12 | 2.4 | 14.4 | Prerequisite | R1, R2 |
| T2 | Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, startup validation, and contextual error logs. | Nate Poppe | 2027-01-11 | 2027-01-20 | C7 | 14 | 2.8 | 16.8 | App path | R3, R7 |
| T3 | Develop Game and Results Storage with database migrations, per-game transactions, duplicate prevention, and model-versioned scores. | Nate Poppe | 2027-01-25 | 2027-02-03 | T2 | 14 | 2.8 | 16.8 | App path | R7 |
| T4 | Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels, missing-data rules, and leakage-controlled training, validation, and test splits. | Braden Hayes | 2027-01-25 | 2027-02-03 | T1, T5 | 16 | 3.2 | 19.2 | Model path | R1, R2 |
| T5 | Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, bounded parallel workers, and failure recovery. | Braden Hayes | 2027-01-11 | 2027-01-20 | T1 | 16 | 3.2 | 19.2 | Model path | R4 |
| T6 | Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks, progress tracking, duplicate prevention, and interrupted-import recovery. | Nate Poppe | 2027-02-08 | 2027-02-17 | T2, T3 | 18 | 3.6 | 21.6 | App path | R3, R7 |
| T7 | Develop and evaluate a baseline and candidate Blunder-Risk Model using the agreed validation criteria and probability-calibration checks. | Braden Hayes | 2027-02-08 | 2027-02-17 | T4 | 16 | 3.2 | 19.2 | Model path | R1, R2 |
| T8 | Develop the User Interface sign-in and import pages with input validation, availability counts, progress messages, and resume controls. | Nate Poppe | 2027-02-22 | 2027-03-03 | T2, T6 | 14 | 2.8 | 16.8 | App path | R3 |
| T9 | Develop Blunder-Risk Model batch inference with feature-schema validation, missing-value preprocessing, versioned model loading, and isolated move failures. | Braden Hayes | 2027-02-22 | 2027-03-03 | T3, T5, T7 | 15 | 3.0 | 18.0 | Model path | R2, R7 |
| T10 | Develop the User Interface Games and Replay pages with the three highest-risk moves, exact board positions, risk scores, explanations, and engine-best moves. | Nate Poppe | 2027-03-22 | 2027-03-31 | T3, T8, T9 | 16 | 3.2 | 19.2 | App path | R5, R7 |
| T11 | Develop Risk Explanation and Analytics with stored plain-language risk factors, rule-based fallbacks, and player-pattern aggregation after core explanations pass verification. | Braden Hayes | 2027-03-22 | 2027-03-31 | T9 | 15 | 3.0 | 18.0 | Model path | R2, R5 |
| T12 | Test User Interface, Game Import and Validation, and Game and Results Storage together for end-to-end workflows, interrupted imports, versioned results, and user-data isolation. | Nate Poppe | 2027-04-05 | 2027-04-14 | T6, T8, T10, T11 | 16 | 3.2 | 19.2 | App path | R3, R7 |
| T13 | Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out prediction results, reproducibility checks, explanation constraints, and the 20-game analysis target. | Braden Hayes | 2027-04-05 | 2027-04-14 | T5, T7, T9, T10, T11 | 16 | 3.2 | 19.2 | Model path | R2, R4 |
| T14 | Document and validate Configuration and User Interface deployment and operation with a reproducible local setup, demo dataset, and user guide. | Nate Poppe | 2027-04-19 | 2027-04-21 | T12, T13, S7 | 10 | 2.0 | 12.0 | Release path | R6, R7, R8 |


### Table 1 - Milestone rows

| ID | Milestone | Completion date | Status | Basis | Protected block |
| --- | --- | --- | --- | --- | --- |
| C1 | Professional Biography | 2026-09-02 | Complete | Canvas | Historical |
| C2 | Team Formation and Project Description | 2026-09-09 | Complete | Canvas | Historical |
| C3 | Advisor Selection and Design Constraints Essay | 2026-09-18 | Complete | Canvas | Historical |
| C4 | User Stories and Use Cases | 2026-09-28 | Complete | Canvas | Historical |
| C5 | High-Level System Design (Design D0) | 2026-10-02 | Complete | Canvas | Historical |
| C6 | Detailed System Design, Part 1 (Design D1) | 2026-10-08 | Complete | Canvas | Historical |
| C7 | Detailed System Design, Part 2 (Design D2) and Peer Review Notes | 2026-10-14 | Complete | Canvas | Historical |
| C8 | Task List, Milestones, Timeline, and Effort Matrix | 2026-10-28 | Planned | Canvas | B0 |
| C9 | Requirements Document and Technical Specification Document | 2026-10-30 | Planned | Canvas | B1 |
| C10 | Test and Validation Plan Draft | 2026-11-04 | Planned | Canvas | B2 |
| C11 | Ethics review package (roadmap Week 11) | 2026-11-06 | Planned | Team target | B3 |
| C12 | Communication package (roadmap Week 12) | 2026-11-13 | Planned | Team target | B4 |
| C13 | Oral presentation readiness 1 (roadmap Week 13) | 2026-11-18 | Planned | Team target | B5 |
| C14 | Oral presentation readiness 2 (roadmap Week 14) | 2026-11-25 | Planned | Team target | B6 |
| C15 | Week 14 fall report | 2026-11-25 | Planned | Team target | B6 |
| C16 | Project Advisor Assessments | 2026-12-08 | Planned | Canvas | B7 |
| F1 | Modeling protocol agreed | 2026-11-13 | Planned | Internal | B4 |
| S1 | Startup and engine feasibility demonstrated | 2027-01-22 | Planned | Internal | B8 |
| S2 | Stored-game and training-data baseline ready | 2027-02-05 | Planned | Internal | B9 |
| S3 | Resumable import and candidate model demonstrated | 2027-02-19 | Planned | Internal | B10 |
| S4 | Import-to-score pipeline demonstrated | 2027-03-05 | Planned | Internal | B11 |
| S5 | Pre-break baseline frozen | 2027-03-12 | Planned | Internal | B12 |
| S6 | Replay and explanations demonstrated | 2027-04-02 | Planned | Internal | B13 |
| S7 | Acceptance candidate approved | 2027-04-16 | Planned | Internal | B14 |
| S8 | Final implementation and handoff ready | 2027-04-23 | Planned | Internal | B15 |


### Table 1 - Protected blocks and constraints

| ID | Start | End | Supports | Kind | Purpose |
| --- | --- | --- | --- | --- | --- |
| B0 | 2026-10-28 | 2026-10-28 | C8 | Release check today | Final consistency and export check; no retrospective buffer is claimed. |
| B1 | 2026-10-29 | 2026-10-29 | C9 | Protected review | Scope wording and requirement/specification review; T1 offers its scope decision before its full completion. |
| B2 | 2026-11-02 | 2026-11-03 | C10 | Protected review | Review the test-plan draft; no new feature commitments in this block. |
| B3 | 2026-11-05 | 2026-11-05 | C11 | Protected review | Review the ethics package and fair-play boundary. |
| B4 | 2026-11-12 | 2026-11-12 | C12, F1 | Protected review | Review communication materials and the T1 modeling protocol after the Nov 11 holiday. |
| B5 | 2026-11-16 | 2026-11-17 | C13 | Protected rehearsal | Presentation corrections and rehearsal only. |
| B6 | 2026-11-23 | 2026-11-24 | C14, C15 | Protected report/rehearsal | Freeze tables, resolve report inconsistencies, rehearse; preserve Thanksgiving Nov 26-29. |
| B7 | 2026-12-03 | 2026-12-04 | C16 | Protected evidence check | Send-ready assessment evidence prepared before finals; no scheduled exam-week development. |
| B8 | 2027-01-21 | 2027-01-21 | S1 | Protected gate buffer | Startup/engine fixes and evidence review only; reserve 1 hour per member. |
| B9 | 2027-02-04 | 2027-02-04 | S2 | Protected gate buffer | Schema/data fixes and evidence review only; reserve 1 hour per member. |
| B10 | 2027-02-18 | 2027-02-18 | S3 | Protected gate buffer | Import/model corrections only; reserve 1 hour per member. |
| B11 | 2027-03-04 | 2027-03-04 | S4 | Protected gate buffer | Integration fixes only; reserve 1 hour per member. |
| B12 | 2027-03-11 | 2027-03-11 | S5 | Protected freeze buffer | Backup/recovery checks and defects only; reserve 1 hour per member. |
| B13 | 2027-04-01 | 2027-04-01 | S6 | Protected gate buffer | Replay/explanation fixes only; reserve 1 hour per member. |
| B14 | 2027-04-15 | 2027-04-15 | S7 | Protected acceptance buffer | Release-blocking fixes only; reserve 1 hour per member. |
| B15 | 2027-04-22 | 2027-04-22 | S8 | Protected release buffer | Clean-install/demo corrections only; reserve 1 hour per member. |
| K1 | 2026-11-26 | 2026-11-29 | R6 | Thanksgiving blackout | No planned project work. |
| K2 | 2026-12-06 | 2026-12-12 | R6, C16 | Fall exams | No development; advisor assessment remains due Dec 8 using evidence prepared Dec 4. |
| K3 | 2026-12-13 | 2027-01-10 | R6 | Winter blackout | No planned development; restart Jan 11. |
| K4 | 2027-01-18 | 2027-01-18 | R6 | University holiday | No required work; T2/T5 budgets fit the remaining days. |
| K5 | 2027-03-08 | 2027-03-12 | R6, S5 | Assumed midterm reserve | Capacity reduced to 6 hours per member; no new tasks; freeze and repair existing work. |
| K6 | 2027-03-15 | 2027-03-21 | R6 | Spring break blackout | No planned project work; T10/T11 start after the break. |
| K7 | 2027-04-24 | 2027-04-29 | R6 | Spring finals blackout | No planned development; release ready Apr 23. |
| K8 | 2027-01-11 | 2027-04-23 | R6 | Spring availability constraint | Braden: 18 credits. Nate: expected 15 credits plus an assumed 20 co-op hours/week. Budget 12 project hours/week each, including coordination and reserves. |


### Critical paths

Model: T5 -> T4 -> T7 -> T9 -> T11 -> T13 -> T14 -> S8. Application: T2 -> T3 -> T6 -> T8 -> T10 -> T12 -> T14 -> S8. T1 is the fall model prerequisite. Both paths are critical to fixed milestone gates after resource and blackout constraints. T10/T11 develop in parallel with shared contract fixtures; integration waits for both. T5 uses local fixtures and T4 uses offline files. T1 provides the scope decision for C9 by Oct 29, before its full protocol is complete.


## 4. Table 2 - Effort Matrix

Hours include 20% contingency. P denotes primary responsibility.

| ID | Statement | Braden Hayes | Nate Poppe | Row total |
| --- | --- | --- | --- | --- |
| T1 | Specify the Blunder-Risk Model training-data source, blunder label, prediction-time features, and evaluation criteria with the advisor. | 12.0 P | 2.4 | 14.4 |
| T2 | Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, startup validation, and contextual error logs. | 2.4 | 14.4 P | 16.8 |
| T3 | Develop Game and Results Storage with database migrations, per-game transactions, duplicate prevention, and model-versioned scores. | 2.4 | 14.4 P | 16.8 |
| T4 | Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels, missing-data rules, and leakage-controlled training, validation, and test splits. | 16.8 P | 2.4 | 19.2 |
| T5 | Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, bounded parallel workers, and failure recovery. | 16.8 P | 2.4 | 19.2 |
| T6 | Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks, progress tracking, duplicate prevention, and interrupted-import recovery. | 3.6 | 18.0 P | 21.6 |
| T7 | Develop and evaluate a baseline and candidate Blunder-Risk Model using the agreed validation criteria and probability-calibration checks. | 16.8 P | 2.4 | 19.2 |
| T8 | Develop the User Interface sign-in and import pages with input validation, availability counts, progress messages, and resume controls. | 2.4 | 14.4 P | 16.8 |
| T9 | Develop Blunder-Risk Model batch inference with feature-schema validation, missing-value preprocessing, versioned model loading, and isolated move failures. | 14.4 P | 3.6 | 18.0 |
| T10 | Develop the User Interface Games and Replay pages with the three highest-risk moves, exact board positions, risk scores, explanations, and engine-best moves. | 2.4 | 16.8 P | 19.2 |
| T11 | Develop Risk Explanation and Analytics with stored plain-language risk factors, rule-based fallbacks, and player-pattern aggregation after core explanations pass verification. | 14.4 P | 3.6 | 18.0 |
| T12 | Test User Interface, Game Import and Validation, and Game and Results Storage together for end-to-end workflows, interrupted imports, versioned results, and user-data isolation. | 4.8 | 14.4 P | 19.2 |
| T13 | Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out prediction results, reproducibility checks, explanation constraints, and the 20-game analysis target. | 15.6 P | 3.6 | 19.2 |
| T14 | Document and validate Configuration and User Interface deployment and operation with a reproducible local setup, demo dataset, and user guide. | 2.4 | 9.6 P | 12.0 |
| TOTAL |  | 127.2 | 122.4 | 249.6 |


### Capacity and rebalancing

Budget 12 project hours per member per working week, including 1 coordination hour. Braden takes 18 credits. Nate is expected to take 15 credits alongside part-time co-op, assumed to be 20 hours/week. Midterms reduce project capacity to 6 hours/week each. Braden carries 4.8 more task hours; if needed, transfer up to 2.4 hours of T13 test execution to Nate while retaining Braden as the largest contributor and primary owner. If Nate instead loses availability, move T12 fixture support to Braden while retaining Nate as the largest contributor.

Fall coursework allowance: 24 hours each. Spring coordination: 14 hours each; protected gates: 8 hours each; pre-break freeze review: up to 2 extra hours each. These are separate from task totals. All estimates are proposed, not measured or owner-elicited PERT estimates.

| Window | Tasks | Braden hours | Nate hours | Capacity each | Overhead/member |
| --- | --- | --- | --- | --- | --- |
| Jan 11-22 | T2, T5 | 22.2 | 19.8 | 24 | 3 |
| Jan 25-Feb 5 | T3, T4 | 22.2 | 19.8 | 24 | 3 |
| Feb 8-19 | T6, T7 | 23.4 | 23.4 | 24 | 3 |
| Feb 22-Mar 5 | T8, T9 | 19.8 | 21.0 | 24 | 3 |
| Mar 22-Apr 2 | T10, T11 | 19.8 | 23.4 | 24 | 3 |
| Apr 5-16 | T12, T13 | 23.4 | 21.0 | 24 | 3 |
| Apr 19-23 | T14 | 4.4 | 11.6 | 12 | 2 |


## 5. Work breakdown structure


### 1. Scope and foundations

T1. Specify the Blunder-Risk Model training-data source, blunder label, prediction-time features, and evaluation criteria with the advisor - Owner: Braden Hayes.

T2. Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, startup validation, and contextual error logs - Owner: Nate Poppe.

T3. Develop Game and Results Storage with database migrations, per-game transactions, duplicate prevention, and model-versioned scores - Owner: Nate Poppe.


### 2. Data and model development

T4. Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels, missing-data rules, and leakage-controlled training, validation, and test splits - Owner: Braden Hayes.

T5. Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, bounded parallel workers, and failure recovery - Owner: Braden Hayes.

T6. Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks, progress tracking, duplicate prevention, and interrupted-import recovery - Owner: Nate Poppe.

T7. Develop and evaluate a baseline and candidate Blunder-Risk Model using the agreed validation criteria and probability-calibration checks - Owner: Braden Hayes.


### 3. Scoring and user experience

T8. Develop the User Interface sign-in and import pages with input validation, availability counts, progress messages, and resume controls - Owner: Nate Poppe.

T9. Develop Blunder-Risk Model batch inference with feature-schema validation, missing-value preprocessing, versioned model loading, and isolated move failures - Owner: Braden Hayes.

T10. Develop the User Interface Games and Replay pages with the three highest-risk moves, exact board positions, risk scores, explanations, and engine-best moves - Owner: Nate Poppe.

T11. Develop Risk Explanation and Analytics with stored plain-language risk factors, rule-based fallbacks, and player-pattern aggregation after core explanations pass verification - Owner: Braden Hayes.


### 4. Validation and delivery

T12. Test User Interface, Game Import and Validation, and Game and Results Storage together for end-to-end workflows, interrupted imports, versioned results, and user-data isolation - Owner: Nate Poppe.

T13. Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out prediction results, reproducibility checks, explanation constraints, and the 20-game analysis target - Owner: Braden Hayes.

T14. Document and validate Configuration and User Interface deployment and operation with a reproducible local setup, demo dataset, and user guide - Owner: Nate Poppe.


## 6. Risk register

| ID | Category | Risk | Likelihood | Impact | Strategy | Mitigation | Owner | Trigger | Table 1 links |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R1 | Technical / external | Training data, labels, or advisor decision arrive late | Medium | High | Reduce | Limit the first dataset to a reproducible sample; agree the blunder label and evaluation protocol in T1; retain a simple baseline rather than extending model research. | Braden Hayes | No usable labeled sample or advisor decision by F1 (Nov 13). | T1, T4, T7; F1, S2; B4, B9 |
| R2 | Technical | Leakage, missing metadata, or weak calibration invalidates predictions | Medium | High | Reduce | Keep played-move outcome information out of input features; split by game and time, add player-disjoint evaluation if claiming new-player generalization, and audit imputation and calibration. | Braden Hayes | Leakage found, or candidate fails agreed validation criteria at S3. | T4, T7, T9, T13; B10, B14 |
| R3 | External | Lichess sign-in, rate limits, or resume semantics block import | Medium | High | Reduce | Verify sign-in and API behavior early; preserve each saved game; test interruption with fixtures; retain PGN input for a demo while fixing API failures. | Nate Poppe | Sign-in cannot complete at S1, or resume duplicates/re-downloads games at S3. | T2, T6, T8, T12; B8, B10 |
| R4 | Technical | Stockfish exceeds the 180-second target | High | High | Reduce | Measure early in T5; pin engine/settings, reuse results, cap workers, and tune search while checking feature quality; recheck the full workflow in T13. | Braden Hayes | Early 20-game probe exceeds 180 seconds or projects an overrun at S1. | T5, T13; S1, S7; B8, B14 |
| R5 | Team / scope | Optional patterns or model experiments displace core functionality | Medium | Medium | Avoid | Deliver factors and core replay first; limit model comparison to one baseline and one candidate; defer player patterns if the core path slips. | Braden Hayes | Any core task uses all its contingency, or factors/replay are incomplete by Mar 29. | T7, T10, T11; S6; B13 |
| R6 | Team | Course load, co-op shifts, exams, or breaks reduce availability | High | High | Reduce | Account for Braden's 18 credits and Nate's expected 15 credits plus part-time co-op; use 12-hour project caps, one-hour coordination, cross-review, and protected breaks; review availability weekly and transfer support work before touching milestone buffers. | Nate Poppe | Either member forecasts under 8 available hours for two successive working weeks. | K1-K8; B8-B15; all task contingency |
| R7 | Technical / team | Integration defects, lost results, or environment differences delay release | Medium | High | Reduce | Share API/feature fixtures before parallel UI/explanation work; test migrations, versioned scores, user isolation, backups, and a clean local deployment. | Nate Poppe | Contract tests fail at S4/S6, restored data differ, or clean install fails before S8. | T3, T9-T14; B11, B13-B15 |
| R8 | External | Unpublished course deadlines or assessment requirements change | Medium | High | Reduce | Check Canvas weekly; replace provisional dates as soon as posted; keep internal release Apr 23 and move work earlier if the official demo is earlier. | Nate Poppe | New official deadline is earlier than a team target, or a new graded item is posted. | C11-C15 and S8 provisional dates; B3-B6, B15 |


## Basis and limitations

Use final D0-D2 completed-game scope and module names. Reconcile live-score wording in project-description.md in C9/T1. Canvas names this Assignment 8, while the supplied prompt refers to Assignments #5 and #6. C1-C7 are complete. Confirm official C11-C15 deadlines when published. Add official CS 5002 deadlines when published. Review availability and scope weekly. Optional patterns are deferred first; do not silently relax the 180-second target. No live games may be analyzed.

Academic calendar: https://www.uc.edu/about/registrar/calendars/academic-year-calendars/2026-2027.html
