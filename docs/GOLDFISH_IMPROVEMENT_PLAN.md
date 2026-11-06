# Goldfish QA — Improved Product Planning and Closure Plan

**Run ID:** GQA-PLAN-02  
**Date:** 2026-11-06  
**Stage:** Stage 3 — Improvement  
**Source audit:** `docs/GOLDFISH_AUDIT.md` (GQA-PLAN-01)  
**Status:** Improved proposal; pending explicit owner approval.  
**Scope:** Improve the documentation/planning response to the Goldfish findings. This is not approval to implement features, select a pilot, choose vendors, set pricing or change the approved technology stack.

## 1. Improvement objective

Convert the initial audit's broad recommendations into an ordered, decision-oriented plan with:
- explicit deliverables;
- decisions that must be made by the owner versus facts that must be researched;
- dependency order;
- testable acceptance criteria;
- clear separation of Native, Integrated and Hybrid behavior;
- safety constraints for finance, tenant security and network actions;
- evidence requirements and exit gates.

Preserve the approved product principles and backend technology direction. Do not silently decide open commercial, legal, security or product questions.

## 2. Improved prioritization

The first audit classified eight findings as High. This improvement adds dependency ordering: not every High finding should be solved in parallel, and market discovery must inform MVP scope before detailed implementation specifications are frozen.

| Order | Work package | Addresses | Why this order | Exit evidence |
|---|---|---|---|---|
| 0 | Documentation baseline and decision register | GQA-14, GQA-17 | Make the planning source of truth reliable; distinguish evidence from assumptions | Consistent status/decision records; repository inventory |
| 1 | Customer discovery and pilot selection | GQA-03, GQA-16 | Avoid designing an oversized product or arbitrary first integration | Interview evidence, scored options, owner-approved pilot brief |
| 2 | Product journeys and service lifecycle | GQA-01, GQA-06, GQA-15 | Define real user jobs and operational states before detailed schema/UI | Journey specs, state tables, acceptance criteria |
| 3 | Hybrid ownership and integration contracts | GQA-02, GQA-09, GQA-12 | Resolve authority and synchronization before enabling cross-system writes | Ownership matrix, canonical contracts, conflict/retry rules |
| 4 | Identity, roles and tenant security | GQA-05, GQA-07, GQA-08 | Protect every workflow and data boundary | Auth decision, permission matrix, RLS proof-of-concept and threat review |
| 5 | Financial lifecycle and accounting model | GQA-04 | Financial implementation depends on identity, ownership and payment-provider responsibilities | Financial domain/state model, reconciliation examples, accounting review |
| 6 | Commercial, operations and legal readiness | GQA-10, GQA-11, GQA-13 | Needed before paid/public launch; some assumptions can be researched earlier | Commercial policy, operational objectives, qualified legal/accounting review |

**Important:** Package 5 (finance) is a high-risk domain and must be designed before payment implementation. Its detailed specification should coordinate with Packages 2–4 because invoice ownership, payer identity, provider settlement and service activation cross those boundaries.

## 3. Work package specifications

### WP0 — Documentation and decision hygiene
**Problem:** Planning can become contradictory if proposals, approved decisions, implementation and verification are mixed.

**Solution:**
- Treat `docs/DECISIONS.md` as the canonical record of owner decisions.
- Treat `docs/REQUIREMENTS_TRACEABILITY.md` as the requirement/status register.
- Treat `docs/WORKFLOW.md` as the current workstream/stage tracker.
- Treat each audit or detailed specification as supporting evidence, not a second decision log.
- Every open question gets an ID, owner decision needed, evidence needed, affected docs and blocking stage.
- Use the states Proposed, Approved, Implemented, Verified, Deferred and Rejected consistently.
- Do not create an approved decision merely because the owner approved doing planning work.

**Acceptance criteria:**
1. Each recommendation is traceable to one or more finding IDs.
2. Each decision is recorded once in the decision log and referenced elsewhere.
3. Stage run history identifies scope, output, limitations and commit evidence.
4. No planning-only work is labelled implemented or verified.

### WP1 — Customer discovery and evidence-backed MVP
**Problem:** The first customer segment, operational pain and first integration are not validated.

**Solution:**
Conduct structured discovery with a deliberately varied sample of small/medium Pakistani fibre and wireless ISPs. Do not assume a particular city, vendor, billing system or network technology is representative.

For each interview, capture:
- approximate subscriber band (optional and anonymized);
- fibre, wireless or mixed access;
- operating regions and support model;
- existing billing/CRM, RADIUS/AAA, router, OLT/ONU and payment processes;
- current subscriber onboarding, billing, payment confirmation and suspension workflow;
- recurring manual work, errors, delays and financial leakage;
- existing system contracts/API access/export options and version;
- willingness to pilot, decision-maker and budget signal;
- data migration constraints and what must remain in the current backend.

Score candidate pilot opportunities using an agreed rubric: pain severity/frequency, number of ISPs reporting it, willingness to pilot/pay, integration feasibility, data access, implementation effort, operational support burden, security risk and ability to demonstrate value in a short pilot. Keep evidence and scores visible; do not let a high market-frequency score hide an impossible integration.

Compare at least three pilot shapes before selecting:
- Native-first ISP with a limited native subscriber/billing workflow.
- Integrated-first ISP retaining its existing backend, with read-only visibility and one controlled action.
- Hybrid ISP with explicitly split ownership across selected domains.

These are discovery options, not preselected product modes.

**Deliverables:** interview guide, anonymized evidence register, current-state workflow maps, integration inventory, scored opportunity matrix, pilot brief and success metrics.

**Acceptance criteria:**
1. Evidence comes from actual ISP conversations or clearly labelled secondary research.
2. Unknowns and conflicting statements remain visible.
3. The chosen pilot has a named user persona, problem, operating mode, system boundary, first integration, in-scope/out-of-scope capabilities and support assumptions.
4. The pilot can be demonstrated without a universal compatibility claim.
5. Owner explicitly approves the pilot scope before implementation.

### WP2 — Golden journeys and state models
**Problem:** Feature lists do not specify complete, recoverable work.

**Solution:** Create one specification per journey using the common template in Section 4. Start with these seven:
1. ISP application → review → approval → tenant provisioning → owner activation.
2. Owner onboarding → mode selection → data ownership mapping → integration validation → readiness/go-live.
3. Subscriber identity verification/recovery → account view → invoice → payment → receipt/status → service status.
4. Cashier/finance task → receipt/allocation → reconciliation → authorized correction/refund.
5. Support/network task → subscriber lookup → diagnostics → approval where needed → action → audit/customer communication.
6. Bridge enrollment → tenant binding → capability test → command lifecycle/offline recovery → revoke.
7. Platform support case → scoped access grant → diagnostics → expiry/revocation → audit review.

For each journey, define:
- actor, goal, preconditions and permissions;
- entry point and required information;
- numbered happy-path steps;
- explicit states and legal transitions;
- validation, duplicate submission and idempotency behavior;
- timeout, offline, retry, cancel and recovery behavior;
- notifications and user-visible explanations;
- audit events and sensitive-data boundaries;
- source-of-truth for every changed record;
- acceptance criteria, negative scenarios and Goldfish personas.

**Acceptance criteria:** Every transition has an authorized actor, precondition, side effect, resulting state, audit event and failure/recovery path. No payment success message is inferred from a browser redirect alone. No service activation is inferred solely from payment confirmation.

### WP3 — Hybrid data ownership and integration contracts
**Problem:** “Integrated” and “Hybrid” are not safe unless authority is explicit.

**Solution:** Define authority at the level of domain/entity/action, not only a single global system setting. For each tenant and integration, record:
- domain/entity (subscriber identity, contact, package catalogue, service assignment, invoice, payment, network state, support ticket);
- authoritative system for creation and updates;
- whether RMSK stores a full record, mapped projection, reference or event only;
- permitted read/write actions;
- sync direction and trigger;
- stable external identifiers and mapping lifecycle;
- conflict detection and resolution owner;
- deletion/disable semantics (tombstone vs hard delete);
- freshness/last-sync indicator;
- retry, idempotency, ordering and reconciliation behavior;
- behavior when the external system is unavailable or mapping is lost.

**Recommended guardrails:**
- One authoritative writer per field/action at a time unless an explicit conflict-resolution rule exists.
- Do not use last-write-wins for financial, identity or service-control state.
- Treat external system IDs as scoped to a specific tenant and connector.
- Never let an integration response silently overwrite an authoritative value from another system.
- Separate command acceptance from command execution and final state confirmation.
- Support read-only mode as the safest initial integration validation step.
- Make write capabilities opt-in, capability-tested and auditable.

**Acceptance criteria:** For every pilot data type and action, the matrix names authority, direction, failure behavior and reconciliation owner. Tests cover duplicate, delayed, out-of-order, conflicting and missing events.

### WP4 — Identity, permissions and tenant security
**Problem:** Authentication, subscriber account linking, role actions and exact database tenant-context trust are open.

**Solution:** Specify these as separate but coordinated layers:
1. Authentication: how a person proves identity.
2. Account linking: how an authenticated person is linked to an ISP staff membership or subscriber account.
3. Authorization: which action they may perform on which tenant resource.
4. Database isolation: how tenant scope is enforced below the application.
5. Privileged support: how platform personnel receive exceptional scoped access.

Create an action/resource permission catalogue before assigning role bundles. Include explicit deny cases for cross-tenant access, staff self-escalation, subscriber access to another account, cashier refunds without permission and network actions without capability/approval.

For RLS, do not assume that transaction-local `set_config` alone makes tenant context unforgeable. It prevents session-state leakage when used correctly, but a compromised application role able to issue arbitrary SQL may set a custom variable itself. Compare context assertion approaches and document the threat boundary, including what happens if the NestJS process or runtime DB credential is compromised. Verify runtime role ownership, grants, `BYPASSRLS`, table owner behavior, policy composition and connection pooling. Select the final mechanism only after a proof-of-concept and security review.

**Acceptance criteria:**
- Every protected route/service has server-side permission checks.
- Tenant membership is checked independently for each active tenant context.
- Missing/invalid context fails closed.
- Runtime roles cannot own tables, run migrations or bypass RLS.
- Adversarial tests cover forged tenant context, cross-tenant foreign keys, concurrent pooled transactions, background jobs and privileged support.
- Subscriber account linking and recovery do not rely on guessable subscriber IDs alone.

### WP5 — Financial lifecycle and reconciliation
**Problem:** Payment events, accounting records and service state are distinct but not fully modelled.

**Solution:** Define separate concepts and state machines for:
- invoice and invoice lines;
- payment intent/request;
- provider transaction/notification;
- received funds/settlement evidence;
- payment allocation to one or more invoices;
- receipt;
- refund/reversal/chargeback;
- manual cash receipt;
- credit/adjustment;
- service activation request and final service state.

Decide with qualified accounting input whether the product requires a balanced double-entry ledger or an auditable subledger for the initial product. Do not describe a simple payment table as a complete accounting ledger. Never edit or delete posted financial history to correct it; use traceable reversal/adjustment records. Define currency precision, rounding, fee/tax responsibility, partial/overpayment handling and reconciliation exceptions.

**Source-of-truth principle:** A gateway/provider is authoritative for its transaction/settlement facts; RMSK maintains its own immutable record of verified payment events and allocations; the ISP backend remains authoritative for service state where that is the configured ownership. These roles must be reconciled, not conflated.

**Acceptance criteria:** Demonstrate duplicate callback, delayed callback, out-of-order event, partial payment, overpayment, failed payment, refund, reversal, chargeback, settlement mismatch and service activation failure without corrupting balances or misrepresenting status.

### WP6 — Commercial, operations and legal readiness
**Solution:**
- Define SaaS plan entitlements separately from usage metering and subscriber collections.
- Define trial, paid, grace, overdue, suspended, cancellation and reactivation transitions.
- Specify exactly which tenant capabilities remain available during billing delinquency; preserve safe export and support access according to approved policy.
- Define measurable SLOs only after workload and hosting assumptions exist.
- Define backup frequency, retention, encryption, restore tests, RPO/RTO and tenant-level export/offboarding.
- Define support hours, severity levels, incident communications and escalation.
- Obtain qualified Pakistan-specific advice on privacy/data protection, telecom/licensing representations, contracts, tax/accounting and payment-provider obligations before public launch. This plan is not legal advice.

**Acceptance criteria:** A tenant lifecycle matrix, commercial policy, operational runbooks, recovery drill evidence and legal/compliance review are approved before paid public launch.

## 4. Standard golden-journey specification template

Every journey document must include:

| Section | Required content |
|---|---|
| Purpose | User problem and measurable outcome |
| Persona | Actor, knowledge, device/connectivity assumptions |
| Authorization | Tenant, role, permissions and prohibited actions |
| Preconditions | Account, configuration, integration and data state |
| Main flow | Numbered user/system steps |
| State model | States, transition triggers, guards and terminal states |
| Data authority | System of record per entity/field/action |
| Side effects | DB changes, external commands, payment effects, notifications |
| Failure paths | Validation, timeout, duplicate, offline, external outage, partial failure |
| Recovery | Retry, cancel, reconcile, manual support and safe rollback |
| Audit/privacy | Events, retention, redaction and visibility |
| Acceptance | Positive, negative, security and accessibility scenarios |
| Open decisions | Questions requiring evidence or owner approval |

Do not begin detailed UI design from a feature title alone. First approve the journey and its states; then design screens and interactions.

## 5. Improved MVP selection rule

Do not choose the MVP by counting features or by assuming Native, Integrated or Hybrid is inherently best.

Use a weighted scorecard after discovery. Proposed dimensions (weights remain subject to owner approval):
- Customer pain severity/frequency — 25%
- Evidence of repeated demand across target ISPs — 20%
- Willingness and readiness to pilot/pay — 15%
- Integration feasibility and data access — 15%
- Time-to-demonstrable-value — 10%
- Support/operational burden — 10%
- Security/compliance risk — 5%

Score each candidate from 1–5 and document evidence/confidence. A candidate with a hard security, legal or data-access blocker cannot win by score alone. Do not treat these suggested weights as approved commercial strategy.

## 6. Release and implementation gates

### Gate A — Discovery complete
- Interview evidence and current-state workflow maps exist.
- Pilot candidate and first integration are compared.
- Owner approves target pilot and explicit MVP boundary.

### Gate B — Product behavior defined
- Relevant golden journeys are approved.
- State transitions, permissions, failure/recovery and acceptance criteria are complete.
- Native/Integrated/Hybrid differences are explicit.

### Gate C — Data and security design ready
- Data ownership/action authority matrix is complete for pilot scope.
- Auth/account-linking and role permissions are defined.
- Tenant-context/RLS proof-of-concept and threat boundary reviewed.
- Finance model and integration command/event semantics are defined for the pilot.

### Gate D — Operational/commercial pilot ready
- Support, backup/restore, monitoring, incident and rollback plans exist.
- Pilot commercial terms and legal/compliance review are complete.
- Pilot success metrics and stop/rollback criteria are agreed.

Only after the owner approves the scoped implementation proposal should Stage 5 begin. Passing a planning gate is not proof that implementation is secure or production-ready.

## 7. Proposed pilot success measures

Set numeric targets only after customer discovery and baseline measurement. At minimum measure:
- time from application approval to configured pilot;
- percentage of onboarding steps completed without staff intervention;
- subscriber/account mapping accuracy;
- integration sync success, lag and reconciliation exception rate;
- payment verification and allocation accuracy;
- duplicate/ambiguous transaction rate;
- time to detect and recover from Bridge/integration outage;
- support tickets per ISP and time to resolution;
- completion rate for key subscriber/staff tasks;
- pilot user adoption and willingness to continue/pay.

Every metric needs a definition, data source, measurement window, owner and target. Avoid invented targets before baseline data exists.

## 8. Finding disposition matrix

| Finding | Improved disposition | Status after this run |
|---|---|---|
| GQA-01 Golden journeys | WP2 standard template and seven journey specs | Refined proposal; specs still to be authored |
| GQA-02 Hybrid ownership | WP3 entity/action-level authority matrix and conflict rules | Refined proposal; matrix still to be authored |
| GQA-03 MVP/integration selection | WP1 evidence-based discovery and pilot scorecard | Refined proposal; market evidence absent |
| GQA-04 Finance | WP5 distinct financial concepts and accounting decision | Refined proposal; accounting model open |
| GQA-05 Identity/auth | WP4 separate authentication, account linking and authorization | Refined proposal; provider/design open |
| GQA-06 Lifecycle | WP2 state machines plus WP6 SaaS lifecycle policy | Refined proposal; transitions still to be authored |
| GQA-07 Permissions | WP4 action/resource catalogue and deny cases | Refined proposal; matrix still to be authored |
| GQA-08 RLS/context | WP4 threat-boundary analysis and proof-of-concept gate | Refined proposal; no mechanism selected |
| GQA-09 Bridge queue | WP3 command lifecycle and WP2 Bridge journey | Refined proposal; command classes open |
| GQA-10 SaaS commercial | WP6 entitlement and delinquency policy | Refined proposal; pricing open |
| GQA-11 Operations | WP6 SLO, recovery and support readiness | Refined proposal; targets open |
| GQA-12 API/events | WP3 canonical contract, versioning and reconciliation | Refined proposal; contracts open |
| GQA-13 Compliance | WP6 qualified Pakistan-specific review | Refined proposal; legal advice not obtained |
| GQA-14 Documentation hygiene | WP0 canonical records and status discipline | Process recommendation; consistency review still needed |
| GQA-15 UX/accessibility | WP2 persona journeys plus acceptance criteria | Refined proposal; standards/language choices open |
| GQA-16 Metrics | WP1 baseline and Section 7 pilot metrics | Refined proposal; numeric targets await evidence |
| GQA-17 Runtime verification | Explicit boundary and later Stage 5 runtime QA | Not closable until implementation exists |

## 9. Decisions deliberately not made

This improvement does **not** choose:
- pilot ISP, city, subscriber segment, operating mode or first vendor integration;
- MVP feature list;
- authentication provider or account-linking mechanism;
- final role matrix or support-access approval rules;
- final RLS trusted-context mechanism;
- per-domain hybrid authority configuration;
- double-entry ledger versus auditable subledger;
- payment gateway, pricing, plans or tax treatment;
- Bridge command queue classes and expiry values;
- numeric SLO/RTO/RPO or support hours;
- final API/event versioning policy;
- legal conclusions or regulatory status.

These require evidence, specialist review or explicit owner decisions.

## 10. Stage 3 completion statement

The initial audit has been converted into a structured closure plan with ordered work packages, deliverables, acceptance criteria, decision boundaries and implementation gates.

**Completed:** planning improvement only.  
**Pending:** owner approval of this planning direction and selection of the next work package.  
**Not performed:** ISP interviews, vendor selection, legal review, runtime tests, code changes, database changes, infrastructure changes or production security testing.
