# Banking Demo — Sprint 1

Target: October 29, 2026. Capacity: 6–8 focused hours per day.
Goal: A reproducible native iOS banking portfolio MVP with a BFF, one server-driven dashboard, and one evaluated Core ML feature.
Status: Planning baseline. No implementation or build is verified by this document.

## Working agreement

- Independent branding, synthetic accounts and transactions, simulated transfers only.
- Target adaptive layouts for iPhone 18 and iPhone Duo; verify actual SDK and simulator availability before promising device coverage.
- Explain each feature's purpose, architecture, data flow, code, alternatives, and verification before moving on.
- Use SwiftUI Pro, Swift Testing Pro, and Swift Concurrency Pro for relevant work. Preserve the verified deployment target unless a change is explicitly agreed.
- Work in small feature branches. Review diffs, commit, push, run checks, then merge.
- Xcode Cloud handles iOS CI. GitHub Actions handles BFF CI. Avoid duplicate iOS pipelines.
- Mark a task done only after its acceptance criteria, documentation, and verification are complete. Never infer success from code generation or a push.
- Record blocked or unverified tasks honestly. Reduce scope if required to protect understanding and quality; record deferred requirements rather than silently dropping them.

## Status conventions

Unchecked = not complete. Add `IN PROGRESS` or `BLOCKED: reason` beside an active item. Checked = meets the definition of done below.

## Backlog and acceptance criteria

### S0 — Baseline and reproducible setup
- [x] S0.1 Inspect the existing Xcode project, entry point, screens, targets, schemes, dependencies, tests, and Git root/remote.
- [x] S0.2 Record macOS, Xcode, Swift language mode, deployment target, and available simulator destinations.
- [ ] S0.3 Run and record a baseline simulator build; document blockers if it fails. **IN PROGRESS — pending review.**
- [ ] S0.4 Create README.md, AGENTS.md, docs/BUILD_JOURNAL.md, docs/ARCHITECTURE.md, and retain this SPRINT.md at the repository root.
- [ ] S0.5 Confirm the banking repository and Xcode Cloud connection; protect credentials and exclude machine-specific files.
Acceptance: A new contributor can identify prerequisites, open the project, and repeat the baseline build from the documentation.

### S1 — Architecture and API decisions
- [ ] S1.1 Document feature boundaries, dependency direction, composition root, state ownership, and navigation.
- [ ] S1.2 Define domain models: accounts, transactions, transfers, money, and categories. Use decimal or minor-unit money representation and explicit currency.
- [ ] S1.3 Define repository interfaces, mock implementations, and dependency injection; justify each abstraction.
- [ ] S1.4 Document dashboard, accounts, transactions, transfer, and SDUI API contracts, including pagination and stable errors.
- [ ] S1.5 Review contracts for decoding, optionality, dates, identifiers, money precision, schema versioning, and compatibility.
Acceptance: Trace a screen action through state, repository, API client, BFF, and response without ambiguous ownership. Record alternatives and decisions.

### S2 — Design system and adaptive shell
- [ ] S2.1 Establish independent name, colors, typography, spacing, and currency formatting.
- [ ] S2.2 Implement reusable account card, transaction row, primary button, and loading/empty/error components.
- [ ] S2.3 Implement navigation and layouts that adapt to available width without hard-coded device names.
- [ ] S2.4 Verify Dynamic Type, VoiceOver labels/order, contrast, touch targets, light/dark appearance, and reduced-motion behavior where relevant.
Acceptance: Preview components in representative narrow/wide layouts and large accessibility text. Record manual observations.

### S3 — Accounts dashboard
- [ ] S3.1 Display synthetic account balances and recent transactions through injected repositories.
- [ ] S3.2 Support loading, content, empty, error, retry, and refresh states.
- [ ] S3.3 Navigate to account and transaction details.
- [ ] S3.4 Verify state transitions, refresh cancellation, and error recovery with meaningful tests.
Acceptance: Every state can be reproduced using controlled fixtures, without real credentials or financial data.

### S4 — Transaction history
- [ ] S4.1 Implement history and details with date, merchant, amount, currency, category, and pending status.
- [ ] S4.2 Implement filtering and paginated loading, including empty results and page failures.
- [ ] S4.3 Verify stable identity, no duplicate pages, correct formatting, and useful accessibility descriptions.
Acceptance: Fixtures demonstrate first/last page, retry, filter changes, and detail navigation.

### S5 — BFF and client networking
- [ ] S5.1 Choose and justify a small BFF stack; document start commands, configuration, and synthetic seed data.
- [ ] S5.2 Implement agreed endpoints with validation, pagination, and consistent error responses.
- [ ] S5.3 Integrate URLSession/Codable client with cancellation, HTTP status handling, and injected configuration.
- [ ] S5.4 Keep credentials out of source; clearly document mock authentication and its production limitations.
- [ ] S5.5 Test contract behavior, invalid inputs, decoding failures, unavailable service, and request cancellation.
Acceptance: The iOS app retrieves synthetic banking data from the running BFF; a clean checkout can reproduce the connection.

### S6 — Server-driven dashboard
- [ ] S6.1 Define a versioned schema and allowlisted native card types with stable identifiers.
- [ ] S6.2 Render account, recent-transaction, and insight cards in server-defined order.
- [ ] S6.3 Handle unknown card types, malformed payloads, unsupported versions, and offline fallback.
- [ ] S6.4 Keep transfer validation and critical flow behavior native; document this boundary.
- [ ] S6.5 Test schema decoding, ordering, unknown cards, and fallback behavior.
Acceptance: Changing server configuration changes supported dashboard composition without a client code change; unsupported content does not crash the screen.

### S7 — Simulated transfer
- [ ] S7.1 Implement account selection, amount entry, validation, review, and confirmation.
- [ ] S7.2 Simulate success and failure through the BFF; prevent duplicate submissions and define retry/idempotency behavior.
- [ ] S7.3 Verify positive amounts, currency rules, insufficient funds, same-account selection, and duplicate submission.
Acceptance: No real money moves. User-facing copy makes simulation clear and final state remains consistent after retries.

### S8 — Core ML categorization
- [ ] S8.1 Define model inputs, category labels, synthetic/lawfully usable training data, and train/validation/test separation.
- [ ] S8.2 Train or obtain a suitable model; record its provenance, version, conversion/training steps, and limitations.
- [ ] S8.3 Integrate actual on-device Core ML inference behind an interface; avoid blocking the UI.
- [ ] S8.4 Display category suggestions with confidence handling, low-confidence fallback, and user override.
- [ ] S8.5 Evaluate held-out accuracy and per-category behavior; measure representative inference latency and record device/build context.
Acceptance: The app runs a real model, not a rule-based placeholder. Claims match measured evidence. Corrections do not imply model retraining unless implemented.

### S9 — Quality and CI/CD
- [ ] S9.1 Configure Xcode Cloud to build and run relevant iOS unit tests for agreed branch/PR events.
- [ ] S9.2 Configure GitHub Actions for BFF tests and agreed static checks.
- [ ] S9.3 Add a focused UI smoke test using supported UI testing tools; Swift Testing covers appropriate unit tests.
- [ ] S9.4 Profile dashboard scrolling/loading and inference; record baseline observations, identified bottlenecks, and any improvements.
- [ ] S9.5 Perform VoiceOver and accessibility-text walkthroughs and representative narrow/wide simulator checks.
- [ ] S9.6 Verify TestFlight eligibility/signing; configure distribution only if available. Otherwise record it as blocked/deferred, not complete.
Acceptance: Relevant CI checks pass on the final commit; limitations and manual checks are explicit. No claim of real-device coverage without that check.

### S10 — Reproduction and presentation
- [ ] S10.1 Complete a clean-checkout reproduction of iOS setup, BFF startup, tests, and model generation or artifact acquisition.
- [ ] S10.2 Finish architecture diagram, decision records, API examples, and feature walkthroughs.
- [ ] S10.3 Record a demo showing supported flows, error handling, SDUI configuration change, and ML categorization.
- [ ] S10.4 Review every backlog item, link evidence, and list deferred scope and known limitations.
Acceptance: Another developer can follow the README and build journal without relying on this chat.

## Suggested schedule

| Date | Focus |
| --- | --- |
| Oct. 23–24 | Baseline, architecture, design system, dashboard |
| Oct. 25 | Transactions, BFF contracts and implementation |
| Oct. 26 | Networking and SDUI |
| Oct. 27 | Simulated transfers and Core ML |
| Oct. 28 | Tests, CI, accessibility, performance |
| Oct. 29 | Fixes, clean reproduction, final documentation, demo |

This is a target sequence, not proof of feasibility. Re-estimate after the baseline and first complete feature.

## Definition of done — required for every task

- [ ] Implementation meets the task's acceptance criteria.
- [ ] User can explain purpose, state/data flow, ownership, and design tradeoffs.
- [ ] Appropriate build/tests/manual checks actually ran; failures are resolved or the task stays incomplete.
- [ ] Relevant accessibility, performance, and error behavior checked.
- [ ] Build journal and reproduction instructions updated.
- [ ] Diff reviewed, committed, pushed, and relevant CI passed; merge recorded where applicable.
- [ ] Evidence recorded below before the task checkbox is checked.

## Evidence record template

Copy for each completed task:

### Task ID — title
- Status:
- Files changed:
- Purpose and user behavior:
- Data flow and state ownership:
- Decision, alternatives, and tradeoffs:
- Exact setup/implementation steps and commands:
- Verification command or manual procedure:
- Actual result and environment:
- Commit / PR / CI reference:
- Documentation updated:
- Known limitations / follow-up:

## Codex operating instructions

Read SPRINT.md before work. Select one task and mark it IN PROGRESS. Explain the purpose and planned changes. Make a small reviewable change. Run appropriate verification. Explain the resulting code and tradeoffs. Update the build journal with exact reproduction steps. Check off the task only after every completion condition is met and evidence is recorded. Do not bulk-check tasks, invent test results, silently change scope, or assume a successful push means a successful build. Preserve user changes and never commit secrets. Pause for the learner's questions between meaningful milestones.
