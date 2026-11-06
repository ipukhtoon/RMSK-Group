# Product Requirements Document (PRD)

**Status:** Approved product direction; detailed feature specifications and implementation remain subject to the owner workflow.

## Product scope

A multi-tenant SaaS platform for Pakistani ISPs, combining customer self-service, optional native ISP management and integration with existing ISP systems.

## User/customer groups

- Platform owner / super administrator.
- ISP owner and authorized management.
- ISP operations and support staff.
- Finance and payment staff.
- Field/technical staff, where enabled.
- ISP subscribers/end customers.
- Integration/Bridge operators.

Detailed permissions must be least-privilege and configurable by role.

## Core capabilities

### A. Tenant onboarding and administration
- Create and configure an isolated ISP tenant.
- Tenant-specific branding, service settings, payment options and integration configuration.
- Platform-level subscription and tenant lifecycle management.
- Secure tenant provisioning, suspension and offboarding procedures.

### B. Subscriber self-service
- Authenticated subscriber access.
- Account and service overview.
- Bills, balances, payment history and supported payment actions.
- Service status and support/contact pathways.
- Notifications for relevant account events.
- Self-service actions must be checked against the ISP's authoritative system and policy.

### C. Native ISP management (optional)
- Subscriber/customer records.
- Plans/packages and service assignment.
- Billing and ledger workflows.
- Payment recording and reconciliation.
- Service lifecycle and operational actions.
- Staff roles, reports and audit history.
- Exact modules and workflows require separate feature approval.

### D. Integration platform
- Adapter-based connectivity to existing ISP backends.
- REST APIs, webhooks, RADIUS, MikroTik RouterOS API, local Bridge, supported OLT/ONU integrations, SNMP and structured CSV/Excel import/export where appropriate.
- Manual fallback for unsupported or temporarily unavailable integrations.
- Integration capability must be documented and tested per vendor/model/version/configuration.

### E. Payments
- Online payment gateways where commercially and technically supported.
- Direct ISP payment instructions.
- Cash payments recorded by authorized staff.
- Separate payment status from service activation status.
- Server-side verification, idempotency, callback protection and reconciliation.

### F. Operations
- Audit logs, monitoring, backups, recovery, alerts and support tooling.
- Integration health and Bridge diagnostics.
- Data export and tenant offboarding controls.

## Product constraints

- Pakistan is the initial market.
- Support multiple ISP integration patterns; do not require one upstream provider, router, authentication system or backend.
- No universal compatibility promise.
- Preserve the ISP's existing backend when integration is the chosen mode.
- Keep each tenant's data and operations isolated.
- Do not mix platform SaaS subscription billing with subscriber payments collected by an ISP.

## Non-functional requirements

- Security and privacy by design.
- Strong tenant isolation.
- Resilience to intermittent ISP-site connectivity.
- Idempotent and auditable financial and integration operations.
- Responsive, accessible user experiences.
- Observability and recoverability.
- Performance targets and service-level objectives to be defined and approved.

## Acceptance

A feature is not implementation-ready until its user flows, permissions, data ownership, failure modes, audit requirements, integration behavior, security implications and testable acceptance criteria are documented and approved.


### A1. ISP registration, approval and tenant provisioning
- Public ISP application is separate from tenant creation, approval and paid subscription activation.
- Collect business/ISP name, owner/applicant details, verified contact channels, country, city, operating districts, access technology, optional estimated subscriber scale, existing management system, preferred operating model and relevant business/licensing information.
- Verify applicant contact channels using an approved verification method.
- New applications enter **Pending** and are reviewed by authorized RMSK platform staff.
- Application states: Pending, Under Review, Additional Information Required, Approved, Rejected and Suspended.
- Approval/rejection and material review actions must be attributable to an authorized staff account and audited.
- Do not represent an applicant as a legally licensed ISP merely because RMSK approved its platform application. Verify relevant licensing claims where applicable; record evidence and review status.
- Only after approval may the platform provision a tenant with a unique tenant ID, initial ISP owner account and isolated tenant configuration.
- Tenant provisioning must be idempotent and recoverable; partial failures must not create duplicate tenants or orphaned owner accounts.
- After provisioning, the ISP owner completes an onboarding wizard covering business profile, branding, operating model, network configuration, integrations/Bridge where applicable, packages, payment configuration, staff invitations and go-live checks.
- Native setup must be optional for integrated-mode ISPs; do not force migration or duplicate data entry for an existing backend where avoidable.
- Keep three distinct lifecycle concepts: **application status**, **tenant lifecycle status**, and **RMSK SaaS subscription/billing status**. Approval does not automatically mean paid subscription activation or operational go-live.
- Exact required fields, evidence, verification methods, approval authority, subscription plans, pricing, grace periods and go-live criteria remain to be specified.
