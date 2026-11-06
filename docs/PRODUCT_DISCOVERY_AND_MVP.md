# Product Discovery and MVP Definition Framework

**Status:** Proposed planning framework — pending owner approval.  
**Market focus:** Pakistan-first, small/medium regional fibre and wireless ISPs.  
**Purpose:** Select a narrow, evidence-backed first pilot without weakening the approved Native / Integrated / Hybrid product direction.

## 1. Decision principles

1. Do not select the MVP from assumptions, personal preference or competitor feature lists alone.
2. Interview actual ISP decision-makers and daily operators; record evidence separately from opinions.
3. Do not promise a vendor integration until its exact product/version, method, capabilities and test environment are verified.
4. Preserve the approved principle that an ISP must not be forced to replace a suitable existing backend.
5. A first pilot may be narrower than the long-term platform. Narrow scope is not a change to the long-term product vision.
6. Treat price, willingness to pay, legal requirements and integration support cost as evidence to gather, not facts already established.
7. Do not collect ISP subscriber personal data during discovery unless necessary, authorized and appropriately protected. Prefer anonymized examples.

## 2. Discovery sample

Recruit a deliberately varied sample of ISP businesses rather than only friendly or technically advanced contacts. Include, where available:
- Fibre-first, wireless-first and mixed access providers.
- Different operating scales (small, growing and more established); do not invent hard subscriber-count bands before discovery.
- ISPs using spreadsheets/manual processes, an established third-party billing/CRM platform, and mixed tools.
- Different network/authentication environments.
- Owner/decision-maker and at least one frontline operator (billing/cashier, support or network operations) from some of the same businesses.

Record sample size and limitations. A small discovery sample is directional, not statistically representative of all Pakistani ISPs.

## 3. Interview guide

Ask for recent real examples. Avoid leading questions such as “Would you use our platform?”

### Business and operating context
1. What services and access technologies do you currently provide?
2. Which areas do you serve, and what does a normal working day look like?
3. Who makes software and payment-process decisions?
4. Which tasks consume the most staff time or cause the most customer complaints?

### Current systems and data ownership
5. Which billing, CRM, AAA/RADIUS, router, OLT/ONU, payment and support tools are currently used? Capture exact vendor, product, version and deployment model only when the ISP is willing.
6. Which system is authoritative for subscriber identity, package, invoice, payment, service state and network configuration?
7. Where is the same information entered more than once?
8. What currently happens when systems disagree or connectivity is lost?
9. Which operations must never be controlled by an external platform?

### Pain and consequences
10. Tell us about the last billing/payment mismatch. What happened and how was it resolved?
11. Tell us about the last failed activation, suspension or package change.
12. How are cash, bank transfer and online payments recorded and reconciled?
13. What reports or information are difficult to obtain?
14. What does the problem cost in staff time, lost revenue, delayed service or customer dissatisfaction?

### Existing solution and change constraints
15. What works well in the current system and must not be lost?
16. What would make you refuse a new platform or integration?
17. What migration, downtime, training or data-export constraints exist?
18. Would read-only integration be useful before allowing write actions? Why?

### Subscriber experience
19. How do subscribers check bills, pay, report payment and contact support today?
20. Which subscriber requests create the most repetitive staff work?
21. What happens when a payment is pending, duplicated or not reflected?

### Commercial evidence
22. What do you currently pay for software, connectivity tools or outsourced support (only if comfortable sharing)?
23. Who approves a purchase, and what evidence do they need?
24. What measurable result would justify paying for a new service?
25. What budget range or pricing structure would be acceptable? Record exact answer and context; do not lead the respondent.

### Pilot readiness
26. Would you consider a limited pilot? What conditions would be required?
27. Can a safe test environment, test account or non-production device be made available?
28. Who would be the pilot owner and daily contact?
29. What would cause the pilot to stop immediately?

## 4. Evidence record

For each interview, capture:
- Interview ID, date, participant role and consent.
- ISP profile at a non-identifying level unless explicit permission is given.
- Current tools and exact versions (known / unknown).
- Direct quote or observed example, clearly distinguished from interviewer interpretation.
- Frequency and impact of each pain point.
- Current workaround and its cost.
- Data owner and operational authority by domain.
- Integration constraints and security concerns.
- Stated willingness and conditions for pilot/payment.
- Confidence: observed evidence, corroborated report, single-source claim or hypothesis.
- Follow-up action and owner.

Do not place confidential credentials, subscriber records or unnecessary personal information in discovery notes or GitHub.

## 5. MVP candidate scoring

Score each candidate capability/integration from 1–5, with written evidence and confidence:
- Customer pain severity and frequency — 25%.
- Number/proportion of interviewed target ISPs reporting it — 20%.
- Measurable business value — 15%.
- Integration feasibility and test access — 15%.
- Security/operational risk (higher score means safer/easier to control) — 10%.
- Delivery/support effort (higher score means lower effort) — 10%.
- Willingness to pilot/pay — 5%.

Weighted score = sum(score × weight). A score is a prioritization aid, not automatic approval. Mark unknown evidence as unknown; do not silently score it as 3. Run sensitivity checks because a small sample can skew results.

### Hard gates regardless of score
A capability cannot enter a pilot if:
- Tenant isolation or authorization is not adequately proven.
- Financial correctness/idempotency is not tested where money is involved.
- The ISP has not authorized the relevant integration and data access.
- Write actions lack clear ownership, permission, confirmation and recovery.
- No safe test environment or rollback/disable procedure exists for high-impact network operations.
- Legal/provider requirements are unresolved for the proposed activity.

## 6. MVP scope decision record

Before approving an MVP, publish:
- Target ISP profile and evidence supporting it.
- Selected operating mode for the pilot (Native, Integrated or Hybrid) and why.
- First integration: vendor/product/version, protocol, deployment and exact tested capabilities.
- Included user personas and end-to-end journeys.
- Included features and explicit exclusions.
- Data source-of-truth and action authority for every included domain.
- Security, privacy and operational prerequisites.
- Pilot duration, support owner and rollback/exit plan.
- Success metrics, baseline, target, measurement method and review date.
- Known limitations and customer-facing compatibility statement.

## 7. Pilot success measures

Choose targets only after collecting a baseline and owner approval. Candidate measures:
- Time to onboard an ISP and configure its first usable workflow.
- Percentage of synchronization events reconciled without manual correction.
- Payment matching/reconciliation accuracy and time to resolve exceptions.
- Subscriber self-service task completion and support-contact reduction.
- Bridge uptime/connectivity and queue recovery time (where Bridge is used).
- Failed/duplicate service actions and time to recover.
- Staff task completion time and training/support hours.
- Pilot adoption, active usage and willingness to continue/pay.

Never invent a target or claim improvement without baseline and measured evidence.

## 8. Decision gate

**Stage 4 owner approval is required** before selecting the first MVP, target segment, first integration, commercial assumptions or pilot success targets. This document supplies a repeatable evidence-gathering method; it does not make those decisions.
