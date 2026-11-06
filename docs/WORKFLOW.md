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


## Goldfish QA — mandatory cross-stage perspective review

**Goldfish QA is an additional review lens embedded across all five stages, not a sixth stage and not a replacement for the Architect, Critical Reviewer, Improvement or owner-approval workflow.** Its purpose is to inspect the product as if the reviewer were encountering it for the first time, without relying on internal project knowledge or assuming that a feature is obvious.

### Required perspective personas
Use the personas relevant to the workstream; do not force irrelevant personas into a technical-only task.
- **First-time visitor:** Can they understand what the product does, who it serves and what to do next?
- **New applicant / onboarding ISP owner:** Can they register, understand verification and approval, recover from mistakes and complete setup without hidden knowledge?
- **ISP owner / administrator:** Can they safely manage the business, staff, packages, billing, integrations and settings?
- **Frontline staff:** Can support agents, cashiers, accountants and network operators complete their actual tasks with only their assigned permissions?
- **Subscriber:** Can they find their account, understand bills/payment/service status and complete self-service tasks without confusion?
- **Platform operations/support:** Can RMSK staff diagnose and support a tenant without accidental or unjustified access to its private data?
- **Mobile-first / constrained connectivity user:** Can the key journey work on a phone, with slow, intermittent or lost connectivity and clear recovery from pending/failed actions?
- **Misuse / confused-user perspective:** What happens when someone enters the wrong identifier, repeats a payment/action, follows an expired link, changes tenant context, lacks permission or attempts an unintended action?

### Goldfish QA method
1. Start each scenario with only the information and permissions that persona would actually have. Do not use undocumented internal knowledge.
2. State the persona's goal and walk through the complete journey step by step.
3. At every step ask: What do I see? What do I understand? What action is available? What happens next? How do I know it worked? How do I recover if it failed?
4. Look for hidden assumptions, unexplained terms, ambiguous statuses, missing feedback, unsafe defaults, dead ends, unnecessary steps and accidental data exposure.
5. Test both the happy path and realistic failure/edge paths, including permission denial, duplicate action, stale data, network loss, timeout, retry and recovery where relevant.
6. Record each finding with persona, scenario, expected outcome, observed/design outcome, severity, user/business/security impact, recommendation and acceptance test.
7. Never invent test results. During design stages, label findings as predicted risks; during implementation, label them as tested only when actually executed and evidenced.

### How Goldfish QA applies to the five stages
- **Stage 1 — Architect:** Identify relevant personas and map their end-to-end journeys, assumptions and failure paths in the plan.
- **Stage 2 — Critical Reviewer:** Independently replay the journeys as each relevant persona; challenge whether the design is understandable, safe and complete for someone without project context.
- **Stage 3 — Improvement:** Resolve or explicitly defer Goldfish findings, refine flows and add persona-based acceptance criteria.
- **Stage 4 — Owner Approval:** Present important persona findings, unresolved usability risks and explicit exclusions to the owner; approval remains the owner's decision.
- **Stage 5 — Implementation & Verification:** Execute persona-based acceptance tests on the real implementation, including mobile/responsive and failure/recovery scenarios where applicable; record evidence and defects.

Goldfish QA is mandatory for every substantial user-facing feature and workflow. For backend-only work, apply the relevant operational, support, security and failure-recovery personas. Record a reason when a persona is not applicable.


## Goldfish QA run tracking
Goldfish QA findings must be recorded in the relevant stage run history and acceptance/test records. They do not create a separate approval path. The owner remains the final decision-maker.

### Goldfish QA baseline
- **Status:** Added to the five-stage workflow on 2026-11-06.
- **Run ID:** GOLD-01.
- **Scope:** Cross-stage persona-based first-time-user, role-based, mobile/connectivity, support and misuse review.
- **Result:** Workflow requirement documented; no product journeys or application code have been tested by this documentation update.
- **Evidence:** `docs/stages/01-ARCHITECT.md` through `docs/stages/05-IMPLEMENTATION-VERIFICATION.md`, and `docs/TESTING_AND_ACCEPTANCE.md`.


## Goldfish QA — mandatory cross-stage review

Goldfish QA is an additional review lens embedded in all five stages. It is **not a sixth stage** and does not replace the Architect, Critical Reviewer, Improvement, Owner Approval or Implementation & Verification responsibilities.

### Persona perspectives
Use the perspectives relevant to the workstream:
- First-time visitor: understands the product and next step.
- New ISP applicant/owner: can register, understand application status and complete onboarding.
- ISP owner/administrator: can manage authorized business operations safely.
- Frontline staff: can complete role-specific tasks without excessive permissions.
- Subscriber: can understand account, package, bills, payments and service status.
- Platform support: can diagnose issues without unjustified access to tenant data.
- Mobile/constrained-connectivity user: can complete key tasks on a phone and recover from weak or lost connectivity.
- Unfamiliar or error-prone user: wrong identifiers, repeated actions, expired links, missing permissions and accidental actions are handled safely.

### Method
1. Start with only the knowledge and permissions that the persona would actually have.
2. Walk through the full journey step by step.
3. At each step ask: What do I see? What do I understand? What can I do? What happens next? How do I know it worked? How do I recover if it failed?
4. Identify hidden assumptions, unclear terms/statuses, dead ends, unsafe defaults, unnecessary steps, missing feedback and data exposure.
5. Review happy paths and realistic failure/recovery paths.
6. Record persona, goal, preconditions, steps, expected outcome, finding, severity, impact, recommendation and acceptance test.
7. Distinguish predicted design findings from tests actually executed. Never claim a test passed without evidence.

### Application across the five stages
- **Architect:** identify relevant personas and map their journeys, assumptions and failure paths.
- **Critical Reviewer:** independently replay journeys as someone without internal project knowledge; challenge clarity, safety and completeness.
- **Improvement:** resolve or explicitly defer findings and add persona-based acceptance criteria.
- **Owner Approval:** present important persona findings and unresolved user risks for the owner's decision.
- **Implementation & Verification:** execute the approved scenarios against the actual product and record evidence and defects.

Goldfish QA is mandatory for substantial user-facing features and workflows. For backend-only work, use relevant operations, support, security and recovery perspectives. Record why any persona is not applicable.

### Goldfish QA run record
- Run ID: GOLD-01
- Date: 2026-11-06
- Result: Cross-stage method documented; no product journeys or application code were tested by this documentation change.
- Evidence: stage playbooks and `docs/TESTING_AND_ACCEPTANCE.md`.


## Active workstream — GQA-PLAN-01 (repository and product-planning audit)
- **Stage 1 Architect:** Completed — documentation inventory and persona journey mapping.
- **Stage 2 Critical Reviewer:** Completed — 17 findings classified; 8 High, 9 Medium.
- **Stage 3 Improvement:** Completed — prioritized closure plan, seven golden journeys and readiness gates proposed.
- **Stage 4 Owner Approval:** Pending — audit recommendations have not been approved.
- **Stage 5 Implementation & Verification:** Not started — no application code or product behavior was tested or changed.
- **Scope limitation:** Documentation-only audit of the current main-branch planning files. No running UI, source code, database, payment integration, Bridge or deployment was tested.
- **Primary report:** [Goldfish QA audit](GOLDFISH_AUDIT.md).
- **Run evidence:** stage playbooks 01–03; report commit ec21b7506b27c37e1b0042ab74e65deaa59831ca.


## Updated active workstream — GQA-PLAN-01
- **Stage 1 Architect:** Completed — documentation inventory and persona journey mapping.
- **Stage 2 Critical Reviewer:** Completed — 17 planning findings (8 High, 9 Medium).
- **Stage 3 Improvement:** Completed — added a structured discovery/MVP framework and seven detailed golden journey specifications; finding-to-improvement dispositions recorded.
- **Stage 4 Owner Approval:** Pending — no MVP, first integration, detailed journeys or new planning proposals are approved by this update.
- **Stage 5 Implementation & Verification:** Not started — no application code, live service, database, payment or Bridge behavior was tested or changed.
- **New planning documents:** [Product Discovery and MVP](PRODUCT_DISCOVERY_AND_MVP.md); [Golden User Journeys](GOLDEN_JOURNEYS.md); [Goldfish Audit](GOLDFISH_AUDIT.md).
- **Next gate:** Owner reviews the improved planning package. If approved, conduct evidence-gathering discovery; do not invent interview findings or select a first integration without evidence.
