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
