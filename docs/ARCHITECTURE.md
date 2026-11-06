# Architecture

**Status:** Direction approved; detailed technology choices and implementation are not yet approved.

## Architectural shape

A modular, multi-tenant SaaS platform with:
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

Cloud provider, programming languages/frameworks, database technology, queue/event technology, deployment topology, API protocol details, tenancy model (shared schema/database vs isolated resources), encryption/key management and exact service boundaries require architecture review and owner approval.


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
