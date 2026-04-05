---
name: planner
description: >-
  This skill should be used when the user or commander asks to "create a project
  plan", "define milestones", "assess risks", "plan the delivery", "create a
  rollout strategy", "estimate effort", "define feature flags strategy", or
  "create a risk register". It produces a structured project plan with
  milestones, risk register, delivery strategy, and progressive rollout plan
  based on approved requirements.
version: 1.0.0
---

# Planner — Project Planning & Delivery Strategy

## Purpose

This skill performs Phase 2 of the Dev Design SkillSet pipeline. It takes
gatekeeper-design-approved requirements (from researcher) and transforms them into
a structured project plan with milestones, risk register, delivery strategy,
and progressive rollout approach. The output drives downstream phases by
establishing scope, sequencing, and risk boundaries.

## When to Activate

Activate when commander delegates Phase 2 (Project Planning) after the
researcher's requirements document has been approved by gatekeeper-design.

---

## Workflow

### Step 1: Ingest Approved Requirements

Read the gatekeeper-design-approved SRS and domain analysis. Extract:
- Total scope (functional requirements count and complexity)
- Non-functional requirements (performance, security, compliance constraints)
- Domain complexity (number of bounded contexts, integration points)
- Stated constraints (timeline, budget, team size, regulatory deadlines)

### Step 2: Define Project Phases

Break the project into logical delivery phases. Each phase MUST be
independently deployable and valuable.

```markdown
## Phase [N]: [Phase Name]
**Duration**: [Estimated weeks/sprints]
**Goal**: [What this phase achieves]
**Deliverables**:
  - [Deliverable 1]
  - [Deliverable 2]
**Requirements covered**: [FR-001, FR-002, ...]
**Dependencies**: [Phase N-1 must be complete / External: API ready]
**Exit criteria**: [What must be true to consider this phase done]
```

Apply the principle: deliver the most valuable, highest-risk items first.
Early phases should prove the architecture and de-risk unknowns.

### Step 3: Create Milestone Map

Define milestones with concrete deliverables and acceptance criteria:

| Milestone | Target Date | Deliverables | Acceptance Criteria |
|-----------|-------------|-------------|---------------------|
| M1: Foundation | Week N | Project scaffolding, CI/CD, DB schema | Pipeline green, schema migrated |
| M2: Core MVP | Week N+X | Core features functional | All MUST requirements passing |
| M3: Integration | Week N+Y | External integrations | Contract tests passing |
| M4: Hardening | Week N+Z | Security, performance, accessibility | NFRs verified |
| M5: Launch | Week N+W | Production deployment | SLOs met for 48 hours |

### Step 4: Build Risk Register

For every identified risk, produce a structured entry:

```markdown
### RISK-001: [Risk Title]
**Category**: Technical | Schedule | Resource | External | Security
**Probability**: High | Medium | Low
**Impact**: High | Medium | Low
**Risk Score**: [Probability × Impact]
**Description**: [What could go wrong]
**Mitigation**: [Specific, actionable steps to reduce probability or impact]
**Contingency**: [What to do if the risk materializes]
**Owner**: [Who is responsible for monitoring]
**Review cadence**: [How often to reassess]
```

Consult `references/project-plan-template.md` for risk assessment matrices
and common risk patterns.

### Step 5: Define Rollout Strategy

Specify the progressive deployment approach:

1. **Deployment model**: Blue/green, canary, rolling, or feature-flag-based
2. **Feature flag lifecycle**: Deploy OFF → internal team → 5% canary → 
   monitor metrics → 25% → 50% → 100% → remove flag and dead code
3. **Rollback plan**: Conditions that trigger rollback, rollback procedure,
   maximum rollback time
4. **Monitoring gates**: Which metrics must be healthy to proceed at each stage
5. **Communication plan**: Who is notified at each rollout stage

### Step 6: Define Technology Decision Timeline

Map when technology decisions must be finalized:

| Decision | Deadline | Impact if Delayed | Decision Owner |
|----------|----------|-------------------|----------------|
| Runtime selection | Phase 1 start | Blocks all development | Architect |
| Database selection | Phase 1 start | Blocks schema design | Architect |
| Frontend framework | Phase 1 | Blocks UI development | Designer |
| CI/CD platform | Phase 1 | Blocks pipeline setup | Engineer |
| Hosting/deployment | Phase 2 | Blocks staging environment | Engineer |

### Step 7: Submit to Gatekeeper

Package the complete project plan and submit to `gatekeeper-design` for review.
Address any REVISE findings and resubmit until APPROVED.

---

## Output Format

The planner produces one consolidated deliverable:

**Project Plan** containing:
1. Phase breakdown with scope mapping
2. Milestone map with dates and acceptance criteria
3. Risk register with mitigations
4. Dependency graph
5. Rollout strategy with feature flag lifecycle
6. Technology decision timeline
7. Resource assumptions

---

## Additional Resources

### Reference Files

For detailed templates and risk patterns:
- **`references/project-plan-template.md`** — Full project plan template, risk assessment matrix, rollout strategy patterns, and estimation guidance
