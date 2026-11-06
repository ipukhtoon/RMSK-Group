# Golden User Journeys and State Contracts

**Status:** Proposed product specification framework — pending owner approval.  
**Purpose:** Convert product capabilities into complete actor journeys, permissions, states, failure handling and testable acceptance criteria.  
**Scope note:** These are target design contracts, not implemented behavior or runtime test results.

## 1. Common journey contract

Every workflow must specify:
1. Actor/persona and tenant context.
2. Preconditions and required permissions.
3. Source of truth and authority for each affected data domain.
4. User-visible states and allowed transitions.
5. Normal path and alternate paths.
6. Validation, idempotency and concurrency behavior.
7. Audit events and notification behavior.
8. Failure, retry, timeout, cancellation and recovery.
9. Data privacy and cross-tenant protections.
10. Acceptance scenarios, including negative tests.

No client-supplied tenant ID, role, payment status or integration identity is trusted without server-side verification.

## 2. Journey A — ISP application to owner activation

**Actor:** Prospective ISP applicant; RMSK application reviewer; provisioning service; initial ISP owner.

### Proposed flow
1. Applicant reviews product/mode descriptions and compatibility limitations.
2. Applicant starts an application; system validates required fields and explains optional fields.
3. Applicant verifies contact channel(s); unverified applications cannot be treated as verified.
4. System issues an application reference and persists draft/submission state safely.
5. Reviewer sees only applications permitted by platform role.
6. Reviewer requests additional information, approves or rejects with reason and audit attribution.
7. Approval triggers an idempotent provisioning workflow: permanent tenant UUID, default tenant configuration and owner invitation/account binding.
8. Provisioning steps are checkpointed. Retry resumes safely; unique constraints prevent duplicate tenant/membership/invitation.
9. Owner accepts a short-lived, single-use invitation and establishes authentication.
10. Owner enters mode-aware onboarding.

### State boundaries
- Application: Draft → Pending → Under Review → Additional Information Required → Under Review → Approved or Rejected. Suspension/reopening rules require policy.
- Provisioning: Not Started → In Progress → Completed, or Failed/Retryable with recorded step and safe resume.
- Tenant lifecycle and SaaS subscription state are separate and must not be inferred from application approval.
- Owner invitation: Issued → Accepted, Expired or Revoked.

### Failure and abuse cases
Duplicate submission, duplicate approval event, reviewer without permission, expired invitation, changed applicant contact, provisioning partial failure, owner already belonging to another tenant, forged tenant ID and unauthorized evidence access.

### Acceptance examples
- Replaying the same approval/provisioning command does not create duplicate tenant or owner membership.
- A rejected/pending application cannot provision a tenant.
- Expired/revoked invitation cannot be used.
- Application approval does not automatically mark subscription paid or tenant live.
- Applicant sees clear status and next action without seeing internal reviewer notes.

## 3. Journey B — Owner onboarding and mode-aware go-live

**Actor:** ISP owner/admin and authorized technical operator.

### Proposed flow
1. Confirm business profile, locale/timezone and contact details.
2. Select Native, Integrated or Hybrid mode and see only relevant setup steps.
3. Declare systems in use and map data domains to authoritative sources.
4. Configure integration using least-privilege credentials; secrets are stored securely and never re-displayed in plaintext.
5. Run connection validation and read-only capability checks first.
6. Review field mapping, sync direction, conflict policy and action authority.
7. Enable write capabilities individually only after explicit ISP authorization and successful test.
8. Configure packages/payment settings only for domains RMSK is authorized to own.
9. Invite staff with least privilege.
10. Complete readiness checks, backup/rollback plan and go-live approval.

### Mode constraints
- Native: RMSK owns only the domains explicitly configured as native.
- Integrated: existing backend remains authoritative for mapped domains; no forced duplicate subscriber/package/financial setup.
- Hybrid: each domain and action has explicit owner, direction, conflict handling and reconciliation process. No implicit dual-write.

### Acceptance examples
- Integrated onboarding can complete without creating duplicate subscriber records in RMSK where not required.
- Write capability remains disabled until authorization, mapping and test pass.
- Failed integration validation shows actionable error and does not mark setup complete.
- Owner can resume onboarding after logout/network loss.
- Go-live cannot pass while required security or integration checks are failed.

## 4. Journey C — Subscriber access, invoice and payment

**Actor:** Subscriber; payment provider; ISP backend/RMSK payment service.

### Proposed flow
1. Subscriber accesses portal using a verified account-linking method.
2. System reveals only accounts/services the subscriber is authorized to access.
3. Subscriber views current package, service state, invoices and payment history.
4. Subscriber selects an eligible invoice/payment option; server calculates payable amount.
5. Server creates a unique payment intent/idempotency key and redirects or presents provider instructions.
6. Provider callback is authenticated, replay-protected and reconciled with server-side amount, currency, merchant and reference.
7. Subscriber sees pending/confirmed/failed/refunded status with timestamp and next steps.
8. Payment allocation and service activation are separate workflows; activation follows the authoritative ISP service-control policy.
9. Receipt and support route are available.

### Separate state machines
- Payment intent/provider transaction: Created → Pending → Succeeded or Failed; later refund/reversal states are separate events, not destructive edits.
- Invoice: Draft → Issued → Partially Paid / Paid / Overdue / Void according to approved rules.
- Service: Pending Activation / Active / Suspended / Change Pending / Termination Pending / Terminated according to ISP authority.
These are candidate states; final transitions and edge rules require finance/domain approval.

### Failure and abuse cases
Duplicate tap, late/out-of-order callback, forged callback, amount/currency mismatch, provider timeout, user closes browser, payment succeeds but service update fails, refund, chargeback and account enumeration.

### Acceptance examples
- Repeated callback produces one financial effect.
- Payment success never silently implies service activation.
- Client cannot choose a lower amount or another tenant's invoice.
- Pending status is not falsely displayed as failed or paid.
- Reconciliation can resolve a provider-success/local-timeout case without duplicate charge.

## 5. Journey D — Cashier and finance operations

**Actor:** Cashier, accountant, finance manager.

### Proposed flow
1. Staff authenticates and selects an authorized tenant.
2. Searches a subscriber within tenant scope and verifies identity/account.
3. Records cash/bank/manual payment with amount, currency, reference, timestamp and operator.
4. System validates invoice balance and allocation rules.
5. Receipt is issued and financial event is immutable/audited.
6. Exceptions (unallocated, duplicate reference, overpayment, reversal/refund) enter a controlled queue.
7. Authorized reviewer approves sensitive correction/refund where policy requires.
8. Daily reconciliation compares recorded receipts with cash/bank/provider settlement evidence.

### Acceptance examples
- Cashier cannot refund or edit posted financial history unless separately authorized.
- Corrections create linked reversal/adjustment records; original event remains.
- Duplicate references are detected according to configurable provider/business rules.
- Every financial change records actor, time, reason and linked source record.
- Tenant staff cannot access another tenant's ledger.

## 6. Journey E — Support and network operations

**Actor:** Support agent, network operator, ISP administrator, subscriber.

### Proposed flow
1. Staff searches a subscriber using tenant-scoped identifiers and confirms correct account.
2. Staff views only permitted diagnostics and service history.
3. Support notes and customer communication are recorded with access controls.
4. Network operator requests a permitted service action; system checks role, service authority, current state and integration capability.
5. High-impact actions require explicit confirmation/reason and, where policy says, secondary approval.
6. Adapter/Bridge executes with command ID, expiry, idempotency and result status.
7. System records outcome and reconciles actual network/service state before showing completion.

### Acceptance examples
- A support agent cannot perform network write actions without permission.
- A stale/expired command is not executed after reconnection.
- An uncertain timeout is reconciled before a potentially destructive retry.
- A failed command is visible as failed/unknown, not falsely successful.
- User can see who initiated a sensitive service action.

## 7. Journey F — Bridge installation, operation and recovery

**Actor:** ISP technical administrator, Bridge agent, RMSK control plane.

### Proposed flow
1. Owner initiates Bridge installation from an authenticated tenant.
2. Platform issues a short-lived, single-use enrollment token bound to tenant and intended installation.
3. Bridge establishes authenticated outbound connection where feasible and receives scoped identity.
4. Operator selects adapter and supplies secrets locally/securely.
5. Bridge performs read-only health/capability checks.
6. Owner authorizes specific write capabilities and action classes.
7. Bridge reports health, version, last contact and queue status.
8. During outage, only explicitly approved queueable operations are persisted; online-only/time-sensitive actions are rejected or expire.
9. On reconnect, Bridge authenticates, validates command age/authorization and reconciles before executing eligible work.
10. Owner/platform can revoke installation; revocation prevents future commands and credentials are rotated/invalidated.

### Acceptance examples
- Enrollment token cannot be reused or bound to another tenant.
- Bridge identity cannot be moved to another tenant by changing a client field.
- Revoked Bridge cannot reconnect or execute new commands.
- Expired service-control command is discarded and surfaced for operator review.
- Queue replay is idempotent and auditable.

## 8. Journey G — Scoped platform support

**Actor:** RMSK support staff, approver, ISP owner/admin, audit reviewer.

### Proposed flow
1. Support case is created with tenant, issue category and purpose.
2. Staff requests minimum required access scope and duration.
3. Approval follows defined separation-of-duties policy.
4. Grant expires automatically and can be revoked immediately.
5. Support sees only approved fields/actions; sensitive access is separately gated.
6. Tenant owner is notified according to policy; all reads/writes are audited.
7. Access ends; post-use review is triggered for elevated/break-glass sessions.

### Acceptance examples
- Platform staff do not gain tenant data access merely from platform role.
- Expired/revoked grant blocks subsequent access.
- Break-glass requires reason, strong authentication, short duration and post-event review.
- Audit trail identifies actor, tenant, purpose, scope, time and actions.

## 9. Cross-journey acceptance gates

Before a journey can be approved for implementation:
- Actor and permissions are explicit.
- State transitions and invalid transitions are defined.
- Source of truth and action authority are explicit.
- Tenant isolation is enforced at every data boundary.
- Idempotency and concurrency are addressed for repeatable actions.
- Failure, timeout, retry and recovery are defined.
- Audit and notification events are defined.
- Mobile/responsive behavior is specified where applicable.
- No success state is shown before authoritative confirmation.
- Positive, negative and abuse tests are listed.

## 10. Decisions intentionally left open

Exact authentication provider/account linking, detailed permission matrix, accounting model, payment provider contracts, service-state policy, support grant approval policy, Bridge queue classes, retention, language priority and detailed state transition rules require separate owner-approved specifications. This framework does not silently decide them.
