# Changelog

All notable approved project decisions and documentation changes are recorded here. Dates use ISO format.

## 2026-11-06 — Initial project documentation baseline
- Established **RMSK Group** as the approved working company identity.
- Recorded Pakistan-first market focus and initial target segment of small/medium regional fibre and wireless ISPs.
- Recorded hybrid SaaS direction: subscriber self-service, optional native ISP management and integration-first mode.
- Recorded the requirement not to force ISPs to replace suitable existing backends.
- Recorded modular vendor-agnostic adapters and the prohibition on universal compatibility claims at launch.
- Recorded RMSK Bridge direction and its security/reliability requirements.
- Recorded multi-tenant isolation, payment integrity and separation of SaaS subscription billing from ISP subscriber payments.
- Recorded the five-stage owner-controlled workflow and documentation-first rule.
- Created initial requirements traceability and decision records.
- No application code or product feature was implemented by this documentation commit.

## Change-entry rule

For every future explicitly approved feature or business/technical decision, add a dated entry describing:
- What was approved.
- Which documents/requirements changed.
- Whether implementation is proposed, underway, implemented or verified.
- Tests or evidence, when applicable.


## 2026-11-06 — ISP registration and onboarding approved
- Approved a controlled ISP application workflow with contact verification and authorized RMSK review.
- Approved application states: Pending, Under Review, Additional Information Required, Approved, Rejected and Suspended.
- Approved tenant and initial owner provisioning only after application approval, with idempotency and recovery safeguards.
- Approved a mode-aware onboarding wizard for business profile, branding, operating model, network configuration, integrations/Bridge, packages, payment configuration, staff and go-live checks.
- Required separate application, tenant lifecycle and SaaS subscription/billing states; approval alone does not activate a paid subscription or mark an ISP live.
- Updated PRD, architecture, decision log and requirements traceability.
- This is a documented product requirement; no application code has been implemented.

## 2026-11-06 — Backend and database technology foundation approved
- Approved NestJS with Node.js/TypeScript as the central backend framework and business-logic authority.
- Approved PostgreSQL as the primary relational database, initially managed/hosted through Supabase, with a portability guardrail against unnecessary Supabase-specific lock-in.
- Approved Prisma as the ORM/data-access layer.
- Approved Redis with BullMQ for cache and asynchronous background jobs.
- Approved Go for the local RMSK Bridge.
- Recorded that this is a technology-direction decision, not an implementation or production-readiness claim.
- Left tenant schema strategy, exact RLS policies, Supabase plan, Redis provider, deployment topology, pooling, backups and production sizing open for detailed design.
- Updated architecture, tenancy/data, decision log and requirements traceability. No application code was implemented by this documentation change.

## 2026-11-06 — Shared-table multi-tenant isolation architecture approved
- Approved one shared PostgreSQL database and shared tenant-owned tables as the initial model, with a unique permanent UUID tenant identifier for each ISP.
- Approved defence in depth: NestJS server-side identity, membership and permission checks; PostgreSQL RLS that fails closed; and tenant-aware composite constraints/foreign keys.
- Required explicit user-to-tenant membership; user identity is not itself a tenant, and multi-tenant membership is independently authorized.
- Required tenant scoping across queries, cache, jobs, events, WebSockets, payment callbacks, Bridge identities, notifications, files, reports and exports.
- Required safe transaction-scoped tenant context with no leakage through Prisma/database connection pooling.
- Required automated cross-tenant negative tests, including relationship manipulation, missing/invalid context and pooled connection reuse.
- Preserved a future option for dedicated database isolation tiers for larger tenants.
- Recorded detailed schema, exact RLS policies, trusted context mechanism, membership/role schema and test implementation as pending architecture work.
- Updated architecture, tenancy/data, security, decision log and requirements traceability. No application code or security control was implemented by this documentation change.


## 2026-11-06 — Stage 3 detailed database architecture improvements documented
- Completed the Improvement stage following the independent Critical Reviewer assessment.
- Refined the proposal for transaction-local tenant context, explicitly documenting that a freely settable PostgreSQL custom setting is not an unforgeable boundary against a compromised application/database role.
- Added database runtime/migration/operations credential separation, RLS policy-composition safeguards, composite relationship enforcement and Prisma transaction/pool requirements.
- Expanded tenant-boundary requirements for BullMQ jobs, payment callbacks, Bridge installations, cache, search, files, WebSockets, notifications, reports and exports.
- Added scoped platform support and break-glass access, idempotent provisioning, migration/recovery safeguards and adversarial security verification gates.
- Added proposed traceability requirements TEN-010 through TEN-019.
- **Status:** Stage 3 proposal only; pending explicit owner approval in Stage 4. No new approved decision, application code, database schema, RLS policy, credential or infrastructure change was made. Existing D-016 and D-017 approvals remain unchanged.


## 2026-11-06 — Goldfish QA integrated into the five-stage workflow
- Added Goldfish QA as a mandatory cross-stage persona-based review method, not a separate sixth stage.
- Defined first-time visitor, ISP applicant/owner, staff, subscriber, platform support, mobile/constrained-connectivity and unfamiliar/error-prone perspectives.
- Added responsibilities to all five stage playbooks and persona-based acceptance coverage.
- **Status:** Workflow/documentation requirement established. This change did not execute product tests or modify application code.


## 2026-11-06 — Repository and product-planning Goldfish audit
- Added docs/GOLDFISH_AUDIT.md with persona walkthroughs, 17 prioritized findings, proposed solutions, seven golden journeys and readiness gates.
- Recorded the audit in Stage 1–3 run histories and the central workflow tracker.
- **Status:** Documentation audit completed; recommendations pending owner approval. No application code or runtime tests were performed.


## 2026-11-06 — Goldfish audit improvement and MVP planning framework
- Added a structured Pakistan ISP discovery guide, evidence record, weighted MVP scoring method, hard safety gates and pilot decision record.
- Added seven golden user journeys with actor, authority, state, failure/recovery and acceptance examples.
- Added a finding-to-improvement disposition for all 17 Goldfish audit findings and updated the PRD, roadmap, traceability and workflow tracker.
- **Status:** Stage 3 improvement documentation completed; Stage 4 owner approval pending. No ISP interviews, MVP selection, integration certification, application implementation or runtime tests are claimed.
