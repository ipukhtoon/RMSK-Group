# Architecture

**Status:** Product and technology directions approved; detailed implementation, deployment topology and security validation remain pending.

## Approved backend and database foundation

The owner has approved the following proposed foundation for RMSK Group:

- **Backend framework:** NestJS on Node.js with TypeScript.
- **Primary database engine:** PostgreSQL.
- **Managed PostgreSQL provider:** Supabase, initially as the hosting/management layer for PostgreSQL.
- **ORM and database access:** Prisma.
- **Cache and queue infrastructure:** Redis with BullMQ for background jobs and asynchronous processing.
- **Local ISP integration agent:** Go-based RMSK Bridge.

NestJS is the central application and business-logic authority. Prisma is the application’s database access layer. PostgreSQL is the authoritative relational data store for RMSK-owned platform and tenant data, subject to explicit source-of-truth rules for integrated ISP systems.

Supabase is selected as the initial managed PostgreSQL provider, not as a requirement to build the application around Supabase-specific authentication, functions, realtime or other proprietary services. Keep application logic and data access portable so the PostgreSQL database can be hosted elsewhere if business, cost, reliability or scaling requirements later justify a move.

Use appropriate pooled connections for normal application traffic and a direct database connection for schema migrations and administrative operations where required by the provider. Connection limits, pooling mode and Prisma compatibility must be validated before production deployment.

Redis/BullMQ will handle appropriate transient cache and asynchronous job workloads; PostgreSQL remains the durable system of record. Job handlers must be idempotent, tenant-aware and observable. Do not treat Redis as the only store for critical financial or business records.

The Go Bridge is a separately deployable local integration agent. It communicates with the central platform through a versioned, authenticated protocol and must follow the security and reliability requirements in the integration and security documents.

**Approval boundary:** This approves the technology direction only. It does not mean the stack has been implemented, deployed, load-tested, security-reviewed or approved for production. Exact hosting topology, Supabase plan, Redis provider/hosting, deployment platform, schema design, tenant isolation implementation, backup configuration and operational sizing remain subject to detailed design and verification.

## Architectural shape

Use a modular backend with clear domain boundaries. The initial application may be delivered as a modular monolith rather than prematurely splitting into microservices. Modules should have explicit interfaces and ownership so that independently scaling or extracting a component remains possible if evidence later supports it.

The platform includes:
- Web/API application layer.
- Tenant-aware domain services.
- Data and event layer.
- Integration/adapters layer.
- Optional native ISP management modules.
- Customer self-service experience.
- RMSK Bridge for secure connections to local ISP environments.
- Platform operations, monitoring and support capabilities.

## Three operating modes

1. **Native mode:** ISP uses RMSK's own approved management modules.
2. **Integrated mode:** ISP keeps its current backend; RMSK connects through adapters/Bridge.
3. **Hybrid mode:** supported functions are split between RMSK and the ISP's existing systems, with explicit ownership for each data domain and action.

## Adapter architecture

- Define stable internal domain contracts.
- Implement vendor/protocol-specific adapters behind those contracts.
- Keep vendor-specific logic out of core business workflows.
- Version adapters and document supported versions/capabilities.
- Validate read/write behavior, error handling, retries and idempotency.
- Allow capability-based feature availability; do not assume every adapter supports every operation.

## Integration methods under consideration

REST APIs, webhooks, MikroTik RouterOS API, RADIUS, SNMP, local Bridge, selected OLT/ONU APIs or protocols, CSV/Excel exchange, and controlled manual workflows. This is a strategy list, not a claim that each integration is implemented.

## Data ownership

For each tenant and integration, define the authoritative system for subscriber identity, plans, balances, invoices, payments, service status, credentials and network state. Avoid uncontrolled dual writes. Use explicit synchronization direction, conflict handling and reconciliation.

## Resilience

- Durable queues for transient integration failures.
- Retry with exponential backoff and jitter.
- Idempotency keys for retried operations.
- Dead-letter/quarantine handling and operator visibility.
- Backpressure and rate limits.
- Reconciliation after recovery.
- Safe manual fallback where automated actions are unavailable.

## Architecture decisions still open

Detailed domain boundaries, API contract/versioning, deployment topology, exact RLS policies and tenant-context propagation, encryption/key management, Redis hosting/provider, backup and recovery configuration, scaling thresholds and exact service boundaries require further design and review. The initial shared-database/shared-table tenancy model is approved; exact implementation remains open. The selected database engine and initial managed PostgreSQL provider are recorded above and in the decision log.


## ISP application, approval and provisioning flow
- Model ISP applications separately from tenant records and SaaS subscription records.
- A public application is stored in a restricted platform-level context; it is not itself a tenant.
- Authorized platform staff review and decide applications. Record reviewer, timestamps, decision, reason and supporting verification evidence with appropriate access controls.
- On approval, execute an idempotent provisioning workflow that creates the tenant boundary, unique tenant identifier, initial owner membership/account and default configuration.
- Provisioning must be transactional where possible; otherwise use a durable workflow with compensating/recovery steps and duplicate prevention.
- Keep application, tenant lifecycle and subscription lifecycle state machines separate.
- The owner onboarding wizard configures the tenant after provisioning. Steps are conditional on Native, Integrated or Hybrid operating mode.
- Tenant isolation controls apply from the first provisioned record onward; no client-supplied tenant identifier is trusted as authorization.
- No tenant is automatically marked paid or live solely because its application is approved.


## Approved shared-table tenant isolation model

The owner has approved one shared PostgreSQL database with shared tenant-owned tables as the initial tenancy model. Each ISP receives a unique, permanent UUID tenant identifier (`tenant_id`).

- Every tenant-owned record must carry its owning tenant identifier; platform-level records are explicitly classified and separately protected.
- NestJS derives tenant context from authenticated identity and verified membership. A client-supplied tenant ID is never proof of authorization.
- PostgreSQL Row-Level Security (RLS) is required as a database-level isolation boundary and must fail closed when trusted tenant context is missing or invalid. RLS supplements, not replaces, NestJS authorization.
- Tenant-aware composite unique constraints and foreign keys prevent cross-tenant relationships.
- User identity is separate from tenant membership. Multi-tenant users require explicit membership and authorization for each active tenant. Platform access is least-privilege and audited.
- Tenant scope applies to queries, cache, jobs, events, WebSockets, callbacks, Bridge identities, notifications, files, exports and reports.
- Tenant context must be transaction-scoped and must not leak across Prisma/database pooled connections or concurrent requests.
- Automated negative tests must prove cross-tenant reads, writes, relationship creation and cross-layer access are denied.
- Preserve a future path to dedicated databases or stronger isolation tiers for larger ISPs.

Exact schema, RLS SQL, trusted context mechanism, Prisma transaction/pooling pattern, membership/role schema and test implementation remain pending detailed architecture and review. This approval is not an implementation or production-readiness claim.


## Stage 3 — Refined detailed database architecture proposal (pending owner approval)

**Status:** Improvement-stage proposal only. This section refines D-017 but does not create a new approved decision, authorize implementation, or claim that controls exist.

### Security model and threat boundary
Use three complementary layers:
1. NestJS authenticates the principal, resolves the requested tenant from the authenticated session, verifies an active membership and the required permission, and creates a server-controlled tenant execution context.
2. PostgreSQL RLS restricts tenant-owned rows as defence in depth, including protection against accidentally omitted tenant predicates.
3. Tenant-aware composite keys and foreign keys prevent records from different tenants being related even when identifiers are known.

RLS based on a transaction-local custom PostgreSQL setting (for example, `app.tenant_id`) is useful against accidental query-scope omissions, but it is **not by itself a security boundary against a compromised application process or arbitrary SQL executed with the application database role**: that role may be able to set a custom setting. The threat model must state this limitation explicitly. Before implementation, compare and test candidate context-assertion designs, including how the database can verify the asserted user/tenant membership, who can mint assertions, secret/key exposure, expiry and revocation. Do not describe a client-supplied or freely settable GUC as cryptographically trusted. If no stronger verifiable mechanism is selected, document the residual risk and rely on hardened NestJS authorization, parameterized queries, restricted database privileges, and defence-in-depth controls rather than overstating RLS guarantees.

### Database roles and policy rules
- Use separate credentials/roles for application runtime, migration, controlled operations, backup/restore and any reporting workload. Do not reuse a migration or owner credential in the API or worker.
- The runtime role must not own tenant tables, be a superuser, or have `BYPASSRLS`. Revoke unnecessary DDL, role-management and schema privileges.
- Enable RLS on every tenant-owned table and evaluate `FORCE ROW LEVEL SECURITY`; explicitly inventory unavoidable owner/superuser bypass paths.
- Prefer restrictive tenant-boundary policies combined with narrowly scoped permissive action policies. Review PostgreSQL's policy-composition semantics: permissive policies combine with OR, while restrictive policies combine with AND. A new permissive policy must never silently widen the tenant boundary.
- For each operation, define both row visibility/eligibility (`USING`) and resulting-row validity (`WITH CHECK`) where applicable. Missing, malformed or absent context must return no tenant rows and reject tenant writes.
- Keep platform-level tables outside tenant policies only when their classification and separate access controls are explicit. Do not create a broad platform policy on tenant tables as a shortcut.

### Prisma transaction and pool contract
- Tenant-scoped work must execute through one interactive transaction/client and one database connection for the entire unit of work.
- Establish context transaction-locally at the start of that transaction; never rely on session-level `SET` or state surviving a commit/rollback.
- Resolve and authorize membership before entering the tenant transaction; re-check revocation-sensitive operations according to a defined freshness policy.
- Prohibit tenant-scoped queries through the root Prisma client outside the approved transaction wrapper. Centralize the wrapper and make unscoped access explicit and separately reviewed.
- API and worker traffic may use a compatible pooled connection URL. Migrations and controlled schema/administrative tasks use a direct connection. Validate the exact Supabase pool mode, Prisma version/driver adapter, prepared-statement behavior, transaction timeouts and provider limits in a proof of concept.
- On transaction failure, timeout or cancellation, discard the request context and ensure the next borrower of a pooled connection starts with no tenant state.

### Async and cross-layer tenant contract
Every queued job, event, WebSocket subscription, payment callback, Bridge installation, notification, file/object, cache entry, search request, report and export must carry or resolve an immutable tenant binding from a trusted server-side record. Validate that binding at the point of execution/delivery; do not trust a tenant ID copied from an untrusted payload.
- BullMQ job envelope: tenant ID, job type/version, entity reference, idempotency key, correlation ID and enqueue metadata; validate tenant/entity ownership before work.
- Payment callback: resolve tenant using a verified provider/merchant mapping and transaction reference, then validate amount, currency, status and idempotency; never select a tenant from an arbitrary callback field alone.
- Bridge: bind each installation credential/certificate to one tenant and installation record; enforce capability scopes, revocation, replay protection and action-level authorization.
- Cache/search/storage/export: include tenant scope in keys, namespaces and authorization checks; use short-lived signed downloads and re-check access at download time.
- Logs and telemetry: include safe tenant correlation identifiers where operationally needed, but redact subscriber PII, payment secrets, network credentials and tokens. Prevent tenant data from appearing in metric labels or shared error messages.

### Platform access
Separate platform operations permissions from access to ISP business data. Support access must be tenant-specific, reason-coded, time-limited, read-only by default and fully audited. Sensitive write access requires separate explicit authorization. Emergency break-glass must require strong authentication, a narrowly scoped temporary grant, reason/ticket, immutable audit trail and post-use review. Do not silently bypass RLS using a normal administrator session.

### Provisioning, migrations and recovery
- Provisioning is a durable, idempotent state machine with a unique application-to-tenant relationship and unique tenant owner membership. Each step records status and idempotency key; retries resume or safely compensate rather than create duplicates.
- Use expand-and-contract migrations for risky changes. Apply migrations through a dedicated credential and controlled release path; test against a production-like copy and verify tenant constraints/RLS after each migration.
- Backups must be encrypted and access-controlled. Define retention, point-in-time recovery availability, RPO/RTO and restore ownership before production. Perform scheduled restore drills, including tenant-scoped export/restore validation and financial reconciliation.
- Tenant suspension, offboarding, deletion, legal retention and data export need explicit state transitions and safeguards; suspension must not accidentally erase financial history or revoke required audit evidence.

### Required proof-of-concept and release gates
Before production implementation approval, demonstrate: cross-tenant read/write denial; missing/invalid/forged context behavior; role and policy composition; pooled connection reuse under concurrent tenant requests; rollback/timeout cleanup; composite-FK rejection; worker tenant binding; callback replay/tenant-mismatch rejection; Bridge credential revocation; tenant-scoped files/search/exports; provisioning retry safety; and backup restoration. Record tested versions, configuration and results. Passing a local proof of concept is not a production security certification.
