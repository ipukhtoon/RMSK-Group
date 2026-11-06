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


## Stage 3 — Refined tenant isolation proposal (pending owner approval)

This is an improved proposal for review, not an approved implementation specification.

### Tenant context and RLS limitations
- NestJS must derive tenant scope from the authenticated principal and a verified, active membership; a tenant ID in a URL, header, request body, job payload or callback is only a selector, never authorization.
- Resolve the membership and permission before opening a tenant-scoped transaction. Define how membership revocation is handled for long-running requests and queued work.
- Establish tenant context transaction-locally on the same Prisma transaction/connection used for all tenant queries. Never use session-level tenant state with pooled connections.
- Important limitation: a custom PostgreSQL setting such as `app.tenant_id` can generally be changed by a database session that has permission to run `set_config`. RLS that trusts only this value protects against forgotten tenant filters, but does not stop a compromised application process or arbitrary SQL running as that role from asserting another tenant. The context mechanism and threat boundary must be explicitly approved after a proof of concept; do not call a freely settable setting an unforgeable identity.
- Fail closed when context is missing, invalid or revoked. No tenant rows should be visible and tenant writes should fail.

### PostgreSQL policy and role requirements
- Runtime API and worker roles must not own tenant tables, be superusers or have `BYPASSRLS`; separate them from migration, backup/restore and controlled operations credentials.
- Enable RLS on every tenant-owned table; assess `FORCE ROW LEVEL SECURITY` and document any unavoidable privileged bypass.
- Design policies to avoid accidental widening. PostgreSQL permissive policies combine with OR; restrictive policies combine with AND. Keep the tenant boundary restrictive and action-specific grants narrow.
- Define `USING` and `WITH CHECK` behavior for SELECT, INSERT, UPDATE and DELETE. Test row movement attempts (changing `tenant_id`) and missing context.
- Use explicit table classification: tenant-owned, platform-owned, or shared/reference data. Each class gets a documented authorization policy; avoid ambiguous unscoped tables.

### Relationship integrity
- Every tenant-owned parent and child carries a non-null tenant ID.
- Use composite uniqueness such as `UNIQUE (tenant_id, id)` and composite foreign keys such as `(tenant_id, customer_id) REFERENCES customers (tenant_id, id)`.
- Apply the same rule to invoice items, payment allocations, services, devices, integration records, audit references and other tenant-owned relationships.
- Prisma schema and migration output must be inspected to confirm the database actually enforces the composite constraints; application-level checks are not a substitute.

### Tenant context across asynchronous and data layers
- BullMQ jobs and events must include a tenant binding and idempotency/correlation identifiers, then revalidate tenant/entity ownership when consumed. A queue payload is not proof of authorization.
- Payment callbacks must resolve the tenant through verified merchant/provider configuration and transaction records, not a freely supplied tenant field.
- Bridge credentials and installations must be permanently bound to one tenant and revocable independently.
- Cache keys, search indexes/filters, WebSocket channels, notification routing, object-storage paths, exports and report generation must be tenant-namespaced and independently authorization-checked.
- Signed file links must be short-lived and issued only after tenant authorization. Logs must redact PII, secrets and network credentials.

### Tenant lifecycle and recoverability
- Provisioning must be idempotent and resumable, with uniqueness constraints preventing duplicate tenant, owner and membership records.
- Model application, tenant lifecycle and SaaS subscription states separately. Define the effect of suspension on staff access, subscriber portal, payments, jobs, Bridge commands and exports.
- Define retention, export, deletion and offboarding rules before launch. Preserve financial and audit records as legally/contractually required.
- Backups, point-in-time recovery, restore drills, RPO/RTO and tenant export verification must be specified and tested. Do not claim a tenant can be restored independently unless the selected backup/restore design proves it.

### Mandatory negative tests
Test two or more tenants with deliberately overlapping local identifiers and attempt cross-tenant reads, writes, deletes, row moves, relationship creation, exports, file access, search, cache lookup, WebSocket subscription, callback replay, Bridge command, job execution and tenant switching. Also test no context, malformed context, revoked membership, forged context, concurrent requests, pooled connection reuse, transaction rollback and timeout cleanup.
