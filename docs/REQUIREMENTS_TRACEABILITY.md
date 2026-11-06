# Requirements Traceability

This register links approved product direction to documentation and future validation. Implementation status is not implied.

| ID | Requirement | Status | Primary document | Future verification |
|---|---|---|---|---|
| GOV-001 | Explicit owner approval before implementation | Approved | WORKFLOW | Change/approval audit |
| GOV-002 | Update docs after every approved feature/decision | Approved | WORKFLOW, CHANGELOG | Review documentation with each approved change |
| BIZ-001 | Pakistan-first market | Approved | PRODUCT_VISION, ROADMAP | Validate with Pakistani ISP research |
| BIZ-002 | Initial focus on small/medium regional fibre and wireless ISPs | Approved | PRODUCT_VISION, ROADMAP | Customer interviews and prospect database |
| PROD-001 | Subscriber self-service | Approved direction | PRD | Journey and acceptance tests |
| PROD-002 | Optional native ISP management | Approved direction | PRD | Approved module specifications |
| PROD-003 | Integration-first mode; do not force backend replacement | Approved | ARCHITECTURE, INTEGRATIONS_AND_BRIDGE | Pilot with existing ISP backends |
| INT-001 | Modular vendor-agnostic adapters | Approved direction | ARCHITECTURE, INTEGRATIONS_AND_BRIDGE | Adapter contract and vendor test matrix |
| INT-002 | No universal compatibility claims | Approved | INTEGRATIONS_AND_BRIDGE | Published support matrix |
| INT-003 | Support multiple integration methods | Approved direction | INTEGRATIONS_AND_BRIDGE | Per-method validation |
| INT-004 | Secure local Bridge | Approved direction | INTEGRATIONS_AND_BRIDGE, SECURITY | Threat model and Bridge test suite |
| INT-005 | Compatibility across diverse upstreams, routers, AAA, fibre/OLT/ONU and existing management systems | Approved product direction | INTEGRATIONS_AND_BRIDGE | Prioritized integration matrix and pilots |
| TEN-001 | Strict server-side tenant isolation | Approved | TENANCY_AND_DATA, SECURITY | Cross-tenant negative tests |
| PAY-001 | Online gateway, direct ISP and staff cash patterns | Approved direction | PAYMENTS_AND_LEDGER | Provider and workflow tests |
| PAY-002 | Server-side verification, idempotency, callback protection, reconciliation | Approved | PAYMENTS_AND_LEDGER, SECURITY | Fraud/replay/duplicate/reconciliation tests |
| PAY-003 | Payment state separate from activation state | Approved | PAYMENTS_AND_LEDGER | State-machine tests |
| PAY-004 | SaaS subscription billing separate from ISP subscriber collections | Approved | PAYMENTS_AND_LEDGER, DECISIONS | Ledger/accounting tests |
| SEC-001 | Least privilege, secure secrets and auditability | Approved direction | SECURITY | Security review and tests |
| OPS-001 | Monitoring, backup, recovery and operational runbooks | Approved direction | OPERATIONS_AND_RELIABILITY | Restore drills and incident exercises |
| UX-001 | Responsive customer and staff experience | Approved direction | UX_AND_BRANDING | Accessibility and device tests |
| ID-001 | RMSK Group is approved working identity | Approved working identity | DECISIONS | SECP/trademark/domain checks remain pending |
| ID-002 | No separate product name selected | Current state | DECISIONS | Owner decision if a product name is needed |

## Status vocabulary

- **Proposed:** Under consideration; not approved.
- **Approved:** Owner approved the direction/requirement.
- **Implemented:** Code/configuration exists.
- **Verified:** Required tests and review passed.
- **Deferred:** Deliberately postponed.
- **Rejected:** Explicitly declined.

Do not mark a requirement implemented or verified based only on documentation.


| ONB-001 | Public ISP application with verified contact channels | Approved | PRD, DECISIONS | Application and contact-verification tests |
| ONB-002 | Authorized RMSK review with explicit approve/reject decisions and audit trail | Approved | PRD, SECURITY | Role, decision and audit tests |
| ONB-003 | Provision tenant and initial owner account only after approval | Approved | PRD, ARCHITECTURE | Provisioning, idempotency and failure-recovery tests |
| ONB-004 | Separate application, tenant and SaaS subscription lifecycle states | Approved | PRD, ARCHITECTURE, PAYMENTS_AND_LEDGER | State-transition and billing-isolation tests |
| ONB-005 | Mode-aware ISP onboarding wizard; do not force existing-backend migration | Approved | PRD, ARCHITECTURE | Native/Integrated/Hybrid onboarding acceptance tests |
| ONB-006 | Keep exact fields, verification policy, pricing and go-live criteria open until specified | Approved constraint | PRD, DECISIONS | Review detailed specifications before implementation |

| TECH-001 | NestJS (Node.js/TypeScript) as the central backend framework and business-logic authority | Approved technology direction | ARCHITECTURE, DECISIONS | Architecture review, API and integration tests |
| TECH-002 | PostgreSQL as primary relational database, initially hosted/managed by Supabase | Approved technology direction | ARCHITECTURE, TENANCY_AND_DATA, DECISIONS | Migration, connection, backup/restore and performance tests |
| TECH-003 | Prisma as backend ORM/data-access layer | Approved technology direction | ARCHITECTURE, DECISIONS | Migration and query tests |
| TECH-004 | Redis with BullMQ for cache and asynchronous background jobs | Approved technology direction | ARCHITECTURE, DECISIONS | Retry, idempotency, tenant-context and failure-recovery tests |
| TECH-005 | Go-based local RMSK Bridge | Approved technology direction | ARCHITECTURE, INTEGRATIONS_AND_BRIDGE, DECISIONS | Protocol, security, offline queue and adapter integration tests |
| TECH-006 | Avoid Supabase-specific application lock-in; preserve PostgreSQL portability | Approved constraint | ARCHITECTURE, DECISIONS | Review application dependencies and provider migration path |


| TEN-002 | Shared PostgreSQL database and shared tenant-owned tables initially, with permanent UUID tenant IDs | Approved architecture | ARCHITECTURE, TENANCY_AND_DATA, DECISIONS | Schema review and tenant ownership audit |
| TEN-003 | NestJS membership/permission checks plus PostgreSQL RLS; fail closed without trusted tenant context | Approved architecture | TENANCY_AND_DATA, SECURITY | RLS policy and authorization negative tests |
| TEN-004 | Tenant-aware composite constraints and foreign keys prevent cross-tenant relationships | Approved architecture | TENANCY_AND_DATA | Database constraint tests |
| TEN-005 | User identity is separate from tenant membership; independently authorize each tenant membership | Approved architecture | TENANCY_AND_DATA, SECURITY | Membership, tenant-switch and revocation tests |
| TEN-006 | Tenant-scope cache, jobs, events, WebSockets, callbacks, Bridge, files, reports and exports | Approved architecture | ARCHITECTURE, TENANCY_AND_DATA, SECURITY | Cross-layer isolation tests |
| TEN-007 | Prevent tenant-context leakage across pooled connections and concurrent requests | Approved security requirement | TENANCY_AND_DATA, SECURITY | Prisma/pool concurrency and connection-reuse tests |
| TEN-008 | Automated cross-tenant negative testing is required before production | Approved release requirement | TENANCY_AND_DATA, SECURITY | Security test suite and release gate |
| TEN-009 | Preserve future dedicated-database isolation tier option | Approved evolution constraint | ARCHITECTURE, TENANCY_AND_DATA | Architecture review before tier introduction |


## Stage 3 — Detailed tenancy/security improvement requirements (proposed; pending owner approval)

| ID | Requirement | Status | Primary document | Future verification |
|---|---|---|---|---|
| TEN-010 | Tenant context must be established transaction-locally on the same Prisma transaction/connection as tenant queries; session-level tenant state is prohibited | Proposed | ARCHITECTURE, TENANCY_AND_DATA | Concurrent pooled-connection and rollback/timeout tests |
| TEN-011 | Document the threat limitation of freely settable PostgreSQL custom tenant settings; select a context assertion only after threat-model review | Proposed | ARCHITECTURE, SECURITY | Forged-context and compromised-role proof-of-concept |
| TEN-012 | Runtime API/worker credentials are separate from migration and operations credentials and cannot own tenant tables or bypass RLS | Proposed | SECURITY, ARCHITECTURE | Privilege inspection and bypass-attempt tests |
| TEN-013 | RLS policy composition must preserve a restrictive tenant boundary; policy additions are reviewed for permissive OR widening | Proposed | SECURITY, TENANCY_AND_DATA | Policy-composition regression tests |
| TEN-014 | Tenant-owned relationships use database-enforced composite foreign keys and unique constraints | Proposed refinement | TENANCY_AND_DATA | Migration/schema inspection and cross-tenant FK tests |
| TEN-015 | Async jobs, callbacks and Bridge commands revalidate trusted tenant/entity binding at execution | Proposed | ARCHITECTURE, SECURITY | Forged/mismatched job, callback and Bridge tests |
| TEN-016 | Cache, search, files, WebSockets, notifications, reports and exports enforce tenant scope independently | Proposed | ARCHITECTURE, TENANCY_AND_DATA, SECURITY | Cross-layer isolation tests |
| TEN-017 | Platform support and break-glass access use scoped, time-limited, reason-coded and audited grants | Proposed | ARCHITECTURE, SECURITY | Authorization, expiry and audit tests |
| TEN-018 | Provisioning, suspension, offboarding and recovery have idempotent workflows and tested lifecycle safeguards | Proposed | ARCHITECTURE, TENANCY_AND_DATA | Retry, lifecycle and restore-drill tests |
| TEN-019 | Production security gate includes adversarial tenant-isolation tests and independent security review | Proposed | SECURITY, TESTING_AND_ACCEPTANCE | Release checklist and review evidence |
