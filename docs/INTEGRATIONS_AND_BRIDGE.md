# Integrations and RMSK Bridge

## Approved strategy

The platform must be vendor-agnostic and integration-method-agnostic. ISPs must not be forced to replace a suitable existing backend.

**Do not promise universal compatibility at launch.** Publish only integrations that have been implemented and tested against documented versions and configurations.

## Adapter-first model

A common internal integration contract sits between core platform workflows and vendor/protocol adapters. Each adapter declares capabilities, supported versions, read/write operations, limitations, authentication method, rate limits and recovery behavior.

## Integration methods to evaluate

- MikroTik RouterOS API.
- RADIUS and supported AAA workflows.
- REST APIs.
- Webhooks and event callbacks.
- Local RMSK Bridge.
- Selected OLT/ONU integrations, based on vendor/model/API availability.
- SNMP for appropriate monitoring/read-only use cases.
- CSV/Excel import/export for controlled migration or fallback.
- Native RMSK management modules.
- Manual workflows where automation is not available.

These are candidate methods, not all confirmed launch integrations.

## RMSK Bridge

A lightweight local integration agent installed on an ISP-controlled server or supported machine.

### Required design properties
- Unique Bridge identity and tenant binding.
- Secure outbound connection where feasible; avoid requiring inbound exposure to the ISP LAN.
- Encrypted and authenticated communication.
- Least-privilege local credentials.
- Durable offline queue for approved operations/events.
- Retry with exponential backoff and jitter.
- Idempotency and replay protection.
- Versioned protocol and compatibility checks.
- Health heartbeat, diagnostics and operational logs.
- Credential rotation and remote revocation.
- Secure update mechanism and rollback strategy.
- Explicit approval and audit for privileged network actions.

## Offline behavior

Bridge must safely queue eligible work while disconnected and synchronize after recovery. Queue limits, expiry, ordering, conflict rules and unsafe-to-replay operations must be defined. Never replay a destructive or time-sensitive action blindly.

## Integration lifecycle

Discovery → compatibility assessment → secure credential setup → test connection → capability validation → controlled activation → monitoring → version/credential maintenance → revocation/offboarding.

## Integration acceptance

For every supported adapter, test authentication, permissions, data mapping, pagination/rate limits, timeout behavior, retries, duplicate delivery, partial failure, reconnect, auditability and tenant isolation.
