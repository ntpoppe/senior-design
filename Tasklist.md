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
