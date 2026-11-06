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
