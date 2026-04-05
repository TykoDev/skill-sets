---
name: architect
description: >-
  This skill should be used when the user or commander asks to "design the
  system architecture", "create C4 diagrams", "write an ADR", "define API
  contracts", "choose architecture patterns", "create the architecture document",
  "define system boundaries", "design the data model", "select between
  monolith and microservices", or "document architecture decisions". It produces
  a comprehensive architecture document with C4 diagrams, ADRs, API contracts,
  and pattern recommendations aligned with Arc42 v9.0.
version: 1.0.0
---

# Architect — System Architecture & Technical Decisions

## Purpose

This skill performs Phase 3 of the Dev Design SkillSet pipeline. It takes
gatekeeper-design-approved requirements (from researcher) and project plan (from
planner) and produces the complete system architecture: C4 diagrams, Arc42
documentation, Architecture Decision Records, API contracts, data model,
and deployment topology.

## When to Activate

Activate when commander delegates Phase 3 (Architecture) after the
researcher's requirements and planner's project plan have been approved
by gatekeeper-design.

---

## Workflow

### Step 1: Architecture Style Selection

Based on the requirements and domain analysis, select the primary
architecture style. Document the decision as ADR-001.

| Style | When to Use | When NOT to Use |
|-------|------------|-----------------|
| **Monolith (modular)** | Small team (< 5), MVP, simple domain | Multiple teams, independent scaling needs |
| **Microservices** | Multiple teams, independent deployability | Small team, strong consistency requirements |
| **Clean Architecture** | Complex business logic, long-lived apps (5+ years) | Simple CRUD, MVPs |
| **Hexagonal** | High testability, multiple adapters needed | Simple apps |
| **Event-Driven (CQRS)** | Audit trails, temporal queries, complex workflows | Simple CRUD |
| **Serverless** | Event-triggered, variable load, cost optimization | Steady high throughput, latency-sensitive |

Consult `references/architecture-patterns.md` for detailed pattern descriptions.

### Step 2: Create C4 Diagrams

Produce diagrams at three levels using Mermaid DSL:

**Level 1 — System Context**: Shows the system as a box surrounded by users
and external systems. Answers: "What does the system do and who interacts with it?"

**Level 2 — Container**: Shows the high-level technical building blocks
(web app, API, database, message queue). Answers: "What are the main technical
components and how do they communicate?"

**Level 3 — Component**: For complex containers, shows the internal components
and their responsibilities. Answers: "How is this container organized internally?"

Do NOT create Level 4 (Code) diagrams manually — use IDE tools on demand.

```mermaid
C4Context
    title System Context Diagram

    Person(user, "End User", "Uses the application")
    System(system, "Application", "Core system being designed")
    System_Ext(payment, "Payment Gateway", "Processes payments")
    System_Ext(email, "Email Service", "Sends notifications")

    Rel(user, system, "Uses", "HTTPS")
    Rel(system, payment, "Processes payments", "HTTPS/REST")
    Rel(system, email, "Sends emails", "SMTP/API")
```

### Step 3: Produce Arc42 Architecture Document

Follow the Arc42 v9.0 template structure. The document MUST include:

1. **Introduction and Goals** — Business requirements, quality goals, stakeholders
2. **Constraints** — Technical, organizational, and regulatory constraints
3. **Context and Scope** — System context diagram (C4 Level 1), external interfaces
4. **Solution Strategy** — Fundamental technology decisions and approaches
5. **Building Block View** — Container and component diagrams (C4 Level 2-3)
6. **Runtime View** — Key sequence diagrams for critical flows
7. **Deployment View** — Infrastructure topology, environments
8. **Crosscutting Concepts** — Security, logging, configuration, error handling, i18n
9. **Architecture Decisions** — Index of ADRs
10. **Quality Requirements** — Quality tree mapping NFRs to architecture tactics
11. **Risks and Technical Debt** — Known risks and accepted debt
12. **Glossary** — Domain terms (from researcher's ubiquitous language)

Consult `references/arc42-template.md` for the complete template.

### Step 4: Write Architecture Decision Records

For every significant technical decision, create an ADR using MADR v4.0:

```markdown
---
status: "{Proposed | Accepted | Deprecated | Superseded by ADR-XXXX}"
date: YYYY-MM-DD
decision-makers: [list]
---

# ADR-XXXX: [Short Title]

## Context and Problem Statement
[What is the technical problem or decision point?]

## Decision Drivers
- [Driver 1]
- [Driver 2]

## Considered Options
1. [Option A]
2. [Option B]
3. [Option C]

## Decision Outcome
Chosen option: "[Option]", because [justification].

### Consequences
- Good: [Positive consequence]
- Bad: [Trade-off accepted]
- Neutral: [Side effect]

## Pros and Cons of Options

### [Option A]
- Good: [Pro]
- Bad: [Con]

### [Option B]
- Good: [Pro]
- Bad: [Con]
```

### Step 5: Define API Contracts

Produce API contract specifications:
- **REST APIs**: OpenAPI 3.1 specification
- **Event-driven APIs**: AsyncAPI 3.0 specification
- **Internal service-to-service**: gRPC with Protocol Buffers or tRPC
- **GraphQL**: Schema definition (if applicable)

API contracts MUST define: endpoints, request/response schemas, error formats
(RFC 7807), authentication requirements, rate limits, and versioning strategy.

### Step 6: Design Data Model

Produce the logical data model:
- Entity-relationship diagrams (Mermaid ERD)
- Key entity definitions with attributes, types, and constraints
- Relationships and cardinality
- Data flow across bounded contexts
- Migration strategy and rollback plan
- Data retention and privacy (GDPR/regulatory compliance)

### Step 7: Define Deployment Topology

Specify the infrastructure architecture:
- Environment list (dev, staging, production)
- Container orchestration (Kubernetes, ECS, serverless)
- CDN and edge computing strategy
- Database hosting and replication
- Secrets management approach
- Disaster recovery and backup strategy

### Step 8: Submit to Gatekeeper

Package the complete architecture document (Arc42), C4 diagrams, ADRs,
and API contracts. Submit to `gatekeeper-design` for adversarial review.

---

## Output Format

The architect produces:

1. **Architecture Document** (Arc42 format) with embedded C4 diagrams
2. **ADR Collection** — One ADR per significant decision
3. **API Contracts** — OpenAPI/AsyncAPI specifications
4. **Data Model** — ERD with entity definitions

---

## Additional Resources

### Reference Files

For detailed templates and pattern descriptions:
- **`references/arc42-template.md`** — Complete Arc42 v9.0 template adapted for AI-driven workflows
- **`references/architecture-patterns.md`** — Clean Architecture, Hexagonal, Event-Driven, CQRS, and microservices pattern details
