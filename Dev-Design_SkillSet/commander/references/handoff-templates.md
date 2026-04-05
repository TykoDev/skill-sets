# Commander Handoff Templates

## Phase 1 Delegation: researcher

```markdown
## COMMANDER DELEGATION: Phase 1 — Requirements & Domain Analysis

### Original User Request
[Paste user's project description verbatim]

### Identified Constraints
- **Timeline**: [Specified or "Not specified"]
- **Team size**: [Specified or "Not specified"]
- **Budget**: [Specified or "Not specified"]
- **Regulatory**: [Specified or "None identified"]
- **Technical constraints**: [Specified or "None identified"]
- **Existing systems**: [Specified or "Greenfield project"]

### Scope Classification
[Full system design / Feature design / Component design]

### Commander Notes
[Any additional context, clarifications from user intake, or assumptions made]

### Expected Deliverables
1. Software Requirements Specification (SRS) following references/requirements-template.md
2. Domain Analysis with bounded contexts and domain model

### Gatekeeper-Design Submission Required
Submit completed deliverables to gatekeeper-design for review before returning to commander.
```

---

## Phase 2 Delegation: planner

```markdown
## COMMANDER DELEGATION: Phase 2 — Project Planning & Delivery Strategy

### Original User Request
[Paste user's project description verbatim]

### Approved Upstream Deliverables
- ✅ SRS (from researcher, gatekeeper-design-approved)
- ✅ Domain Analysis (from researcher, gatekeeper-design-approved)

### Key Requirements Summary
- Total functional requirements: [count]
- MUST-priority items: [count]
- Key NFRs: [list critical ones]
- Bounded contexts: [count]

### Commander Notes
[Any timeline pressure, phasing preferences, or risk areas to prioritize]

### Expected Deliverables
1. Project Plan with phase breakdown and milestones
2. Risk register with mitigations
3. Rollout strategy with feature flag lifecycle
4. Technology decision timeline

### Gatekeeper-Design Submission Required
Submit completed deliverables to gatekeeper-design for review before returning to commander.
```

---

## Phase 3 Delegation: architect

```markdown
## COMMANDER DELEGATION: Phase 3 — System Architecture

### Original User Request
[Paste user's project description verbatim]

### Approved Upstream Deliverables
- ✅ SRS (from researcher)
- ✅ Domain Analysis (from researcher)
- ✅ Project Plan (from planner)

### Key Architecture Drivers
- Primary quality attributes: [from SRS NFRs]
- Domain complexity: [number of bounded contexts]
- Scale requirements: [from NFRs]
- Integration points: [from SRS external interfaces]
- Team constraints: [from planner]

### Tech Stack Preference
[If user specified a preference, note it here. Otherwise: "No preference — recommend based on requirements"]

### Available Tech Stack Templates
Backend: tech-stacks/{node-typescript, bun-typescript, deno-typescript, python-fastapi, rust-axum, go-gin, dotnet-aspnet}.md
Frontend: tech-stacks/{react-nextjs, react-tanstack, vue-nuxt, angular, astro, vite-spa, svelte-sveltekit}.md

### Expected Deliverables
1. Arc42 Architecture Document with C4 diagrams (Mermaid)
2. Architecture Decision Records (ADRs) for significant decisions
3. API Contracts (OpenAPI/AsyncAPI specifications)
4. Data Model (ERD with entity definitions)

### Gatekeeper-Design Submission Required
Submit completed deliverables to gatekeeper-design for review before returning to commander.
```

---

## Phase 4 Delegation: designer

```markdown
## COMMANDER DELEGATION: Phase 4 — Frontend Architecture & UI/UX

### Original User Request
[Paste user's project description verbatim]

### Approved Upstream Deliverables
- ✅ SRS (from researcher)
- ✅ Architecture Document (from architect)
- ✅ API Contracts (from architect)

### Frontend Requirements
- SEO requirements: [Critical / Not important]
- Target devices: [Desktop / Mobile / Both]
- Accessibility level: [WCAG 2.2 AA / custom]
- Performance targets: [from SRS NFRs]
- Framework preference: [if user specified]

### Architecture Context
- Rendering approach recommended by architect: [SSR/SPA/SSG/Islands]
- Backend API protocol: [REST/GraphQL/tRPC/gRPC]
- Auth flow: [from architecture document]

### Expected Deliverables
1. Frontend Architecture Specification
2. Component hierarchy and design system spec
3. State management strategy
4. Accessibility requirements
5. Performance targets

### Gatekeeper-Design Submission Required
Submit completed deliverables to gatekeeper-design for review before returning to commander.
```

---

## Phase 5 Delegation: engineer

```markdown
## COMMANDER DELEGATION: Phase 5 — Implementation Specification

### Original User Request
[Paste user's project description verbatim]

### Approved Upstream Deliverables
- ✅ SRS (from researcher)
- ✅ Project Plan (from planner)
- ✅ Architecture Document (from architect)
- ✅ API Contracts (from architect)
- ✅ ADRs (from architect)
- ✅ Frontend Spec (from designer) [if applicable]

### Architecture Decisions Summary
- Architecture style: [from ADR-001]
- Runtime: [from ADR]
- Framework: [from ADR]
- Database: [from ADR]
- ORM: [from ADR]

### Tech Stack Selection
- Backend: [final stack from architect's ADRs]
- Frontend: [final stack from designer's spec]

### Expected Deliverables
1. Repository structure (complete directory layout)
2. Environment variable contract (.env specification)
3. Testing strategy (tools, levels, coverage targets)
4. CI/CD pipeline specification
5. Docker/containerization configuration
6. Security controls (OWASP mapping)
7. Observability configuration (OpenTelemetry)
8. Code quality tooling

### Gatekeeper-Design Submission Required
Submit completed deliverables to gatekeeper-design for review before returning to commander.
```

---

## Gatekeeper-Design Submission Template

```markdown
## GATEKEEPER-DESIGN REVIEW REQUEST

### Phase
[Phase number and name]

### Source Skill
[researcher / planner / architect / designer / engineer]

### Deliverable(s)
[List all documents being submitted]

### Upstream Context
[What approved deliverables informed this work]

### Review Request
Execute full adversarial review per gatekeeper-design protocol.
Validate against review-criteria.md for this deliverable type.
Cross-check against upstream approved deliverables for consistency.
```

---

## Final Delivery Template

```markdown
# DESIGN SPECIFICATION PACKAGE
## [Project Name]

**Prepared by**: Dev Design SkillSet Pipeline
**Date**: [YYYY-MM-DD]
**Status**: Complete — All phases gatekeeper-design-approved

---

## Executive Summary
[One paragraph: what was designed, key decisions, recommended approach]

## Package Contents

| # | Deliverable | Source | Gatekeeper-Design Status |
|---|------------|--------|-------------------|
| 1 | Software Requirements Specification | researcher | ✅ APPROVED |
| 2 | Domain Analysis | researcher | ✅ APPROVED |
| 3 | Project Plan | planner | ✅ APPROVED |
| 4 | Architecture Document (Arc42) | architect | ✅ APPROVED |
| 5 | API Contracts | architect | ✅ APPROVED |
| 6 | ADR Collection | architect | ✅ APPROVED |
| 7 | Frontend Architecture Spec | designer | ✅ APPROVED |
| 8 | Implementation Specification | engineer | ✅ APPROVED |

## Tech Stack Summary

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Backend Runtime | [e.g., Node.js 22] | [Brief rationale] |
| Backend Framework | [e.g., Fastify 5] | [Brief rationale] |
| Frontend Framework | [e.g., Next.js 15] | [Brief rationale] |
| Database | [e.g., PostgreSQL 16] | [Brief rationale] |
| ORM | [e.g., Drizzle] | [Brief rationale] |
| Validation | [e.g., Zod v4] | [Brief rationale] |

## Recommended Next Actions

1. [First action to take when beginning implementation]
2. [Second action]
3. [Third action]

## Open Questions / Deferred Decisions
[Any items that require further discussion or were deferred]
```
