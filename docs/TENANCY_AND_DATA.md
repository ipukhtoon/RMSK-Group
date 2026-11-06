# Tenancy and Data

## Approved database foundation

- **Database engine:** PostgreSQL.
- **Initial managed provider:** Supabase-hosted PostgreSQL.
- **Application data access:** Prisma, used by the NestJS backend.
- Supabase is the initial database hosting/management provider; avoid unnecessary dependence on Supabase-specific application services so the platform remains portable to another PostgreSQL host.
- PostgreSQL is the durable relational system of record for RMSK-owned data. Redis is not a replacement for durable business or financial records.
- Database schema, migrations, connection pooling, backup/recovery and row-level security policies must be designed and verified before production.

## Tenant isolation — mandatory

- Every ISP is a distinct tenant.
- Tenant context must be derived and enforced server-side, never trusted solely from client-supplied tenant IDs.
- Every tenant-owned record, query, cache key, object-storage path, job and event must be tenant-scoped.
- Authorization checks must verify both user permission and tenant membership.
- Background workers and integration callbacks must preserve tenant context.
- Cross-tenant access must fail closed and be covered by automated tests.
- PostgreSQL row-level security may provide an additional database-level defense-in-depth layer; exact policies and their interaction with Prisma connection pooling and tenant context must be designed and tested. RLS does not replace application authorization.

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

Exact RLS policies and tenant-context propagation, encryption and key-management model, backup retention and recovery objectives, formal data-retention periods, connection-pool configuration and production sizing remain unselected pending detailed architecture and security review. The initial shared database/shared-table model with permanent UUID tenant IDs is approved.


## Approved shared-table tenancy and isolation architecture

The approved initial model is one shared PostgreSQL database with shared tenant-owned tables. Each ISP has a unique, permanent UUID tenant identifier (`tenant_id`). This is an architectural decision; no schema or security control is yet implemented.

### Required controls
- Every tenant-owned row has an explicit, required tenant owner. Platform-level records are separately classified and protected.
- NestJS derives tenant context from the authenticated user and verified tenant membership. Never authorize from a client-supplied tenant ID.
- PostgreSQL RLS protects tenant-owned tables and fails closed when trusted tenant context is absent, invalid or revoked. RLS is defence in depth, not a replacement for backend authorization.
- Tenant-aware composite unique constraints and foreign keys prevent cross-tenant parent/child relationships.
- A user account is distinct from an ISP tenant. Membership is explicit; a person may belong to multiple ISPs only with independently authorized membership in each. Platform identities/data are separate, and privileged platform access is narrowly scoped and audited.
- Scope queries, cache keys, BullMQ jobs, events, WebSockets, payment callbacks, Bridge identities, notifications, file paths, reports and exports to the verified tenant.
- Tenant context must be established through a trusted server-side mechanism within a safe transaction boundary. It must not leak when Prisma/database pooled connections are reused or requests run concurrently.
- Cross-tenant access must fail closed and be covered by automated negative tests, including identifier substitution, cross-tenant relationship creation, missing/invalid context, async processing and connection reuse.

A future dedicated-database or stronger isolation tier may be introduced for larger or contractually distinct ISPs. It is not required for the initial launch.

### Detailed design still required
Complete table inventory and schema, membership/role model, exact RLS SQL and context mechanism, Prisma transaction/pooling patterns, composite key conventions, platform support/break-glass access, migrations and automated test suite must pass the Architect, Critical Reviewer and Improvement stages before implementation.
