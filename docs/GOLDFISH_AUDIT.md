# Goldfish QA — Repository and Product Planning Audit

**Run ID:** GQA-PLAN-01  
**Date:** 2026-11-06  
**Scope:** Documentation and product-planning review of the current `main` branch.  
**Method:** Documentation-based Goldfish walkthroughs plus independent Critical Reviewer assessment.  
**Status:** Stage 1, Stage 2 and Stage 3 completed as a planning exercise. Proposed improvements await explicit owner approval.  
**Important limitation:** The repository currently provides product and architecture documentation, not a running product/UI. No application behavior, API, database, payment, Bridge or mobile journey was executed. Findings below are design/documentation findings, not failed runtime tests.

## 1. Executive assessment

The product direction is coherent and worth preserving: Pakistan-first ISP SaaS, Native/Integrated/Hybrid operating modes, no forced backend replacement, vendor-agnostic adapters, RMSK Bridge, subscriber self-service, optional native ISP management, strict tenant isolation and separation of RMSK SaaS billing from ISP subscriber collections.

The documentation is a strong foundation, but it is **not yet a complete implementation-ready product plan**. It explains what RMSK intends to become more clearly than it explains exactly how a real ISP owner, staff member or subscriber completes the core tasks, how data ownership works in hybrid mode, how the MVP is selected, or how commercial and operational success will be measured.

**Overall verdict:** Directionally sound; conditionally ready for the next product-definition work; not ready for application implementation. Preserve the approved principles and stack. Close the High-priority planning gaps and obtain owner approval before implementation.

## 2. Goldfish persona walkthroughs

These are predicted design walkthroughs based on the documented product plan. They are not usability tests of a running application.

### Persona A — First-time visitor / prospective ISP
**Goal:** Understand the offer and decide whether to apply.
1. Finds RMSK and tries to understand whether it is for an ISP of their size and technology.
2. Compares Native, Integrated and Hybrid modes.
3. Wants to know supported existing systems, pricing approach, onboarding requirements and support.
4. Starts an ISP application and wants to know what information/evidence is required.
5. Verifies contact details, submits and needs a reference/status page.
6. Responds to an additional-information request and waits for a decision.

**Gaps:** No documented public website information architecture, eligibility explanation, application form validation, save/resume, reference number, status tracking, privacy notice/consent, expected review time or applicant communication journey. No clear way to avoid promising an integration before compatibility assessment.

**Proper solution proposal:** Define a public prospect-to-application journey with plain-language mode comparison, an integration compatibility questionnaire, transparent required/optional fields, save/resume, verified contact channels, unique application reference, clear state explanations, secure evidence upload, status notifications and an explicit statement that RMSK approval is platform approval—not proof of PTA licensing or legal status.

### Persona B — New ISP owner / onboarding
**Goal:** Get an approved ISP tenant safely configured and ready for a pilot.
1. Receives approval and activates the initial owner account.
2. Sets business profile, timezone, currency and branding.
3. Chooses Native, Integrated or Hybrid operating mode.
4. Maps existing systems and selects which system owns each data domain/action.
5. Configures network/integration credentials and validates capabilities.
6. Sets packages/pricing and payment options where applicable.
7. Invites staff with appropriate roles.
8. Runs readiness checks and understands what remains before go-live.

**Gaps:** The onboarding topics are listed, but there is no step sequence, progress/resume model, role invitation and recovery flow, integration test wizard, source-of-truth mapping, readiness checklist, test mode or safe go-live gate. It is unclear which setup steps are mandatory in each mode.

**Proper solution proposal:** Create a mode-aware onboarding state machine with save/resume, step prerequisites, clear completion status and a final go-live checklist. Integrated mode should ask the owner to connect and map existing systems—not recreate subscriber, package or billing data in RMSK. Require a per-domain source-of-truth and per-action authority matrix before enabling writes. Keep test connection and read-only validation ahead of write/activation privileges.

### Persona C — ISP owner / administrator
**Goal:** Understand the health and financial/operational state of the ISP.
**Gaps:** No defined dashboard information hierarchy, operational KPIs, alert priorities, subscriber lifecycle, package change, service suspension/reactivation, staff delegation, audit review or exception handling workflows.

**Proper solution proposal:** Define a small set of owner jobs-to-be-done and dashboard decisions first: subscriber/service status, unpaid/overdue accounts, payment exceptions, integration/Bridge health, service incidents and tasks needing approval. Specify each workflow with preconditions, permission, confirmation, audit, resulting state and recovery path. Avoid a dashboard full of vanity metrics.

### Persona D — Frontline staff (support, cashier, accountant, network operator)
**Goal:** Complete a narrow daily task without seeing or changing unrelated information.
**Gaps:** Roles are provisional categories rather than a permission matrix. There are no task-specific workflows for cash collection, receipt correction, refund/adjustment, subscriber lookup, support notes, service activation or network actions. Financial and network permissions are said to be separated but not mapped to concrete actions.

**Proper solution proposal:** Build a permission catalogue around actions and resources, then map roles to permissions. Specify high-risk actions separately (cash adjustment, refund, service suspension, activation, credential reveal/export, role changes). Require confirmation/reason and immutable audit for sensitive actions; use dual approval only where risk/business policy justifies it. Include denied-action and staff-offboarding scenarios.

### Persona E — Subscriber
**Goal:** Check account/service, pay a bill and know what happens next.
**Gaps:** The PRD lists screens/capabilities but no end-to-end sign-in/account-linking, identity recovery, bill detail, payment selection, pending payment, receipt, refund/dispute, service activation delay or support journey. Payment and activation are correctly separate, but customer-facing explanations are not specified.

**Proper solution proposal:** Define the subscriber journey from account invitation/verification to account recovery, account/service overview, invoice explanation, payment intent, provider confirmation, receipt, separate service-state update and support escalation. Always show distinct states such as payment pending/confirmed/failed/refunded and service pending/active/suspended, with timestamps and plain-language next steps. Prevent duplicate payment submission and show a safe retry/reconciliation path.

### Persona F — Finance staff / cashier
**Goal:** Record, reconcile and correct money movements accurately.
**Gaps:** Ledger principles are sound but insufficient to implement. The documents do not define invoice/payment/refund/adjustment state transitions, allocation rules, partial payments, overpayments, reversals, chargebacks, settlement matching, reconciliation exceptions or accounting exports.

**Proper solution proposal:** Define an immutable financial event/ledger model and explicit state machines before implementation. Decide whether the accounting requirement needs a balanced double-entry ledger or a simpler auditable subledger; do not choose silently. Model payment intent, provider transaction, receipt, allocation, refund/reversal and settlement as distinct concepts with idempotency and traceability. Define rounding, currency, taxes, fees and date/time rules. Add reconciliation queues and permission-controlled corrections.

### Persona G — Platform support / operations
**Goal:** Diagnose an ISP issue without gaining unjustified access to private subscriber data.
**Gaps:** Scoped support access is proposed, but there is no support case workflow, tenant-consent/authorization model, exact grant scope, approval/expiry/revocation flow, access visibility to the ISP owner or break-glass operational procedure.

**Proper solution proposal:** Define support access as a separate auditable grant: tenant, purpose/ticket, allowed actions/data categories, requester/approver, expiry and revocation. Default to metadata/health diagnostics and read-only access. Make sensitive data access separately gated and visible in the tenant audit trail. Define emergency access, notification and post-incident review.

### Persona H — Mobile / weak-connectivity user
**Goal:** Complete important work on a phone despite slow or interrupted connectivity.
**Gaps:** Mobile-first and Bridge offline operation are principles, not per-workflow behavior. The documents do not distinguish browser/app connectivity from ISP Bridge connectivity, nor specify which user actions may be queued, which must be online, or how stale data is labelled.

**Proper solution proposal:** Define online/offline capability per action. Show last-sync time and data freshness; preserve unsent form work; make retries safe; prevent duplicate payments and unsafe network commands; label pending actions; provide explicit retry/cancel/reconcile behavior. Do not imply that all staff/subscriber workflows work offline.

### Persona I — Confused, mistaken or malicious user
**Goal:** Avoid accidental damage or unauthorized access.
**Gaps:** Security principles are broad; journeys for wrong subscriber selection, tenant switching, duplicate clicks, stale invitations, expired payment links, wrong merchant mapping, role escalation and destructive network actions are not fully specified.

**Proper solution proposal:** Add negative journeys and confirmation patterns for high-impact actions. Revalidate resource ownership server-side, use idempotency for retries, require re-authentication/step-up for sensitive operations, make invitations single-use/expiring/revocable, and never reveal whether another tenant's records exist.

## 3. Critical findings and priority

Severity reflects planning risk, not an observed production vulnerability.

| ID | Severity | Finding | Why it matters | Recommended solution | Implementation blocker? |
|---|---|---|---|---|---|
| GQA-01 | High | No end-to-end golden journeys and detailed state machines | Teams may implement disconnected screens with missing failure/recovery paths | Specify journeys for application, onboarding, subscriber payment, staff operations, support and integration lifecycle | Yes, for those workflows |
| GQA-02 | High | Hybrid data ownership is not operationally defined | Conflicting writes can corrupt subscriber, invoice, payment or service state | Create per-tenant, per-domain source-of-truth and per-action authority matrix, sync direction, conflict policy and reconciliation | Yes, before hybrid writes |
| GQA-03 | High | MVP and first integration are not selected from customer evidence | Scope may expand across many vendors and features without a usable pilot | Conduct structured ISP discovery, rank pain/market frequency/integration feasibility/support cost, then approve one narrow pilot segment and integration | Yes, before MVP build |
| GQA-04 | High | Financial domain is not sufficiently specified | Duplicate/partial/out-of-order events can create incorrect balances and poor reconciliation | Specify immutable financial records, payment lifecycle, allocations, reversals/refunds, settlement and reconciliation; decide accounting model explicitly | Yes, before financial implementation |
| GQA-05 | High | Identity, authentication and account recovery are open | Staff/subscriber identity linking and account takeover/recovery paths are unclear | Decide auth architecture/provider, user identity model, tenant membership, subscriber linking, MFA/step-up, invitations, recovery and revocation | Yes, before auth implementation |
| GQA-06 | High | Tenant lifecycle and suspension consequences are unspecified | Suspension could block legitimate payments/support or leave privileged integrations active | Define application, tenant, subscription and operational states plus transition matrix and per-capability suspension behavior | Yes, before lifecycle implementation |
| GQA-07 | High | Permission catalogue and role matrix are missing | Provisional role labels cannot enforce least privilege consistently | Define resource/action permission catalogue and role matrix; separate finance, support, network and platform permissions | Yes, before protected workflows |
| GQA-08 | High | Tenant RLS/context threat model remains unresolved | A freely settable tenant context can be forged by a compromised app role; claims about RLS strength may be overstated | Select and prove a trusted context design, runtime privileges, policy composition and residual threat boundary; adversarially test it | Yes, before tenant-data implementation |
| GQA-09 | Medium | Bridge offline queue semantics are incomplete | Stale or unsafe commands may execute after reconnect | Classify queueable vs online-only operations; define expiry, ordering, dedupe, cancellation, conflict and operator approval | Yes, before write-capable Bridge actions |
| GQA-10 | Medium | SaaS commercial model is not defined | ISP onboarding cannot explain trial, plan limits, suspension, renewal or billing support | Define plans, metering/limits, trial/grace, invoices, taxes, payment and suspension policy as a commercial decision | Yes, before paid launch |
| GQA-11 | Medium | Operational targets and support model are absent | Hosting, alerting, staffing and cost cannot be sized responsibly | Define SLOs, RTO/RPO, backup retention, support hours, severity levels, incident communications and capacity assumptions | Yes, before production |
| GQA-12 | Medium | API/event contracts and compatibility lifecycle are abstract | Integrations may diverge and become difficult to support/version | Define canonical contracts, adapter capability model, version/deprecation policy, webhook delivery and error taxonomy | Yes, before external integration commitments |
| GQA-13 | Medium | Pakistan legal/compliance discovery is only a general note | Product may collect/process data or payments without clear obligations | Obtain qualified Pakistan-specific legal/accounting/payment advice; map data categories, contracts, retention and provider requirements | Yes, before public launch |
| GQA-14 | Medium | Goldfish workflow documentation has duplicate sections and inconsistent run bookkeeping | The process itself may become hard to trust | Consolidate one canonical Goldfish section, maintain one run ID/status and link evidence | No product blocker; fix documentation |
| GQA-15 | Medium | UX, localization and accessibility are principles only | Mobile-first promise is not testable | Define language priority, terminology glossary, responsive breakpoints, accessibility criteria and usability acceptance tests | Before UX implementation |
| GQA-16 | Medium | No numeric product success metrics | Cannot judge pilot success or whether integrations deliver value | Define onboarding, sync accuracy, payment reconciliation, support burden, Bridge health, task completion and adoption measures with baselines/targets | Before pilot exit |
| GQA-17 | Medium | No application/runtime repository was inspected or tested in this audit | Cannot validate real code, screens, build, API, schema or security controls | Treat this as documentation-only; run a separate code/UI/technical QA when implementation exists | Before claiming runtime verification |

## 4. Recommended product-definition sequence

Do not try to specify every possible module at once. Work in this order:

### Workstream A — Validate the first customer and MVP
1. Interview a small but diverse cohort of Pakistani regional fibre/wireless ISPs.
2. Record current billing/CRM/AAA/router/OLT tools and pain points with evidence.
3. Identify the most common and feasible integration pair.
4. Select one pilot persona, one operating mode and one end-to-end business outcome.
5. Define measurable pilot success and support cost.

**Deliverable:** evidence-backed MVP scope and first integration decision.

### Workstream B — Define the golden journeys
Specify application/review/provisioning, owner onboarding, subscriber access/payment, staff cash/finance/support/network tasks, integration setup/recovery and platform support. Each journey must have permissions, states, failure handling, audit and acceptance criteria.

**Deliverable:** user journey and state-transition specifications.

### Workstream C — Resolve data ownership and domain models
Create a data ownership matrix for Native, Integrated and Hybrid modes. Define canonical subscriber/service/invoice/payment models, mapping, sync direction, conflicts, versioning and reconciliation. Define financial model before payment coding.

**Deliverable:** domain model, source-of-truth matrix and financial lifecycle specification.

### Workstream D — Close identity, permissions and tenant security
Decide authentication/account recovery, tenant membership, subscriber linking, permission catalogue, support grants and trusted database tenant context. Prove RLS/Prisma/pooling behavior with adversarial tests.

**Deliverable:** security architecture and authorization matrix.

### Workstream E — Define operations and commercial readiness
Specify RMSK SaaS plans, subscription lifecycle, operational targets, backup/restore, support, legal/compliance discovery, monitoring and incident response.

**Deliverable:** launch operating model and go-live checklist.

## 5. Recommended golden journeys to specify next

Priority order:
1. ISP application → review → approval → idempotent tenant provisioning → owner activation.
2. Owner onboarding by Native / Integrated / Hybrid mode → source-of-truth mapping → integration validation → go-live.
3. Subscriber sign-in/recovery → account/service view → invoice → payment → confirmation → separate service activation status.
4. Cashier/finance staff → cash receipt or payment reconciliation → correction/refund with authorization and audit.
5. Support/network staff → subscriber lookup → diagnostics → authorized service action → audit and customer communication.
6. Bridge installation → tenant binding → capability test → offline/reconnect → safe sync/reconciliation → revoke.
7. Platform support → scoped grant → diagnostics → expiry/revocation → audit review.

## 6. Open decisions that must remain open until owner approval

- First pilot ISP segment and first integration.
- Exact MVP feature boundary.
- Auth provider and subscriber account-linking method.
- Full role/permission matrix and support-access model.
- Exact trusted tenant-context/RLS mechanism.
- Source-of-truth and conflict rules for each domain in Hybrid mode.
- Financial ledger/accounting model and detailed payment states.
- SaaS pricing, plans, trials, limits and suspension rules.
- Bridge queueable action classes and expiry/ordering rules.
- SLO, RTO, RPO, backup retention and support commitments.
- API/event contract and versioning policy.
- Localization/accessibility acceptance criteria.
- Pakistan-specific legal, privacy, telecom, tax and payment obligations.

## 7. Exit criteria before implementation planning

The owner should not be asked to approve a broad build until:
- A first MVP/pilot segment is supported by customer evidence.
- The seven golden journeys have actor, permissions, states, error/recovery and acceptance criteria.
- Hybrid data ownership is explicit.
- Financial and service lifecycle state machines are defined.
- Authentication and permission model is specified.
- Tenant context/RLS design has a reviewed proof of concept and threat boundary.
- First integration capabilities and support limits are documented.
- Commercial and operational launch assumptions are visible.
- Goldfish scenarios have testable acceptance criteria.

## 8. Audit boundary

This review used the Markdown documentation on GitHub's `main` branch, including product vision, PRD, architecture, tenancy/data, security, payments, integrations/Bridge, roles, UX, operations, API/events, roadmap, testing, workflow, decisions, traceability, changelog, market positioning and stage playbooks. It did not inspect a running UI, application source code, database instance, payment gateway, Bridge binary or deployment environment. No implementation or production security test was performed.


## 9. Stage 3 — Improved solution package (GQA-PLAN-01)

The initial audit identified gaps. This improvement pass converts the broad recommendations into two reusable planning specifications:
- [Product Discovery and MVP Definition Framework](PRODUCT_DISCOVERY_AND_MVP.md): structured Pakistani ISP discovery, non-leading interview guide, evidence quality, weighted prioritization, hard safety gates and an owner-controlled MVP decision record.
- [Golden User Journeys and State Contracts](GOLDEN_JOURNEYS.md): seven end-to-end journeys with actors, proposed flows, state boundaries, failure/abuse cases and acceptance examples.

### Finding-to-improvement disposition

| Findings | Improved treatment | Remaining dependency |
|---|---|---|
| GQA-01 | Seven golden journeys now have common workflow contracts and scenario-level acceptance examples | Owner approval and detailed per-workflow state/permission decisions |
| GQA-02 | Journey and discovery frameworks require source-of-truth and action-authority mapping per domain | ISP-specific mapping; conflict and reconciliation rules |
| GQA-03 | Discovery guide, evidence record, scoring model, hard gates and MVP decision record added | Real ISP interviews and owner selection |
| GQA-04 | Payment journey separates intent, provider transaction, invoice allocation and service activation; immutable corrections proposed | Approved ledger/accounting model, complete edge-case rules |
| GQA-05 | Identity and account-linking are explicit preconditions across application and subscriber journeys | Auth provider, recovery, MFA/step-up and linking decisions |
| GQA-06 | Application, provisioning, tenant, subscription and service states are kept distinct | Full tenant/subscription transition and suspension policy |
| GQA-07 | Journeys require actor/action permissions; sensitive actions have explicit authorization gates | Complete resource/action permission catalogue and role matrix |
| GQA-08 | All journeys require trusted tenant context and cross-tenant negative tests | Detailed RLS/context proof-of-concept and independent security review |
| GQA-09 | Bridge journey defines enrollment, read-only validation, write authorization, expiry, replay and revocation expectations | Exact queueable action classes, ordering and protocol contract |
| GQA-10 | Discovery framework captures pricing evidence and requires an owner-approved commercial record | Actual willingness-to-pay evidence and plan/pricing decisions |
| GQA-11 | MVP/pilot decision record requires support owner, rollback and measurable operational assumptions | Approved SLO, RTO/RPO, support and capacity targets |
| GQA-12 | Journeys require explicit adapter capability and integration validation before enabling writes | Canonical API/event schemas and version lifecycle |
| GQA-13 | Discovery and launch planning identify legal/provider obligations as hard-gate research | Qualified Pakistan-specific legal, tax, telecom and payment advice |
| GQA-14 | This report is the canonical GQA-PLAN-01 audit; stage histories and central tracker link it | Keep future runs additive and avoid duplicate competing status records |
| GQA-15 | Journey framework requires mobile behavior, clear states and recovery | Language, accessibility and responsive design acceptance specification |
| GQA-16 | MVP framework defines candidate measurable pilot indicators and baseline-before-target rule | Actual baseline, target values and owner approval |
| GQA-17 | Audit boundary explicitly excludes code/runtime verification | Separate repository code/UI/security QA when implementation exists |

### Improved MVP recommendation

Do **not** choose Native, Integrated or Hybrid as the first pilot by assumption. The long-term product must support all three. The first pilot mode and first integration must be selected from customer evidence, available test access, delivery effort, supportability and risk. A read-only integration pilot may be a safer initial validation step where it demonstrates value without granting network write authority; it is a candidate, not a predetermined decision.

### Recommended sequencing

1. Conduct customer discovery and capture evidence before locking MVP scope.
2. Select one narrow pilot outcome and one supported integration with explicit capability limits.
3. Approve the golden journeys relevant to that pilot.
4. Resolve data ownership, financial state and permission decisions for those journeys.
5. Complete tenant security proof-of-concept and operational readiness gates.
6. Only then submit a bounded implementation proposal for Stage 4 approval.

### Stage 3 conclusion

Stage 3 improvement documentation is complete for this planning pass. It does not mean the findings are implemented, real ISP demand is validated, any integration is certified, or the owner has approved the proposed MVP. Stage 4 remains pending.
