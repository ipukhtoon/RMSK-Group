# Decision Log

Record decisions that the owner explicitly approves. Proposals and research findings must not be promoted to approved decisions without approval.

## D-001 — Pakistan-first market
- **Status:** Approved
- **Decision:** Prioritize Pakistan, Pakistani ISP customers, local market research and applicable Pakistani registration/compliance. International conflicts do not automatically disqualify a name unless they create a credible relevant risk or international expansion is explicitly planned.
- **Rationale:** Initial business focus is Pakistan; global expansion can be considered later and may use a different brand.

## D-002 — Company working identity
- **Status:** Approved working identity; legal registration pending
- **Decision:** Use **RMSK Group** as the company/brand identity.
- **Not implied:** SECP approval, incorporation, trademark clearance or domain ownership.

## D-003 — No separate product brand yet
- **Status:** Approved current state
- **Decision:** No separate product name is currently selected. Do not refer to RMSK Telecom as the approved product or company name unless the owner reintroduces it.

## D-004 — Hybrid ISP SaaS direction
- **Status:** Approved
- **Decision:** Combine subscriber self-service, optional native ISP management and integration-first support for ISPs retaining existing backends.

## D-005 — Do not force backend replacement
- **Status:** Approved
- **Decision:** ISPs with a suitable existing backend should be able to keep it and connect through supported integrations.

## D-006 — Vendor-agnostic modular integration strategy
- **Status:** Approved product direction
- **Decision:** Use modular adapters for different upstream providers, routers, authentication systems, fibre/OLT/ONU equipment and ISP management software. Do not promise universal compatibility at launch.

## D-007 — RMSK Bridge direction
- **Status:** Approved product direction
- **Decision:** Plan a lightweight local Bridge for secure connection to ISP-controlled systems, with unique identity, encrypted communication, least privilege, durable offline queue, retries/backoff, idempotency, monitoring/audit, revocation and secure updates.

## D-008 — Multi-tenant SaaS
- **Status:** Approved
- **Decision:** Strict server-side tenant isolation is mandatory across application, data, jobs, cache, events, storage and integrations.

## D-009 — Payment integrity
- **Status:** Approved
- **Decision:** Support online gateways, direct ISP payments and staff-recorded cash where configured. Require server-side verification, callback protection, idempotency and reconciliation. Payment and service activation states remain separate.

## D-010 — Platform subscription billing separation
- **Status:** Approved
- **Decision:** RMSK Group's SaaS subscription billing is separate from payments collected by an ISP from its subscribers.

## D-011 — Documentation-first
- **Status:** Approved
- **Decision:** Markdown project documents are the source of truth. Update them whenever a feature or business/technical logic is explicitly approved.

## D-012 — Five-stage owner workflow
- **Status:** Approved
- **Decision:** Architect → Critical Reviewer → Improvement → Owner Approval → Implementation & Verification. No implementation before explicit approval.

## D-013 — Initial target segment
- **Status:** Approved direction
- **Decision:** Focus first on small and medium regional fibre and wireless ISPs in Pakistan; do not treat the entire licensed ISP population as the immediate target market.

## D-014 — Naming research scope
- **Status:** Approved
- **Decision:** Prioritize Pakistani company registry, Pakistani trademarks, Pakistani commercial use and .pk/.com.pk domain checks. International use is context, not automatic disqualification.


## D-015 — Controlled ISP registration and tenant onboarding
- **Status:** Approved product requirement / workflow direction
- **Decision:** Provide public ISP applications followed by contact verification, authorized RMSK review and explicit approval/rejection. Provision a separate tenant and initial owner account only after approval. Then guide the ISP owner through a mode-aware onboarding wizard.
- **Application states:** Pending, Under Review, Additional Information Required, Approved, Rejected and Suspended.
- **Lifecycle separation:** ISP application status, tenant lifecycle status and RMSK SaaS subscription/billing status must be separate. Approval does not automatically activate a paid subscription or make the ISP operational/live.
- **Onboarding scope:** Business profile, branding, operating model, network configuration, integrations/Bridge where applicable, packages, payment configuration, staff accounts and go-live checks.
- **Guardrails:** Verify contact channels; review relevant business/licensing claims where applicable; maintain audit records; use idempotent/recoverable tenant provisioning; do not force integrated-mode ISPs to migrate suitable existing backends.
- **Not decided:** Exact form fields, document requirements, verification providers, approval policy/roles, plan names/pricing, billing terms, grace periods and go-live acceptance criteria.
- **Rationale:** Prevent fraudulent or accidental tenant creation, preserve platform control, and separate commercial approval from technical onboarding and subscription activation.

## D-016 — Backend and database technology foundation
- **Status:** Approved technology direction; implementation and production readiness pending
- **Decision:** Adopt NestJS (Node.js/TypeScript) as the backend framework; PostgreSQL as the primary relational database; Supabase as the initial managed PostgreSQL provider; Prisma as the ORM/data-access layer; Redis with BullMQ for cache and background-job processing; and Go for the local RMSK Bridge.
- **Architecture guardrails:** NestJS remains the central business-logic and application authority. Supabase is selected for managed PostgreSQL hosting, not as a requirement to depend on Supabase-specific authentication, functions or other services. Keep the application portable to another PostgreSQL provider. PostgreSQL remains the durable system of record; Redis is not the sole store for critical business or financial records. Background jobs must be tenant-aware, idempotent and observable.
- **Tenant security:** Enforce tenant isolation server-side. Evaluate PostgreSQL row-level security as defense in depth; exact policies and implementation remain subject to security review and testing.
- **Not decided:** Tenant schema/partitioning strategy, exact RLS policies, Supabase plan, Redis hosting/provider, deployment platform/topology, connection-pool settings, backup/recovery configuration, production sizing and implementation details.
- **Rationale:** Establish a maintainable, strongly typed backend and relational data foundation while retaining integration flexibility and avoiding unnecessary provider lock-in.


## D-017 — Shared-table multi-tenant isolation architecture
- **Status:** Approved high-level architecture; detailed design and implementation pending
- **Decision:** Start with one shared PostgreSQL database and shared tenant-owned tables. Assign every ISP a unique, permanent UUID tenant identifier (`tenant_id`). Protect tenant data through defence in depth: NestJS server-side authentication/authorization and membership checks; PostgreSQL Row-Level Security (RLS) that fails closed; and tenant-aware composite constraints and foreign keys.
- **Identity model:** A user account is distinct from an ISP tenant. Tenant membership is explicit. A user may belong to multiple ISPs only when separately authorized for each tenant. Platform-level identities/data are separate from tenant-owned data, and privileged platform access is narrowly scoped and audited.
- **Coverage:** Isolation applies to database records and relationships, cache, BullMQ jobs, events, WebSockets, payment callbacks, Bridge identities, notifications, files, reports and exports. Tenant context must not leak across pooled database connections or concurrent requests.
- **Testing requirement:** Automated negative tests must prove cross-tenant reads, writes, relationship creation, exports and asynchronous access are denied, including missing/invalid tenant context and pooled connection reuse.
- **Evolution:** Preserve the option of dedicated databases or stronger isolation tiers for larger tenants.
- **Not decided:** Full table/schema inventory, exact RLS SQL and trusted context mechanism, Prisma transaction/pooling implementation, membership/role schema, platform-admin break-glass workflow, migration procedure and detailed test suite.
- **Rationale:** Multiple independent barriers ensure a missed application filter or guessed identifier cannot expose or associate another ISP's private data.
