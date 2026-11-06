# Stage 1 — Architect

**Purpose:** Understand the requested work and produce a complete, evidence-based plan before review or implementation.

## Required inputs
- The owner's current request and constraints.
- Current GitHub repository state and relevant documentation.
- Existing code, data model, integrations and deployment constraints where applicable.
- Previously approved decisions and unresolved questions.

## Required actions
1. Inspect the relevant repository files and current branch; do not rely on remembered or stale snapshots.
2. Identify the goal, users, workflows, scope and explicit exclusions.
3. Separate confirmed facts, approved decisions, assumptions and proposals.
4. Map affected modules, tables, APIs, integrations, jobs, permissions and user interfaces.
5. Identify data ownership, tenant boundaries, migration/backfill needs and compatibility concerns.
6. Define alternatives, dependencies, risks and unresolved questions.
7. Produce a step-by-step plan and measurable acceptance criteria.
8. Identify which documents will need updating if the proposal is later approved.

## Prohibited actions
- Do not implement, refactor, delete or deploy code.
- Do not convert assumptions or proposals into approved decisions.
- Do not change approved behavior without explicit approval.
- Do not claim tests or inspections were performed if they were not.

## Required output
- Scope and exclusions.
- Current-state findings with repository paths/evidence.
- Proposed design and alternatives.
- Data/security/integration/migration impact.
- Risks, dependencies and open questions.
- Acceptance criteria.
- Documents expected to change.

## Exit gate
Stage 1 is complete when the plan is sufficiently specific for an independent reviewer to challenge. Record its status and run summary in `docs/WORKFLOW.md` and this file. Proceed to Stage 2; no implementation permission is implied.

## Run history
| Run | Date | Workstream | Result | Evidence / commit | Status |
|---|---|---|---|---|---|
| DB-ARCH-01 | 2026-11-06 | Detailed database architecture | Proposed users, tenants, memberships, RBAC, tenant tables, RLS, Prisma/pooling, platform access, provisioning, migrations and recovery | See `docs/ARCHITECTURE.md`, `docs/TENANCY_AND_DATA.md` | Completed; proposal only |
