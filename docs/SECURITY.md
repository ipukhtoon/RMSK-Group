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


## Stage 3 — Refined multi-tenant security proposal (pending owner approval)

**Proposal status:** These controls are recommended for the detailed design. They are not implemented and do not yet constitute an approved final security specification.

### Threat model and RLS boundary
- Treat client-controlled tenant identifiers as untrusted selectors.
- NestJS must authenticate the user, verify active membership in the selected tenant and enforce the required permission on every protected operation.
- PostgreSQL RLS is defence in depth against omitted tenant predicates and accidental query-scope errors.
- Do not overstate RLS based only on a custom session setting: a database role that can execute `set_config` may be able to change a custom tenant setting. This mechanism does not protect against a compromised backend process or arbitrary SQL execution under that role. The final design must document this residual risk or adopt a database-verifiable context assertion with a clearly defined trust/key boundary.
- Tenant context must be transaction-local, fail closed and be used on the same connection as all scoped queries.

### Database privilege separation
- Separate runtime API, runtime worker, migration, backup/restore and controlled operations credentials.
- Runtime credentials must not own tenant tables, have superuser privileges, `BYPASSRLS`, role creation or unnecessary DDL privileges.
- Restrict schema/table/function grants to the minimum needed.
- Enable RLS for every tenant-owned table; assess `FORCE ROW LEVEL SECURITY` and inventory all privileged bypass paths.
- Review policy composition: permissive policies combine with OR and can broaden access. Use a restrictive tenant boundary and narrowly scoped action policies; review every policy change as security-sensitive.
- Test SELECT, INSERT, UPDATE, DELETE and attempts to change a row's tenant owner. Missing context must deny access.

### Application, transaction and async boundaries
- Centralize tenant-scoped Prisma access in a transaction wrapper; prohibit accidental root-client queries for tenant-owned data.
- Use parameterized queries and prohibit unsafe dynamic SQL built from user input.
- Verify selected Prisma and Supabase pooler behavior, transaction-local context, prepared statements, timeout and connection reuse before production.
- Workers must validate tenant/entity binding when consuming a job; jobs must be idempotent and safe to retry.
- Payment callbacks must verify signature/provider configuration, tenant-to-merchant mapping, transaction reference, amount, currency, status and replay/idempotency state.
- Bridge installation identity must be tenant-bound, least-privileged, revocable, authenticated and replay-protected. Privileged network actions require capability and authorization checks.
- Apply tenant authorization to cache, search, files, notifications, WebSockets, reports and exports, not only SQL queries.

### Platform support and privileged access
- Separate platform operational permissions from tenant business-data access.
- Support access is tenant-specific, purpose/reason recorded, time-limited and read-only by default.
- Sensitive support writes require explicit elevated authorization and audit.
- Break-glass access requires strong authentication, short-lived scoped grants, a recorded incident/ticket and mandatory post-use review.
- Privileged actions and access to sensitive tenant data must be tamper-resistant in audit records. Avoid storing secrets or unnecessary personal data in logs.

### Security verification gates
Before production release, require automated and adversarial tests for cross-tenant reads/writes/deletes, forged/missing context, policy-composition regressions, runtime-role bypass attempts, composite-FK violations, tenant switching, membership revocation, concurrent pooled connections, transaction rollback/timeouts, async job tenant mismatch, payment callback replay, Bridge credential revocation, file/export authorization and backup restoration. Add independent security review/penetration testing before production. A passing unit test suite alone is not a security certification.
