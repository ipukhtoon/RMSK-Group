# Security Requirements

**Security is a release requirement, not a later enhancement.**

## Identity and access
- Strong authentication for platform and ISP staff.
- Role-based, least-privilege access.
- Tenant membership checked server-side on every protected operation.
- Secure session lifecycle, revocation and recovery.
- Strong protection for privileged/super-admin accounts.
- Staff offboarding and credential rotation.

## Tenant security
- Tenant isolation at API, database, cache, storage, job and event layers.
- No client-controlled tenant authorization.
- Automated negative tests for cross-tenant reads and writes.
- Audit sensitive administrative and financial actions.

## Integration security
- Unique identity for each Bridge installation.
- Outbound-only connection preferred where feasible.
- Encrypted communication and mutual authentication.
- Least-privilege credentials and narrowly scoped permissions.
- Credential rotation, revocation and device/Bridge disablement.
- Signed/verified messages where applicable.
- Replay protection, request validation, rate limiting and audit logs.
- Secure update and version policy for Bridge.

## Payment security
- Verify payment status server-side with the gateway/provider.
- Validate callback signatures and transaction references.
- Idempotent payment processing.
- Protect against replay, forged callbacks and duplicate credits.
- Never trust client-side success screens as proof of payment.
- Separate money receipt from service activation.
- Reconcile gateway, ledger and ISP backend records.

## Application security
- Input validation and output encoding.
- Secure secrets management.
- Dependency and vulnerability management.
- Secure configuration and environment separation.
- Rate limiting and abuse detection.
- Safe error messages; no secrets in logs.
- Backup encryption and restore testing.
- Security review and penetration testing before production.

## Compliance

Obtain Pakistani legal, privacy, telecom and payment compliance advice appropriate to the actual services, contracts and data processed. This document is not legal advice and does not assert compliance certification.


## Multi-tenant isolation controls
- Initial model: shared PostgreSQL database and shared tenant-owned tables, with a unique permanent UUID tenant identifier per ISP.
- Enforce tenant authorization in NestJS using authenticated identity, explicit tenant membership and per-tenant permissions. Never trust client-provided tenant IDs.
- Enforce database isolation with PostgreSQL RLS that fails closed when trusted tenant context is missing, invalid or revoked.
- Use tenant-aware composite constraints and foreign keys to prohibit cross-tenant relationships.
- Keep user identity separate from tenant membership. Authorize each membership independently; isolate platform-level identities/data and audit privileged access.
- Scope cache, jobs, events, WebSockets, payment callbacks, Bridge identities, notifications, files, reports and exports to the verified tenant.
- Prevent tenant context leakage across pooled database connections and concurrent requests; use a safe transaction-scoped context and test Prisma/pool behavior.
- Require automated negative tests for cross-tenant reads, writes, relationship manipulation, exports and asynchronous access before production.
- Detailed schema, RLS policies, context propagation and test implementation remain pending design/review; documentation does not imply implementation.
