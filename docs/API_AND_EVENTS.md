# API and Events

## Direction

Use explicit, versioned API contracts for first-party clients, tenant integrations and Bridge communication. Exact protocols and schemas remain open until architecture approval.

## API requirements

- Authenticate every request.
- Derive and validate tenant context server-side.
- Authorize actions by tenant, role and resource.
- Validate payloads and enforce size/rate limits.
- Use consistent error codes and correlation IDs.
- Document pagination, filtering, sorting and versioning.
- Make retriable write operations idempotent.
- Avoid exposing internal identifiers or secrets unnecessarily.
- Audit sensitive and financial operations.

## Event requirements

Events should have stable IDs, tenant scope, event type/version, occurrence time, correlation/causation IDs and a validated payload. Delivery may be at least once; consumers must be idempotent. Define ordering guarantees only where they can be enforced.

## Candidate event families

Subscriber/account changes, invoice and payment lifecycle, service state, integration health, Bridge connectivity, tenant lifecycle and audit/security events.

## Webhooks

Use signature verification, timestamps/replay protection, bounded retries, delivery logs, secret rotation and explicit endpoint verification. Never trust a webhook solely because it reached the endpoint.

## Open decisions

REST/GraphQL/gRPC choices, event broker, schema registry, webhook signing format, version policy and public API access model require architecture approval.
