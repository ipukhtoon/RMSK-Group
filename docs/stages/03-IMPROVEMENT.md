# Stage 3 — Improvement

**Purpose:** Resolve or reduce Stage 2 findings and refine the proposal into a coherent, reviewable candidate for owner approval.

## Required actions
1. Map every reviewer finding to a proposed change, accepted residual risk or explicit deferral.
2. Resolve critical/high blockers where possible; explain any remaining blocker.
3. Compare alternatives and trade-offs rather than selecting a design without rationale.
4. Preserve approved product principles and technology decisions unless a change is explicitly proposed for owner decision.
5. Define trust boundaries, failure behavior, data ownership and security controls precisely.
6. Add migration, rollback, recovery and compatibility considerations.
7. Define measurable acceptance criteria and negative/adversarial tests.
8. Update proposal documents and traceability, clearly marking content as proposed.
9. Record what changed, why, what remains open and which reviewer findings remain unresolved.

## Prohibited actions
- Do not implement application code or infrastructure.
- Do not treat a proposed improvement as owner-approved.
- Do not close a reviewer finding without evidence or an explicit residual-risk decision.
- Do not change the approved stack or scope silently.

## Required output
- Finding-to-resolution matrix.
- Refined design and rationale.
- Alternatives/trade-offs.
- Remaining risks and open decisions.
- Acceptance criteria and verification plan.
- List of documentation changes and commit references.

## Exit gate
Stage 3 is complete when a coherent candidate is ready for explicit Stage 4 owner review. Update `docs/WORKFLOW.md` and this file after the run. No implementation permission is implied.

## Run history
| Run | Date | Workstream | Result | Evidence / commit | Status |
|---|---|---|---|---|---|
| DB-ARCH-01 | 2026-11-06 | Detailed database architecture | Refined tenant-context threat boundary, DB roles, RLS policy composition, Prisma/pool contract, async/cross-layer isolation, platform access, lifecycle and release gates | GitHub commits: `623aaaa`, `6aabb6c`, `5657405`, `7b2e3a2`, `4193060` | Completed; awaiting owner approval |


## Goldfish QA
Map each persona finding to a design improvement, acceptance criterion, accepted residual risk or explicit deferral. Improve clarity and recovery. Add positive, negative, mobile/connectivity and failure scenarios to acceptance criteria. Do not claim testing was executed when only a design walkthrough occurred.

| GQA-PLAN-01 | 2026-11-06 | Repository and product-planning Goldfish audit | Proposed a prioritized five-workstream closure plan, seven golden journeys and implementation-readiness exit criteria; all new solutions remain proposals pending owner approval | `docs/GOLDFISH_AUDIT.md` (commit `ec21b7506b27c37e1b0042ab74e65deaa59831ca`) | Completed; pending owner approval |


| GQA-PLAN-01-IMPROVE | 2026-11-06 | Improve Goldfish repository/planning audit | Converted 17 audit findings into a finding-to-improvement disposition; created structured ISP discovery/MVP scoring and hard gates; specified seven golden journeys with actor, flow, state, failure and acceptance examples; kept all market/technical/commercial decisions open pending evidence and owner approval | `docs/GOLDFISH_AUDIT.md`, `docs/PRODUCT_DISCOVERY_AND_MVP.md`, `docs/GOLDEN_JOURNEYS.md` (commits recorded in central tracker) | Completed; Stage 4 pending |
