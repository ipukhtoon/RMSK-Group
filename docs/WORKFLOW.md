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
