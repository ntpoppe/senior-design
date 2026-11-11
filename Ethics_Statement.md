# Chess Blunder-Risk Analyzer: Ethics Statement

Braden Hayes and Nate Poppe | CS5001 | November 11, 2026

## 1. Privacy summary

The app reviews finished games. It does not assist during live play. Before an import, we propose a notice explaining what is saved, who can access it, and when it is deleted, followed by an unchecked consent box. Lichess sign-in alone is not consent to store games or use them for training. Pilot uploads will not be used for training without a separate opt-in. Opponents have not consented just because their games are public; we will avoid publishing their names or individual results.

The table groups fields with the same handling. P means local PostgreSQL; U means the signed-in user through the API and authorized project maintainers. Proposed retention R means deletion on request or within 30 days after the final demonstration, including local exports and backups. No cloud database or sensor data is planned.

| Data item | Source | Purpose | Consent | Storage | Access | Retention |
|---|---|---|---|---|---|---|
| user_id, lichess_username | Lichess/login | Identify the account and its records | Sign-in + notice | P | U | R |
| OAuth code, state, token; session cookie | Lichess/browser | Verify login | OAuth screen | Temporary backend memory; browser cookie | Backend/user browser | Discard OAuth values after login; cookie up to 7 days or logout |
| job_id, user_id, source, username, perf_type, target_n | Import form | Select games and track the request | Import checkbox | P | U | R |
| status, imported_n, analyzed_n, oldest_played_at, last_progress_at, message | Import service | Progress, errors, and resume | Import checkbox | P; progress response | U | R |
| game_id, source_game_id, job_id, user_id, played_at; PGN | Lichess/PGN upload | Replay and avoid duplicates | Import checkbox; uploader must have permission | P; temporary upload | U | R; remove temporary upload after parsing |
| Player/opponent handles, ratings, result, time control, moves and clock tags inside PGN | Game source | Match the player and build features | Import notice; opponents not assumed to consent | P within game data | U | R; no public raw-game export |
| move_id, game_id, ply, SAN, fen_before, clock_ms, is_user_move | Parsed game | Show positions and analyze player moves | Import checkbox | P/API; engine memory | U; local Stockfish | R; engine input temporary |
| best_move, eval_cp, analysis status, features and missing-data flags | Stockfish/extraction | Calculate risk and handle incomplete data | Import checkbox | P; local processing | U | R |
| model_version_id, version_tag, is_active, model_file, feature_list, trained_at, metrics | Training process | Repeat and compare predictions | Training-data permission checked separately | Local files/P | Team; version/results shown to user | Keep code and aggregate results; remove personal training records under their policy |
| score, game_id, move_id, model_version_id; factor rank, text, source; contributions/patterns | Model/explainer | Explain estimated risk | Import checkbox | P/API; contributions in memory | U | R; contributions temporary |
| Error code, time, component and job reference | Application logs | Diagnose failures | Import notice | Local log file | Team only | Proposed 14 days, or earlier deletion request |
| Training games, labels, split IDs and derived features | Dataset not yet selected | Fit and evaluate models | Check source terms; separate consent for pilot data | Team-only local files | Braden and Nate | Set before collection; pilot data follows R |

Passwords and persistent OAuth tokens are excluded by the existing design. We also propose discarding unnecessary PGN comments and personal headers, such as email or location, before saving. No such removal is claimed as implemented. Username-linked data is not anonymous. Local storage does not automatically mean encryption at rest; that must be checked on the pilot computer.

This follows ACM 1.6 (privacy) and 1.2 (avoiding harm): collect only what supports review, keep records separated by user, and reject unfinished games. Under ACM 1.4 (fairness), we will compare model results across rating ranges, time controls, and missing-clock cases. We will describe gaps rather than claim equal accuracy for everyone. Scores should use text as well as color and plain explanations; the first version is English-only.

<!-- PAGEBREAK -->

## 2. Security summary

The design uses Lichess OAuth and signed, HttpOnly session cookies lasting up to seven days. OAuth tokens are not saved. The API checks the session on every route except sign-in/callback and basic status. Missing sessions return 401; attempts to read another user's records return 404. The status response should reveal only whether services are ready, not credentials or file paths.

Before a pilot, Nate will configure unique database credentials and a random cookie-signing secret in an untracked local environment file, with placeholders only in examples. The database stays on the private container network. HTTPS is required for network access; Secure/SameSite cookie settings, OAuth state checks, and protection against cross-site requests still need implementation and tests. No default passwords should be accepted at startup.

Imports are limited to 50 games and PGN uploads to 5 MB, with one running job per user. Lichess requests are serial and stop on a 429 response. These limits are not a complete inbound rate limiter; limits for login and other API requests must be set before exposing the service beyond the local machine. Logs must not contain cookies, tokens, passwords, or raw uploads.

No secrets were found in the repository history check on November 11, 2026.

| Self-audit question | Answer |
|---|---|
| Are default credentials in use? | No running app was available to check. Unique local credentials and startup checks are planned. |
| Can users bypass sign-in? | Protected routes require sessions in the design; implementation testing is pending. |
| Can one user access another user's records? | Ownership checks and 404 responses are specified; tests are planned. |
| Are secrets committed? | No secrets were found in the repository history check. |
| Are transport and storage protected? | HTTPS is specified. Local disk encryption and database permissions still need checking. |
| Are uploads and requests bounded? | Size, game-count, and job limits are specified. General API rate limiting is not yet set. |
| Are deletion and log cleanup working? | Proposed in this statement; not implemented or tested yet. |

ACM 2.9 calls for secure systems, and ACM 1.3 calls for honest reporting. For this project that means testing ownership checks and reporting these controls as planned until there is evidence they work.

<!-- PAGEBREAK -->

## 3. Licensing summary

These are the tools named in our design. Versions are not chosen yet. We will record the versions and licenses as we add dependencies.

| Dependency/data | Version | SPDX license | Type | Obligation |
|---|---|---|---|---|
| React | TBD | MIT | Permissive | Keep copyright and license notice. |
| TypeScript | TBD | Apache-2.0 | Permissive | Keep license/notices and identify modifications. |
| Python | TBD | PSF-2.0 | Permissive | Keep Python and bundled third-party notices. |
| FastAPI | TBD | MIT | Permissive | Keep copyright and license notice. |
| Pydantic | TBD | MIT | Permissive | Keep notice; check compiled dependencies too. |
| PostgreSQL | TBD | PostgreSQL | Permissive | Keep copyright and license notice. |
| SQLAlchemy | TBD | MIT | Permissive | Keep copyright and license notice. |
| Alembic | TBD | MIT | Permissive | Keep copyright and license notice. |
| python-chess | TBD | GPL-3.0-or-later | Copyleft | Preserve GPL terms and provide corresponding source when distributing a covered combined work. |
| Stockfish | TBD | GPL-3.0-or-later | Copyleft | Include GPL and corresponding source for the exact distributed engine, including changes. |
| Docker Compose | TBD | Apache-2.0 | Permissive | Preserve license/notices if redistributed; Docker Desktop has separate terms. |
| Lichess database exports, if selected | Dump TBD | CC0-1.0 | Public-domain dedication | Record which dump was used; CC0 does not remove privacy responsibilities. |
| User PGNs / Lichess API | Per import | No blanket SPDX license claimed | Source-dependent | Review source terms and uploader permission; do not assume every upload is CC0. |
| Model library and board/icon assets | Not selected | TBD | Not determined | Choose and check before including them. |

We plan to release our code under GPL-3.0-or-later to work with python-chess and Stockfish. We will keep the required license notices and provide source code when distributing the app. The repository does not yet have a LICENSE file.

## 4. Attribution list

Sources are also listed in `NOTICE.md`. No adapted application code or separate app asset bundle was found in the repository. We will add credits for any code, icons, or images we use.

- React contributors: https://github.com/facebook/react (MIT).
- Microsoft/TypeScript contributors: https://github.com/microsoft/TypeScript (Apache-2.0).
- Python Software Foundation: https://docs.python.org/3/license.html (PSF-2.0 and bundled notices).
- FastAPI contributors: https://github.com/fastapi/fastapi (MIT).
- Pydantic contributors: https://github.com/pydantic/pydantic (MIT).
- PostgreSQL Global Development Group: https://www.postgresql.org/about/licence/ (PostgreSQL).
- SQLAlchemy and Alembic contributors: https://github.com/sqlalchemy/sqlalchemy and https://github.com/sqlalchemy/alembic (MIT).
- Niklas Fiekas/python-chess contributors: https://github.com/niklasf/python-chess (GPL-3.0-or-later).
- Stockfish developers: https://github.com/official-stockfish/Stockfish (GPL-3.0-or-later).
- Docker Compose contributors: https://github.com/docker/compose (Apache-2.0).
- Lichess: https://database.lichess.org/ (downloaded PGN dumps, CC0-1.0); API imports: https://lichess.org/api and https://lichess.org/terms-of-service. Training source remains undecided.

Reference checks: https://spdx.org/licenses/; https://www.gnu.org/licenses/license-list.html; https://www.acm.org/code-of-ethics. ACM 1.5 applies here: credit the source and follow its terms, including when a tool is free.

<!-- PAGEBREAK -->

## 5. AI-use disclosure

"Generative AI tools may be used for drafting and code assistance but not for the design decisions or written analysis that are assessed in this course."

| Tool | Used by | Use |
|---|---|---|
| ChatGPT | Braden Hayes | Drafting and formatting documents, planning and test suggestions, discussing models, and repository/license checks. |
| Claude | Nate Poppe | Project ideation. |

Braden Hayes: I confirm that the submitted architecture, data model, priorities, and design rationale reflect our own decisions and analysis, and that my AI use is accurately disclosed above. Signature: Braden Hayes Date: 11/11/2026
Nate Poppe: I confirm that the submitted architecture, data model, priorities, and design rationale reflect our own decisions and analysis, and that my AI use is accurately disclosed above. Signature: Nate Poppe Date: 11/11/2026
