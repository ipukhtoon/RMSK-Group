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
