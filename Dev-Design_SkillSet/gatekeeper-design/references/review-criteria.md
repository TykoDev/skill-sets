# Gatekeeper Review Criteria — Per Deliverable Type

## Requirements Document (from researcher)

### Required Sections Checklist
- [ ] Problem statement with clear scope boundaries
- [ ] Stakeholder identification with roles
- [ ] Functional requirements with testable acceptance criteria
- [ ] Non-functional requirements with measurable thresholds
- [ ] Quality attributes mapped to ISO 25010 characteristics
- [ ] Domain model or bounded context identification
- [ ] Constraints and assumptions explicitly stated
- [ ] Out-of-scope items explicitly listed

### Accuracy Checks
- Verify acceptance criteria are truly testable (not subjective like "fast" or "user-friendly")
- Verify NFRs have numeric thresholds (response time < 200ms, not "fast response")
- Check for conflicting requirements (e.g., "real-time" + "batch processing" for same data)
- Verify stakeholder roles match responsibilities described
- Confirm domain terms are defined consistently throughout

### Completeness Checks
- Every functional requirement has at least one acceptance criterion
- Error scenarios and edge cases are addressed
- Data retention and privacy requirements are specified
- Integration points with external systems are identified
- Regulatory/compliance requirements are listed if applicable

---

## Project Plan (from planner)

### Required Sections Checklist
- [ ] Milestones with dates or relative sequencing
- [ ] Deliverable list per milestone
- [ ] Risk register with probability, impact, and mitigation
- [ ] Dependency map (what blocks what)
- [ ] Rollout strategy (phased, big-bang, canary)
- [ ] Feature flag strategy for progressive rollout
- [ ] Resource assumptions or constraints

### Accuracy Checks
- Verify milestone dependencies are logically ordered (no circular dependencies)
- Check risk mitigations are actionable (not "monitor the situation")
- Verify rollout strategy matches the architecture (can't canary-deploy a monolith without infrastructure)
- Confirm estimates are grounded in complexity, not arbitrary dates
- Check for missing dependencies between deliverables

### Consistency Checks
- Milestones align with requirements scope
- Risks identified in the plan match risks in the architecture document
- Rollout strategy is compatible with the chosen tech stack

---

## Architecture Document (from architect)

### Required Sections Checklist
- [ ] C4 System Context diagram
- [ ] C4 Container diagram
- [ ] C4 Component diagram (for complex domains)
- [ ] Architecture Decision Records (ADRs) for significant choices
- [ ] API contracts (OpenAPI for REST, AsyncAPI for events)
- [ ] Data model with entity relationships
- [ ] Security architecture (auth flows, trust boundaries)
- [ ] Deployment topology (environments, regions)
- [ ] Cross-cutting concerns (logging, caching, config, i18n)
- [ ] Alternatives considered with trade-off analysis

### Accuracy Checks
- Verify C4 diagrams are internally consistent (containers in context diagram match container diagram)
- Check ADR decisions are justified with concrete reasoning (not "it's the standard")
- Verify API contracts match the data models
- Check that stated architecture pattern (Clean/Hexagonal/etc.) is actually reflected in the structure
- Verify tech stack versions exist and are compatible with each other
- Confirm performance claims are realistic for the chosen architecture

### Feasibility Checks
- Can the deployment topology actually handle the stated NFRs?
- Are the chosen technologies compatible (e.g., don't specify a library that doesn't exist for the runtime)
- Is the complexity appropriate for the team size and timeline?
- Are there single points of failure in the architecture?

---

## Frontend Specification (from designer)

### Required Sections Checklist
- [ ] Frontend architecture pattern decision (SPA, SSR, SSG, Islands)
- [ ] Component hierarchy and design system
- [ ] State management strategy
- [ ] Routing strategy
- [ ] Accessibility requirements (WCAG 2.2 compliance level)
- [ ] Performance targets (Core Web Vitals)
- [ ] Responsive design breakpoints
- [ ] Browser/device support matrix

### Accuracy Checks
- Verify chosen frontend framework matches the project requirements
- Check state management choice is appropriate (not Redux for simple apps)
- Verify accessibility claims meet WCAG 2.2 Level AA requirements
- Confirm Core Web Vitals targets are realistic for the chosen architecture
- Check component naming follows stated conventions consistently

### Consistency Checks
- Frontend tech stack aligns with architect's container diagram
- API consumption patterns match the API contracts in the architecture doc
- State management approach aligns with data flow patterns in architecture

---

## Implementation Specification (from engineer)

### Required Sections Checklist
- [ ] Repository structure with directory layout
- [ ] Code organization pattern (domain-first)
- [ ] Testing strategy with tool choices and coverage targets
- [ ] CI/CD pipeline design
- [ ] Docker/containerization configuration
- [ ] Environment variable contract (.env specification)
- [ ] Security implementation details (OWASP controls)
- [ ] Observability setup (OpenTelemetry configuration)
- [ ] Code quality tooling configuration
- [ ] Migration and data seeding strategy

### Accuracy Checks
- Verify dependency versions are current and compatible
- Check CI/CD pipeline stages are logically ordered
- Verify Docker configurations follow multi-stage build best practices
- Confirm testing strategy covers the testing pyramid/trophy/vase model appropriately
- Check ORM configurations match the database stated in architecture
- Verify security controls address OWASP Top 10 items relevant to the application

### Consistency Checks
- Repository structure matches the architecture's domain model
- CI/CD pipeline produces artifacts needed by the deployment topology
- Environment variables defined here match what the architecture expects
- Testing tools are compatible with the chosen framework and runtime
- Observability setup aligns with the reliability requirements (SLOs)

---

## Cross-Deliverable Consistency Matrix

When reviewing any deliverable, cross-check against previously approved deliverables:

| Check | Source A | Source B | What to Verify |
|-------|----------|----------|----------------|
| Domain alignment | Requirements | Architecture | Same bounded contexts, same entities |
| API match | Architecture | Implementation | OpenAPI contracts match route definitions |
| Stack consistency | Architecture | Implementation | Same runtime versions, same frameworks |
| NFR coverage | Requirements | Reliability/Impl | Performance targets addressed with specific mechanisms |
| Security coverage | Requirements | Security/Impl | All security requirements have implementation controls |
| Scope containment | Requirements | All | No deliverable exceeds the defined scope |
