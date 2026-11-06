# Stage 5 — Implementation & Verification

**Purpose:** Deliver only the scope explicitly approved in Stage 4, then verify the result and update the project record.

## Pre-implementation gate
Before changing anything:
- Confirm the exact Stage 4 approval record.
- Re-read current repository state and relevant files.
- Confirm branch, working tree/commit state and affected components.
- Confirm backup/rollback plan for risky changes.
- Confirm acceptance criteria, test plan and excluded scope.
- Stop if approval, requirements or safety prerequisites are ambiguous.

## Implementation rules
1. Implement only approved scope; no opportunistic redesign or unrelated cleanup.
2. Preserve existing approved behavior and compatibility.
3. Use reviewable, focused changes and safe migration practices.
4. Never expose secrets, credentials, subscriber personal data or production data in commits/logs.
5. Use idempotent and recoverable operations for provisioning, jobs and migrations.
6. Keep documentation synchronized with actual implementation status.

## Verification requirements
Select tests appropriate to the change and report exact results:
- Unit, integration, end-to-end and regression tests.
- Authorization and cross-tenant negative tests.
- Database constraints, RLS and migration/rollback tests.
- Concurrency, transaction, pooling and async-job tests.
- Integration, retry, idempotency and failure-recovery tests.
- Performance and operational checks where relevant.
- Backup/restore validation for data-risking changes.
- Build, lint, type-check and deployment checks where applicable.

Do not mark a control implemented or verified without evidence. Distinguish not run, passed, failed, blocked and not applicable.

## Required completion report
- Approved scope and implementation summary.
- Changed files and commit references.
- Tests run with commands/environment and outcomes.
- Security, migration, performance and regression findings.
- Known limitations, deferred work and residual risks.
- Rollback/recovery instructions where relevant.
- Documentation status and next recommended action.

## Prohibited actions
- Do not exceed the approved scope.
- Do not claim tests passed if they were not run.
- Do not claim production readiness from a successful build alone.
- Do not close unresolved critical/high findings without explicit disposition.

## Exit gate
A run is complete only when implementation and verification evidence are recorded. Update the relevant project documents, `docs/WORKFLOW.md`, this file, decisions, traceability and changelog. If verification fails, record the failure and next action; do not label the work complete.

## Run history
| Run | Date | Workstream | Result | Evidence / commit | Status |
|---|---|---|---|---|---|
| DB-ARCH-01 | 2026-11-06 | Detailed database architecture | No implementation authorized or performed | Stage 4 approval pending | Not started |


## Goldfish QA
Execute approved persona scenarios against the actual implementation. Verify first-time comprehension, role-appropriate permissions, task completion, clear success/failure/pending feedback, duplicate-action safety, mobile responsiveness and recovery from network/timeouts where relevant. Record each scenario as Passed, Failed, Blocked, Not Run or Not Applicable with evidence/reason. Do not convert a design walkthrough into a passed test.
