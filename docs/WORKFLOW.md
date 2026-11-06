# Owner-Controlled Project Workflow

This five-stage workflow is mandatory for substantial product and engineering work.

## 1. Architect
- Inspect current repository, documentation, code and constraints.
- Clarify requirements and produce a plan.
- Identify affected modules, data, integrations and migration needs.
- Do not edit implementation or silently change approved behavior.

## 2. Critical Reviewer
Conduct an independent review for:
- Missing requirements and edge cases.
- Security, privacy and tenant isolation.
- Bugs and incorrect business logic.
- Database integrity and migration risk.
- Performance and reliability.
- Integration failures and retry/idempotency.
- UX/accessibility and regression risk.
- Test coverage and operational impact.

## 3. Improvement
- Refine the plan based on review.
- Present alternatives and trade-offs.
- Identify scope boundaries and acceptance criteria.
- Keep proposals separate from approved decisions.

## 4. Owner Approval
- Explicit approval is required before implementation.
- Silence, discussion, a question or approval of a concept is not automatically implementation permission.
- Record exactly what was approved and what remains excluded.
- If scope changes, return for approval.

## 5. Implementation and Verification
- Implement only approved scope.
- Preserve existing approved functionality.
- Add/update tests and run relevant validation.
- Review security, data migrations, performance and regressions.
- Report changed files, tests, limitations and remaining risks.

## Documentation maintenance

After every explicitly approved feature or business/technical decision:
- Update the PRD/requirements.
- Update architecture or relevant domain document.
- Add/update decision record.
- Update requirements traceability.
- Add a dated changelog entry.
- Keep status clear: proposed, approved, implemented, verified, deferred or rejected.

## Working style

Prefer complete, copy-paste-ready artifacts and direct actionable instructions. Do not invent connected tools or completed actions. Preserve existing functionality unless the owner approves a change.


## Stage-by-stage run tracking

The detailed stage playbooks live in `docs/stages/`:
1. [Architect](stages/01-ARCHITECT.md)
2. [Critical Reviewer](stages/02-CRITICAL-REVIEWER.md)
3. [Improvement](stages/03-IMPROVEMENT.md)
4. [Owner Approval](stages/04-OWNER-APPROVAL.md)
5. [Implementation & Verification](stages/05-IMPLEMENTATION-VERIFICATION.md)

### Mandatory run-record rule
After **every stage run**, update that stage's run history and this workflow's current project tracker in the same documentation update cycle. Record date, workstream/run ID, scope, outcome, status, unresolved blockers, evidence/tests (if any), affected documents and commit reference. Update the next stage's status and the project README/changelog when the overall project state materially changes.

Use these status labels consistently: `Not started`, `In progress`, `Completed`, `Pending owner approval`, `Blocked`, `Deferred`, `Rejected`. Stage completion does not mean owner approval or implementation completion.

### Current active workstream — DB-ARCH-01
- **Stage 1 Architect:** Completed — detailed database architecture proposal documented.
- **Stage 2 Critical Reviewer:** Completed — security and reliability findings recorded.
- **Stage 3 Improvement:** Completed — refined proposal documented; owner approval pending.
- **Stage 4 Owner Approval:** Pending — no approval of the improved detailed design recorded.
- **Stage 5 Implementation & Verification:** Not started — no implementation authorized or performed.
- **Current gate:** Await explicit owner approval of the refined Stage 3 database architecture scope before implementation planning or code changes.
- **Evidence:** Stage playbooks and the Stage 3 updates listed in their run histories.
