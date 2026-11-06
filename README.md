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
