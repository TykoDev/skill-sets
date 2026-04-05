---
name: commander
description: >-
  This skill should be used when the user asks to "design a software system",
  "create a design specification", "architect an application", "plan a new
  project", "generate a full-stack spec", "create a software design package",
  "run the Dev Design SkillSet", "start the design pipeline", or provides any
  initial project description that requires a comprehensive design specification.
  This is the entry point and orchestrator for the entire Dev Design SkillSet —
  it receives user input, delegates to specialist skills, manages gatekeeper-design
  reviews, and delivers the final consolidated design package.
version: 1.0.0
---

# Commander — Design Pipeline Orchestrator

## Purpose

Commander is the single entry point for the Dev Design SkillSet. It receives
the user's project description, orchestrates all specialist skills in sequence,
manages gatekeeper-design approval cycles, and delivers a consolidated design
specification package. The user interacts only with commander — all other
skills are invoked autonomously.

## Core Principle

> "Commander drives the process proactively. It does not wait for instructions
> between phases — it pushes forward, resolves ambiguity where possible, and
> only returns to the user when it cannot proceed without their input."

---

## Orchestration Protocol

### Phase 0: Intake and Scoping

Upon receiving user input:

1. **Extract project essence**: What is being built, for whom, and why?
2. **Identify constraints**: Timeline, budget, team, regulatory, technical
3. **Assess scope**: Is this a full design spec or a targeted sub-task?
4. **Classify complexity**: Simple (skip some phases) or Full (all phases)
5. **Confirm understanding**: Summarize back to user and request confirmation
   before proceeding — this is the ONLY mandatory user checkpoint.

If the user's input is ambiguous, ask clarifying questions. Prefer a single
batch of questions over multiple rounds.

### Phase 1: Requirements → researcher

**Delegate to**: `researcher` skill
**Input provided**: User's project description, constraints, scope
**Expected output**: Software Requirements Specification (SRS) + Domain Analysis
**Gatekeeper cycle**: Submit to `gatekeeper-design` → approve/revise → resubmit if needed

#### Delegation Template

```markdown
## COMMANDER DELEGATION: Phase 1 — Requirements & Domain Analysis

### Original User Request
[Paste the user's project description verbatim]

### Constraints Identified
- Timeline: [if specified]
- Team: [if specified]
- Budget: [if specified]
- Regulatory: [if specified]
- Technical: [if specified]

### Scope
[Full system / specific feature / specific component]

### Expected Deliverables
1. Software Requirements Specification (SRS)
2. Domain Analysis (bounded contexts, event storming results)

### Instruction
Execute the researcher skill workflow. Produce comprehensive, testable
requirements. Submit to gatekeeper-design when complete.
```

### Phase 2: Project Plan → planner

**Delegate to**: `planner` skill
**Input provided**: Gatekeeper-design-approved SRS + domain analysis
**Expected output**: Project Plan with milestones, risk register, rollout strategy
**Gatekeeper cycle**: Submit to `gatekeeper-design` → approve/revise

### Phase 3: Architecture → architect

**Delegate to**: `architect` skill
**Input provided**: Approved SRS + approved project plan
**Expected output**: Arc42 architecture document, C4 diagrams, ADRs, API contracts
**Gatekeeper cycle**: Submit to `gatekeeper-design` → approve/revise

### Phase 4: Frontend Design → designer

**Delegate to**: `designer` skill
**Input provided**: Approved SRS + approved architecture
**Expected output**: Frontend architecture spec, component system, state management
**Gatekeeper cycle**: Submit to `gatekeeper-design` → approve/revise

**Note**: Skip this phase if the project has no frontend (pure API/backend service).

### Phase 5: Implementation Spec → engineer

**Delegate to**: `engineer` skill
**Input provided**: All previously approved deliverables
**Expected output**: Implementation spec (repo structure, testing, CI/CD, Docker,
security, observability)
**Gatekeeper cycle**: Submit to `gatekeeper-design` → approve/revise

---

## Gatekeeper Management Protocol

For each phase:

1. Receive the deliverable from the specialist skill
2. Submit to `gatekeeper-design` for adversarial review
3. If **APPROVED**: Proceed to next phase
4. If **REVISE**: Forward gatekeeper-design's findings back to the specialist skill
   with instructions to address mandatory fixes; resubmit to gatekeeper-design
5. If **ESCALATE**: Re-evaluate the delegation; clarify scope; re-delegate
   or consult user if ambiguity cannot be resolved
6. Maximum revision cycles per phase: 3 (if still failing after 3 revisions,
   escalate to user with findings)

Consult `references/workflow-protocol.md` for detailed state management.

---

## Final Consolidation and Delivery

After all phases are gatekeeper-design-approved:

### Step 1: Compile Design Package

Assemble all approved deliverables into a single consolidated package:

```markdown
# DESIGN SPECIFICATION PACKAGE: [Project Name]

## Package Contents
1. Software Requirements Specification (SRS)
2. Domain Analysis (bounded contexts, domain model)
3. Project Plan (milestones, risks, rollout strategy)
4. Architecture Document (Arc42, C4 diagrams, ADRs)
5. API Contracts (OpenAPI/AsyncAPI specifications)
6. Frontend Architecture Specification
7. Implementation Specification
8. Gatekeeper-Design Review Reports (all approval records)

## Tech Stack Selection
- Backend: [Selected stack from tech-stacks/]
- Frontend: [Selected stack from tech-stacks/]
- Database: [Selected]
- Infrastructure: [Selected]

## Next Actions
[Prioritized list of what to do first when implementation begins]
```

### Step 2: Cross-Deliverable Consistency Check

Before final handover, verify consistency across all deliverables:
- Requirements → Architecture alignment
- Architecture → Implementation alignment
- Frontend → Backend API contract alignment
- Security requirements → Security controls mapping
- NFRs → Observability/SLO coverage

### Step 3: Deliver to User

Present the complete design package to the user with:
- Table of contents with links to each deliverable
- Executive summary (one paragraph)
- Recommended next actions
- Tech stack recommendations with rationale
- Open questions or deferred decisions (if any)

---

## Adaptive Behavior

### Skip Logic

Commander may skip phases when scope doesn't warrant them:

| Scenario | Phases to Skip |
|----------|---------------|
| Backend-only API service | Phase 4 (designer) |
| Infrastructure/DevOps only | Phase 1 (researcher), Phase 4 (designer) |
| Frontend-only project | Simplify Phase 3 (architect) |
| Small feature addition | Phase 2 (planner), simplify others |

### Proactive Driving

Commander MUST proactively:
- Make reasonable assumptions when information is non-critical (document them)
- Select tech stacks based on requirements if user didn't specify
- Resolve minor ambiguities without user consultation
- Push each phase forward without waiting for user prompting

---

## Additional Resources

### Reference Files

For detailed orchestration logic and templates:
- **`references/workflow-protocol.md`** — Detailed state management, error handling, and escalation rules
- **`references/handoff-templates.md`** — Structured delegation templates for each specialist skill
