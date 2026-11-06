# Roadmap

This is a staged planning outline, not authorization to implement.

## Phase 0 — Discovery and foundations
- Confirm target ISP segment and workflows in Pakistan.
- Build an initial prospect database of small/medium regional fibre and wireless ISPs.
- Interview prospective ISPs and validate willingness to pay.
- Inventory common billing systems, routers, AAA and fibre/OLT environments.
- Define MVP scope and measurable acceptance criteria.
- Complete threat model, data map and legal/compliance discovery.

## Phase 1 — Platform foundation
- Tenant lifecycle and isolation.
- Identity, roles and audit.
- Core subscriber/account model.
- Platform operations, backups and observability.
- API and adapter contracts.

## Phase 2 — First usable customer/ISP workflows
- Subscriber self-service baseline.
- Payment intent/verification and ledger foundation.
- Native management capabilities only where required for MVP.
- First validated integration(s) chosen from market evidence.

## Phase 3 — Bridge and integrations
- Secure Bridge identity and communication.
- Durable queue, retry, revocation and update lifecycle.
- First supported adapters.
- Integration diagnostics and reconciliation.

## Phase 4 — Pilot and controlled launch
- Pilot with selected Pakistani ISPs.
- Security and tenant-isolation testing.
- Payment and financial reconciliation.
- Operational runbooks and support readiness.
- Measure onboarding, reliability and adoption.

## Phase 5 — Expansion
- Add integrations based on verified demand.
- Improve white-labeling, analytics and automation.
- Consider other markets only when commercially justified.

## Prioritization rule

Prioritize customer pain, market demand, security, reliability and integration leverage—not feature count. Each phase requires explicit owner approval before implementation.


## Proposed discovery-to-MVP gate (pending owner approval)

Before Phase 1 implementation scope is committed:
1. Use `docs/PRODUCT_DISCOVERY_AND_MVP.md` to interview a varied set of Pakistani ISPs and record evidence, not assumptions.
2. Select the first target segment, pilot operating mode, first integration and bounded outcome using the documented scoring method plus hard safety gates.
3. Specify the pilot's relevant journeys using `docs/GOLDEN_JOURNEYS.md`.
4. Resolve the pilot's source-of-truth/action-authority map, financial states, permissions, failure/recovery and acceptance tests.
5. Approve support, rollback, legal/provider checks and measurable pilot success criteria.

This is a proposed gate, not an approved selection of a particular ISP segment, mode, vendor, integration or feature scope.
