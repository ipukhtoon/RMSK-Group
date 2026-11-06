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
