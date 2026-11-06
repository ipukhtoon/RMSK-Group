# RMSK Group

**RMSK Group** is the approved working identity for a Pakistan-first technology business whose first planned product is a multi-tenant SaaS platform for Internet Service Providers (ISPs).

> **Project status:** Documentation and product planning. The backend/database technology direction is approved, but no application implementation or production deployment is implied. Build decisions require explicit owner approval under [WORKFLOW.md](docs/WORKFLOW.md).

## Product direction

The platform is intended to combine:
- Customer self-service and payment experiences.
- Optional native ISP operations for providers that do not have a suitable backend.
- A modular integration engine and lightweight **RMSK Bridge** for ISPs that want to keep their existing billing, subscriber, authentication or network systems.

**Core principle:** Do not force an ISP to replace its existing backend. Support different integration methods through modular adapters, while validating each integration before claiming compatibility.

## Approved technology direction

- Backend: NestJS (Node.js/TypeScript).
- Primary database: PostgreSQL, initially hosted/managed through Supabase.
- ORM/data access: Prisma.
- Cache and background jobs: Redis with BullMQ.
- Local ISP integration agent: Go-based RMSK Bridge.

These are approved technology choices, not claims of implementation or production readiness. Detailed tenancy/schema design, hosting plans/providers, security policies, backups and deployment topology remain to be specified and verified.

## Market focus

- Initial market: Pakistan.
- Initial target: small and medium regional fibre and wireless ISPs.
- International expansion is not a present naming or launch requirement. The business may choose a different brand if it later expands internationally.

## Company identity

- Approved working brand: **RMSK Group**.
- No separate product name has been approved.
- The legal entity name, SECP reservation, trademark status and domains are not yet confirmed. Do not describe the name as legally cleared or registered.
- The user may use a suitable Pakistani legal suffix when forming the entity, subject to SECP approval.

## Documentation rules

These Markdown files are the project’s source of truth for approved product direction and decisions. Whenever the owner explicitly approves a feature or changes business/technical logic, update the relevant requirements, architecture, decision records, traceability and changelog before or alongside implementation. Keep proposals clearly marked as proposals.

## Start here

- [Product vision](docs/PRODUCT_VISION.md)
- [PRD](docs/PRD.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Tenancy and data](docs/TENANCY_AND_DATA.md)
- [Integration and Bridge strategy](docs/INTEGRATIONS_AND_BRIDGE.md)
- [Security](docs/SECURITY.md)
- [Owner workflow](docs/WORKFLOW.md)
- [Decision log](docs/DECISIONS.md)
- [Requirements traceability](docs/REQUIREMENTS_TRACEABILITY.md)
- [Changelog](docs/CHANGELOG.md)


## Five-stage project control

Every substantial workstream follows the five mandatory stages, with a dedicated playbook and run history:
- [Stage 1 — Architect](docs/stages/01-ARCHITECT.md)
- [Stage 2 — Critical Reviewer](docs/stages/02-CRITICAL-REVIEWER.md)
- [Stage 3 — Improvement](docs/stages/03-IMPROVEMENT.md)
- [Stage 4 — Owner Approval](docs/stages/04-OWNER-APPROVAL.md)
- [Stage 5 — Implementation & Verification](docs/stages/05-IMPLEMENTATION-VERIFICATION.md)
- [Workflow and current run tracker](docs/WORKFLOW.md)

**Tracking rule:** After every stage run, update its run history and the central tracker with the outcome, status, evidence, unresolved issues and commit reference. Approval is scope-specific; no implementation starts before explicit Stage 4 approval.


## Goldfish QA
The five-stage workflow includes **Goldfish QA** as a mandatory cross-stage review lens. It simulates relevant first-time visitors, ISP applicants/owners, staff roles, subscribers, platform support, mobile/constrained-connectivity users and confused/misuse scenarios. It is integrated into all five stages; it is not a separate sixth stage. See [Owner workflow](docs/WORKFLOW.md) and [Testing & acceptance](docs/TESTING_AND_ACCEPTANCE.md).
