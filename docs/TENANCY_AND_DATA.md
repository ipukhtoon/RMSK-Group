# Tenancy and Data

## Tenant isolation — mandatory

- Every ISP is a distinct tenant.
- Tenant context must be derived and enforced server-side, never trusted solely from client-supplied tenant IDs.
- Every tenant-owned record, query, cache key, object-storage path, job and event must be tenant-scoped.
- Authorization checks must verify both user permission and tenant membership.
- Background workers and integration callbacks must preserve tenant context.
- Cross-tenant access must fail closed and be covered by automated tests.

## Data ownership and synchronization

For each integration, explicitly define the source of truth for:
- Subscriber/customer identity.
- Packages/plans and service assignment.
- Invoices, balances and payment records.
- Service lifecycle and activation/suspension state.
- Network/authentication state.
- Support cases and notifications.

Do not assume the SaaS database is authoritative for data owned by an ISP's existing backend. Define mapping, conflict resolution, sync direction, timestamps/versioning and reconciliation.

## Data lifecycle

Define retention, archival, export, correction, deletion and tenant-offboarding rules before production use. Preserve legally required financial/audit records while respecting applicable Pakistani law and contracts.

## Sensitive data

Minimize collection and access. Never store plaintext passwords, payment secrets or reusable network credentials. Use managed secrets/encryption and tightly scoped service identities. Do not place credentials or subscriber personal data in logs.

## Open decisions

Database engine, schema strategy, tenant partitioning, encryption model, backup retention and formal data-retention periods remain unselected pending architecture and legal review.
