# Stage 4 — Owner Approval

**Purpose:** Obtain the owner's explicit, informed decision on a specific scope before implementation.

## Required approval package
Present:
- Exact objective and scope.
- Proposed design and important alternatives.
- What is included and explicitly excluded.
- Costs, dependencies, operational implications and risks.
- Remaining unresolved questions and accepted residual risks.
- Acceptance criteria and verification plan.
- Documents and decisions that will be updated.

## Approval rules
1. Only an explicit owner approval of the stated scope counts.
2. A question, discussion, silence, general encouragement or approval of an earlier concept is not implementation authorization.
3. If the owner approves only part, record only that part as approved; the rest remains proposed.
4. If the owner requests changes, return to the appropriate earlier stage and update the proposal.
5. If scope changes materially after approval, stop and seek renewed approval.
6. Record approval date, exact approved scope, exclusions, conditions and any deferred decisions.
7. Never claim legal, security, operational or production readiness merely because the owner approved a design.

## Required output
An approval record with:
- Workstream/run ID.
- Owner's explicit approval wording or faithful summary.
- Approved scope and exclusions.
- Conditions and unresolved decisions.
- Approval date and related documentation commit.

## Prohibited actions
- Do not infer approval.
- Do not begin implementation while approval is pending.
- Do not expand the approved scope during implementation without returning for approval.

## Exit gate
Proceed to Stage 5 only for the exact approved scope. Update `docs/WORKFLOW.md`, this file, decision records and traceability as appropriate.

## Run history
| Run | Date | Workstream | Decision | Evidence | Status |
|---|---|---|---|---|---|
| DB-ARCH-01 | 2026-11-06 | Detailed database architecture | No Stage 3 approval recorded yet | Stage 3 proposal in architecture, tenancy and security documents | Pending |


## Goldfish QA
Include relevant personas and critical journeys, important findings, unresolved usability/accessibility/recovery risks, mitigations and exclusions in the approval package. Persona feedback informs the owner's decision; it does not approve work automatically.
