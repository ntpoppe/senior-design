# Chess Blunder Predictor — Project Plan

**Braden Hayes and Nate Poppe** · Baseline Oct 28, 2026

Assignments #5 and #6. Scope follows final Designs D0–D2: completed games only (the older project description's live-scoring wording is reconciled in C9). Fall 2026 covers design deliverables; implementation runs Jan 11–Apr 23, 2027.

## 1. Task List (Assignment #5)

T1. Specify the Blunder-Risk Model training data, blunder label, prediction-time features, and evaluation criteria with the advisor. Owner: Braden Hayes.

T2. Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, and startup validation. Owner: Nate Poppe.

T3. Develop Game and Results Storage with database migrations, duplicate prevention, and model-versioned scores. Owner: Nate Poppe.

T4. Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels and leakage-controlled splits. Owner: Braden Hayes.

T5. Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, and bounded parallel workers. Owner: Braden Hayes.

T6. Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks and interrupted-import recovery. Owner: Nate Poppe.

T7. Develop and evaluate a baseline and candidate Blunder-Risk Model with probability-calibration checks. Owner: Braden Hayes.

T8. Develop the User Interface sign-in and import pages with progress messages and resume controls. Owner: Nate Poppe.

T9. Develop Blunder-Risk Model batch inference with feature-schema validation and versioned model loading. Owner: Braden Hayes.

T10. Develop the User Interface Games and Replay pages showing the three highest-risk moves, risk scores, and engine-best moves. Owner: Nate Poppe.

T11. Develop Risk Explanation and Analytics with plain-language risk factors and player-pattern aggregation. Owner: Braden Hayes.

T12. Test User Interface, Game Import and Validation, and Game and Results Storage end to end, including interrupted imports and user-data isolation. Owner: Nate Poppe.

T13. Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out data and the 20-game analysis target. Owner: Braden Hayes.

T14. Document and validate Configuration and User Interface deployment with a reproducible local setup, demo dataset, and user guide. Owner: Nate Poppe.

## 2. Milestones List (Assignment #6)

C = course deliverable, F/S = internal milestone. Canvas dates are due dates; C11–C14 are team targets for roadmap items with no published deadline. The Dec 8–Jan 22 gap is the exam and winter blackout (K2–K3); no work is planned there.

| ID | Milestone | Date | Status | What it demonstrates | How verified | Verifier |
| --- | --- | --- | --- | --- | --- | --- |
| C1 | Professional Biography | Sep 2, 2026 | Complete | Introduces each member's background and the skills each brings to the project. | Instructor checks both submitted biographies and Canvas receipts. | Instructor |
| C2 | Team Formation and Project Description | Sep 9, 2026 | Complete | Records the two-person team and the project's purpose and scope. | Roster and project-description.md compared with the submission. | Instructor |
| C3 | Advisor Selection and Design Constraints Essay | Sep 18, 2026 | Complete | Identifies the advisor and the constraints that govern the design. | Advisor confirmation and the submitted constraints essay checked. | Instructor, advisor |
| C4 | User Stories and Use Cases | Sep 28, 2026 | Complete | Defines the user workflow and the acceptance criteria later used in D2. | Submitted stories and criteria checked against D2. | Instructor |
| C5 | Design D0: High-Level System Design | Oct 2, 2026 | Complete | Defines the system modules, their interfaces, and primary owners. | D0 artifact inspected in the submission and the repository. | Instructor, advisor |
| C6 | Design D1: Detailed Design Part 1 | Oct 8, 2026 | Complete | Defines storage, import, feature-extraction, and API contracts. | D1 artifact and diagrams inspected. | Instructor, advisor |
| C7 | Design D2: Detailed Design Part 2 and Peer Review | Oct 14, 2026 | Complete | Closes the design with model and UI behavior, edge cases, and peer-review responses. | D2 and its peer-review section inspected; separate notes confirmed in the repository. | Instructor, advisor |
| C8 | Task List, Milestones, Timeline, Effort Matrix | Oct 28, 2026 | Submitted | Establishes a consistent, resourced delivery baseline with owners and dates. | This PDF submitted; Tasklist.md in the repository root and plan sources in /Planning. | Instructor |
| C9 | Requirements and Technical Specification | Oct 30, 2026 | Planned | Freezes the completed-game scope and measurable system contracts. | Requirements traced to D0–D2; live-scoring wording resolved; open ML decisions recorded. | Instructor, advisor |
| C10 | Test and Validation Plan Draft | Nov 4, 2026 | Planned | Maps each requirement to fixtures, tests, metrics, and acceptance evidence. | Test cases reviewed for timing, resume, user isolation, model versions, and held-out evaluation. | Instructor |
| C11 | Ethics review package | Nov 6, 2026 | Planned | Documents fair-play boundaries, data handling, and the limits of predicted probabilities. | Advisor reviews the completed-game-only policy, data provenance, and uncertainty wording. | Advisor |
| F1 | Modeling protocol agreed (T1) | Nov 13, 2026 | Planned | Settles the data source, blunder label, pre-move feature boundary, and evaluation protocol. | Advisor reviews the T1 decision log, sample rows, and model-selection criteria. | Advisor, Nate |
| C12 | Communication package | Nov 13, 2026 | Planned | Makes the project understandable through a short narrative and draft slides. | Nate checks the usability narrative, Braden checks modeling claims, advisor reviews. | Advisor |
| C13 | Oral presentation rehearsal 1 | Nov 18, 2026 | Planned | Shows a coherent explanation of the design and delivery plan. | Timed rehearsal; feedback and assigned corrections recorded. | Advisor |
| C14 | Oral presentation rehearsal 2 | Nov 25, 2026 | Planned | Shows that rehearsal feedback has been incorporated. | Repeat timed presentation; major feedback items closed. | Instructor, advisor |
| C15 | Week 14 fall report | Nov 25, 2026 | Planned | Preserves the design baseline and reproduces the current planning tables. | All six components present, task IDs identical across tables, status updated. | Instructor, advisor |
| C16 | Project Advisor Assessments | Dec 8, 2026 | Planned | Gives the advisor evidence of fall design progress and readiness. | Evidence package sent Dec 4; advisor completes the assessment by Dec 8. | Advisor |
| S1 | Startup and engine feasibility | Jan 22, 2027 | Planned | The app starts with authentication, configuration, and logging working, and fixed engine settings produce features on sample PGNs. | T2/T5 smoke tests pass; an early 20-game timing probe is recorded with hardware and engine settings. | Both members |
| S2 | Storage and training-data baseline | Feb 5, 2027 | Planned | The database schema and a bounded training dataset support repeatable development. | Migrations apply cleanly on a fresh database; label samples, missing data, and split leakage audited. | Both members, advisor |
| S3 | Resumable import and candidate model | Feb 19, 2027 | Planned | Validated imports survive interruption, and a candidate model improves on the agreed baseline. | Injected import interruption leaves no duplicates; validation metrics and calibration compared with the baseline. | Both members, advisor |
| S4 | Import-to-score pipeline | Mar 5, 2027 | Planned | The import UI drives versioned inference and shows correct progress end to end. | A completed-game import runs through to stored scores; incompatible feature batches are rejected. | Both members |
| S5 | Pre-break baseline frozen | Mar 12, 2027 | Planned | A tagged working build can be resumed after midterms and spring break. | Smoke tests pass, database and model artifacts backed up, outstanding defects documented. | Both members |
| S6 | Replay and explanations | Apr 2, 2027 | Planned | Replay shows the selected board, the top-risk moves, and readable risk factors. | Acceptance fixtures show exact FENs and at least two supported factors of 25 words or fewer. | Both members, advisor |
| S7 | Acceptance candidate | Apr 16, 2027 | Planned | Core requirements pass integrated, statistical, and performance checks. | Acceptance suite and held-out evaluation pass; 20 games analyzed in under 180 s; release blockers closed. | Both members, advisor |
| S8 | Final release and handoff | Apr 23, 2027 | Planned | A repeatable local release, user guide, evaluation report, and demo are ready before finals. | Clean-environment install, demo rehearsal, and artifact hashes and documentation verified. | Advisor, both members |

## 3. Table 1: Timeline (Assignment #6)

Each task carries 20% buffer: work is planned to finish on the "Work done" date, and the days up to "Buffered finish" are reserve. A protected block before each milestone (below) is reserved for fixes and verification only. Dependencies are finish-to-start.

| ID | Task statement | Owner | Start | Work done | Buffered finish | Depends on | Path / risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| T1 | Specify the Blunder-Risk Model training data, blunder label, prediction-time features, and evaluation criteria with the advisor. | Braden Hayes | Oct 29, 2026 | Nov 6, 2026 | Nov 10, 2026 | C7 | Model; R1, R2 |
| T2 | Develop Authentication, Configuration, and Logging with Lichess sign-in, session checks, and startup validation. | Nate Poppe | Jan 11, 2027 | Jan 15, 2027 | Jan 20, 2027 | C7 | App; R3, R7 |
| T3 | Develop Game and Results Storage with database migrations, duplicate prevention, and model-versioned scores. | Nate Poppe | Jan 25, 2027 | Feb 1, 2027 | Feb 3, 2027 | T2 | App; R7 |
| T4 | Obtain and prepare the Blunder-Risk Model training dataset with reproducible labels and leakage-controlled splits. | Braden Hayes | Jan 25, 2027 | Feb 1, 2027 | Feb 3, 2027 | T1, T5 | Model; R1, R2 |
| T5 | Develop Chess Feature Extraction with Stockfish analysis, reusable engine results, and bounded parallel workers. | Braden Hayes | Jan 11, 2027 | Jan 15, 2027 | Jan 20, 2027 | T1 | Model; R4 |
| T6 | Develop Game Import and Validation for Lichess and PGN inputs with completed-game checks and interrupted-import recovery. | Nate Poppe | Feb 8, 2027 | Feb 15, 2027 | Feb 17, 2027 | T2, T3 | App; R3, R7 |
| T7 | Develop and evaluate a baseline and candidate Blunder-Risk Model with probability-calibration checks. | Braden Hayes | Feb 8, 2027 | Feb 15, 2027 | Feb 17, 2027 | T4 | Model; R1, R2 |
| T8 | Develop the User Interface sign-in and import pages with progress messages and resume controls. | Nate Poppe | Feb 22, 2027 | Mar 1, 2027 | Mar 3, 2027 | T2, T6 | App; R3 |
| T9 | Develop Blunder-Risk Model batch inference with feature-schema validation and versioned model loading. | Braden Hayes | Feb 22, 2027 | Mar 1, 2027 | Mar 3, 2027 | T3, T5, T7 | Model; R2, R7 |
| T10 | Develop the User Interface Games and Replay pages showing the three highest-risk moves, risk scores, and engine-best moves. | Nate Poppe | Mar 22, 2027 | Mar 29, 2027 | Mar 31, 2027 | T3, T8, T9 | App; R5, R7 |
| T11 | Develop Risk Explanation and Analytics with plain-language risk factors and player-pattern aggregation. | Braden Hayes | Mar 22, 2027 | Mar 29, 2027 | Mar 31, 2027 | T9 | Model; R2, R5 |
| T12 | Test User Interface, Game Import and Validation, and Game and Results Storage end to end, including interrupted imports and user-data isolation. | Nate Poppe | Apr 5, 2027 | Apr 12, 2027 | Apr 14, 2027 | T6, T8, T10, T11 | App; R3, R7 |
| T13 | Validate and refine Chess Feature Extraction, Blunder-Risk Model, and Risk Explanation and Analytics against held-out data and the 20-game analysis target. | Braden Hayes | Apr 5, 2027 | Apr 12, 2027 | Apr 14, 2027 | T5, T7, T9–T11 | Model; R2, R4 |
| T14 | Document and validate Configuration and User Interface deployment with a reproducible local setup, demo dataset, and user guide. | Nate Poppe | Apr 19, 2027 | Apr 20, 2027 | Apr 21, 2027 | T12, T13, S7 | Release; R6–R8 |

**Critical paths.** Model: T1 → T5 → T4 → T7 → T9 → T11 → T13 → T14 → S8. App: T2 → T3 → T6 → T8 → T10 → T12 → T14 → S8. Both are critical because each member carries one path at full capacity; a slip on either consumes a protected block or cuts optional scope (T11 player patterns go first).

**Milestone dates and protected blocks**

| Milestone | Planned completion | Protected block |
| --- | --- | --- |
| C1–C7 | Sep 2–Oct 14, 2026 | Complete |
| C8 | Oct 28, 2026 | Oct 28 |
| C9 | Oct 30, 2026 | Oct 29 |
| C10 | Nov 4, 2026 | Nov 2–3 |
| C11 | Nov 6, 2026 | Nov 5 |
| C12, F1 | Nov 13, 2026 | Nov 12 |
| C13 | Nov 18, 2026 | Nov 16–17 |
| C14, C15 | Nov 25, 2026 | Nov 23–24 |
| C16 | Dec 8, 2026 | Dec 3–4 |
| S1 | Jan 22, 2027 | Jan 21 |
| S2 | Feb 5, 2027 | Feb 4 |
| S3 | Feb 19, 2027 | Feb 18 |
| S4 | Mar 5, 2027 | Mar 4 |
| S5 | Mar 12, 2027 | Mar 11 |
| S6 | Apr 2, 2027 | Apr 1 |
| S7 | Apr 16, 2027 | Apr 15 |
| S8 | Apr 23, 2027 | Apr 22 |

**Availability constraints**

| ID | Dates | Constraint | Response |
| --- | --- | --- | --- |
| K1 | Nov 26–29, 2026 | Thanksgiving | No project work |
| K2 | Dec 6–12, 2026 | Fall exams | No development; C16 evidence sent Dec 4 |
| K3 | Dec 13, 2026–Jan 10, 2027 | Winter break | No development; restart Jan 11 |
| K4 | Jan 18, 2027 | University holiday | T2/T5 fit the remaining days |
| K5 | Mar 8–12, 2027 | Midterms | 6 h/member; freeze and repair only |
| K6 | Mar 15–21, 2027 | Spring break | No project work |
| K7 | Apr 24–29, 2027 | Spring finals | No development; release Apr 23 |
| K8 | Jan 11–Apr 23, 2027 | Course load and co-op | Braden 18 credits; Nate 15 credits plus 20 h/week co-op; 12 project h/week each |

## 4. Table 2: Effort Matrix (Assignment #6)

**DRAFT: estimates pending team review on Oct 29.** Each owner estimates optimistic, most likely, and pessimistic hours (O/M/P); the matrix uses (O + 4M + P) / 6. Support hours are the other member's review and pairing time. **P** marks the primary owner, who carries the largest share of every task. Base hours are shown per task, with the 20% contingency added once at the bottom.

| ID | Task | Owner O / M / P (h) | Braden Hayes (h) | Nate Poppe (h) | Row total (h) |
| --- | --- | --- | --- | --- | --- |
| T1 | Modeling protocol | 6 / 10 / 16 | **10.3 P** | 2.0 | 12.3 |
| T2 | Auth, configuration, logging | 8 / 12 / 20 | 1.0 | **12.7 P** | 13.7 |
| T3 | Game and results storage | 8 / 11 / 18 | 2.0 | **11.7 P** | 13.7 |
| T4 | Training dataset | 8 / 13 / 22 | **13.7 P** | 1.0 | 14.7 |
| T5 | Chess feature extraction | 10 / 14 / 24 | **15.0 P** | 1.0 | 16.0 |
| T6 | Game import and validation | 10 / 15 / 26 | 2.0 | **16.0 P** | 18.0 |
| T7 | Baseline and candidate model | 9 / 14 / 22 | **14.5 P** | 1.0 | 15.5 |
| T8 | Sign-in and import UI | 7 / 11 / 18 | 1.0 | **11.5 P** | 12.5 |
| T9 | Batch inference | 8 / 12 / 18 | **12.3 P** | 3.0 | 15.3 |
| T10 | Games and replay UI | 9 / 14 / 22 | 2.0 | **14.5 P** | 16.5 |
| T11 | Risk explanation and analytics | 8 / 12 / 20 | **12.7 P** | 2.0 | 14.7 |
| T12 | App integration testing | 8 / 12 / 20 | 4.0 | **12.7 P** | 16.7 |
| T13 | Model validation and refinement | 8 / 13 / 20 | **13.3 P** | 3.0 | 16.3 |
| T14 | Deployment and user guide | 4 / 7 / 12 | 1.5 | **7.3 P** | 8.8 |
| **Base subtotal** | | | **105.3** | **99.4** | **204.7** |
| **Contingency (+20%)** | | | 21.1 | 19.9 | 41.0 |
| **Total** | | | **126.4** | **119.3** | **245.7** |

Braden carries 5.9 more base hours (about 6%), driven by the model path. To rebalance, Nate takes up to 3 hours of T13 test execution; if Nate's availability drops instead, T12 fixture work moves to Braden. Primary ownership does not change.

## 5. Work Breakdown Structure

1. **Scope and foundations**
    - T1. Modeling protocol (Braden Hayes)
    - T2. Auth, configuration, logging (Nate Poppe)
    - T3. Game and results storage (Nate Poppe)
2. **Data and model development**
    - T4. Training dataset (Braden Hayes)
    - T5. Chess feature extraction (Braden Hayes)
    - T7. Baseline and candidate model (Braden Hayes)
3. **Import, scoring, and user experience**
    - T6. Game import and validation (Nate Poppe)
    - T8. Sign-in and import UI (Nate Poppe)
    - T9. Batch inference (Braden Hayes)
    - T10. Games and replay UI (Nate Poppe)
    - T11. Risk explanation and analytics (Braden Hayes)
4. **Validation and delivery**
    - T12. App integration testing (Nate Poppe)
    - T13. Model validation and refinement (Braden Hayes)
    - T14. Deployment and user guide (Nate Poppe)

## 6. Risk Register

Likelihood: low <25%, medium 25–60%, high >60%. High impact threatens a core requirement or milestone. Owners check triggers weekly; the last column shows where each risk appears in the schedule.

| ID | Risk | Category | Likelihood | Impact | Strategy | Mitigation | Trigger | Owner | In Table 1 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R1 | Training data, labels, or advisor decision arrive late | Technical / external | Medium | High | Reduce | Fix label and protocol in T1; start with a small reproducible sample and a simple baseline | No labeled sample or advisor decision by F1 (Nov 13) | Braden | T1, T4, T7; F1, S2 |
| R2 | Leakage or weak calibration invalidates predictions | Technical | Medium | High | Reduce | Keep played-move outcomes out of features; split by game and time; audit calibration | Leakage found, or candidate fails criteria at S3 | Braden | T4, T7, T9, T13 |
| R3 | Lichess sign-in, rate limits, or resume logic block import | External | Medium | High | Reduce | Verify API early; test interruption with fixtures; keep PGN upload as demo fallback | Sign-in fails at S1, or resume duplicates games at S3 | Nate | T2, T6, T8, T12 |
| R4 | Stockfish analysis exceeds 180 s for 20 games | Technical | High | High | Reduce | Time early in T5; cache results, cap workers, tune search depth | 20-game probe projects over 180 s at S1 | Braden | T5, T13; S1, S7 |
| R5 | Optional features or model experiments displace core work | Team / scope | Medium | Medium | Avoid | One baseline and one candidate only; defer player patterns if core slips | A core task uses all its buffer, or replay incomplete by Mar 29 | Braden | T7, T10, T11; S6 |
| R6 | Course load, co-op, exams, or breaks reduce availability | Team | High | High | Reduce | 12 h/week caps, weekly availability check, shift support hours before using milestone blocks | Either member under 8 available hours for two straight weeks | Nate | K1–K8, all buffers |
| R7 | Integration defects or environment differences delay release | Technical / team | Medium | High | Reduce | Shared API and feature fixtures before parallel work; clean-install test | Contract tests fail at S4/S6, or clean install fails before S8 | Nate | T3, T9–T14 |
| R8 | Course deadlines are published earlier than team targets | External | Medium | High | Reduce | Check Canvas weekly; pull work earlier if needed | A new deadline precedes a team target | Nate | C11–C15, S8 |
