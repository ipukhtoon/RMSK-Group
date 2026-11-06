# Testing and Acceptance

## Quality gates

No feature is complete solely because its happy path works. Verify permissions, tenant boundaries, failure states, retries, audit records, data integrity and recovery.

## Required test layers

- Unit tests for domain rules and adapters.
- Integration tests against controlled vendor/API simulators or test systems.
- API contract and schema tests.
- End-to-end tests for key user journeys.
- Tenant-isolation negative tests.
- Role/permission matrix tests.
- Payment callback, duplicate, replay and reconciliation tests.
- Bridge disconnect, offline queue, reconnect and revocation tests.
- Migration, backup and restore tests.
- Performance, load and rate-limit tests.
- Security testing, dependency scanning and penetration testing before production.

## Acceptance criteria template

For every approved feature, document:
1. User/business problem.
2. Actors and permissions.
3. Preconditions and authoritative data source.
4. Happy path.
5. Validation and edge cases.
6. Failure/offline behavior.
7. Audit and notification behavior.
8. Security/privacy implications.
9. Tenant-isolation requirements.
10. Measurable tests and expected outcomes.
11. Rollback/migration considerations.

## Release gates

A release must pass automated tests, manual critical-flow review, security review proportional to risk, migration/backup review and documented known-issue assessment. Owner approval is required at the workflow gate.


## Goldfish QA — persona-based acceptance testing
Goldfish QA is mandatory for substantial user-facing workflows and is integrated into all five stages. It complements technical testing and is not a separate project stage.

### Baseline persona coverage
- First-time visitor: understands purpose and next step.
- New ISP applicant/owner: completes registration and onboarding with clear status and recovery.
- ISP owner/administrator: completes authorized management tasks safely.
- Staff roles: can perform permitted tasks and are blocked from prohibited tasks.
- Subscriber: understands account, package, bill, payment and service status and can complete supported self-service actions.
- Platform support: diagnoses within explicitly granted scope without unintended tenant-data access.
- Mobile/constrained connectivity: key journeys remain usable on phones and communicate offline, pending, timeout and retry states clearly.
- Unfamiliar/error-prone user: wrong IDs, repeated submissions, expired links, missing permissions, tenant switching and unintended actions fail safely.

### Scenario record
For each applicable scenario record: run ID; persona; goal; preconditions and permissions; steps; expected outcome; actual outcome; status (Passed/Failed/Blocked/Not Run/Not Applicable); evidence; severity and defect reference; recovery behavior. For planning stages, label results as predicted/design review rather than tested.


## Planning audit reference
The documentation-only Goldfish QA planning audit is recorded in [GOLDFISH_AUDIT.md](GOLDFISH_AUDIT.md). Its predicted persona findings are not runtime test results. Convert approved journey specifications into executable acceptance tests during implementation and verification.
