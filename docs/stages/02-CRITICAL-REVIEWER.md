# Stage 2 — Critical Reviewer

**Purpose:** Independently challenge the Stage 1 plan. The reviewer must seek failure modes, not simply endorse the Architect's work.

## Required review domains
- Requirement completeness, edge cases and conflicting assumptions.
- Authentication, authorization, privacy and tenant isolation.
- Database integrity, constraints, RLS, privilege boundaries and migrations.
- Concurrency, transactions, connection pooling and data consistency.
- Background jobs, events, callbacks, retries and idempotency.
- Integrations, Bridge identity, credential handling and failure recovery.
- Performance, scalability, availability and operational cost.
- UX, accessibility and user error prevention where relevant.
- Backward compatibility, regression risk and test coverage.
- Backup, restore, incident response and lifecycle/offboarding.

## Required actions
1. Review the proposal independently against requirements and approved decisions.
2. Verify time-sensitive technical claims against authoritative documentation when needed.
3. Classify findings by severity: Critical, High, Medium or Low.
4. Distinguish implementation blockers from decisions that can remain open.
5. Explain realistic failure scenarios, impact and recommended mitigation.
6. Identify missing tests and evidence required to close each finding.
7. State whether the proposal is acceptable, conditionally acceptable or requires redesign.

## Prohibited actions
- Do not implement fixes.
- Do not silently rewrite the proposal as approved.
- Do not dismiss a risk merely because a mitigation is planned.
- Do not claim security certification from design review or unit tests.

## Required output
A findings table with severity, affected area, failure scenario, impact, recommendation, blocking status and verification method; plus an overall verdict and unresolved issues.

## Exit gate
Stage 2 is complete when findings are recorded and handed to Stage 3. Record the review summary in `docs/WORKFLOW.md` and this file. No implementation permission is implied.

## Run history
| Run | Date | Workstream | Result | Evidence / commit | Status |
|---|---|---|---|---|---|
| DB-ARCH-01 | 2026-11-06 | Detailed database architecture | Flagged tenant-context forgery, RLS bypass/policy composition, pooled connection leakage, async tenant binding, privileged access and cross-layer isolation gaps | Findings summarized in `docs/SECURITY.md` and `docs/ARCHITECTURE.md` | Completed |
