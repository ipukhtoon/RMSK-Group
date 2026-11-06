# Roles and Permissions

## Role model direction

Use tenant-scoped role-based access control with least privilege. Platform-level roles must be separate from ISP tenant roles.

### Platform roles
- Super administrator: platform-wide administration; privileged actions audited.
- Platform support: limited, time-bound support access; no implicit unrestricted tenant access.
- Platform finance/operations: narrowly scoped platform subscription and operational permissions.

### ISP tenant roles (provisional categories)
- ISP owner.
- ISP administrator/manager.
- Finance staff.
- Customer support staff.
- Network/technical operator.
- Field staff (if required).
- Read-only/reporting role.
- Subscriber/end customer.

Final role names, permission matrix and delegation rules require separate approval.

## Rules

- Deny by default.
- Enforce permissions server-side.
- Enforce tenant membership in addition to role.
- Separate financial permissions from network-control permissions.
- Require additional confirmation/authorization for high-impact actions.
- Audit role changes, access grants, exports, payment adjustments and service/network actions.
- Support prompt access revocation when staff leave.
- Avoid shared staff accounts.

## Open decisions

MFA policy, approval workflows, break-glass access, support impersonation policy and detailed permission matrix remain to be specified.
