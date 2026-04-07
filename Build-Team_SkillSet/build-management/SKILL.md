---
name: build-management
description: >-
  This skill should be used when the user asks to "build this project",
  "implement this design", "start the build pipeline", "execute the
  implementation plan", "run the Build Team SkillSet", "build from this
  spec", "implement this architecture", "start coding from the design",
  or provides an approved design specification that requires translation
  into production code. This is the entry point and orchestrator for the
  entire Build Team SkillSet — it receives implementation plans, delegates
  to specialist build skills, manages gatekeeper-build review cycles, and
  delivers the final built code package.
version: 1.0.0
---

# Build Management — Build Pipeline Orchestrator

## Purpose

Build-management is the single entry point for the Build Team SkillSet. It receives
the user's approved design specification or implementation plan, orchestrates all
specialist build skills in sequence, manages gatekeeper-build approval cycles, and
delivers a consolidated production-ready code package. The user interacts only with
build-management — all other skills are invoked autonomously.

The canonical workflow is strict and fail-closed:

`build-management -> bob-the-builder -> gatekeeper-build -> test-builder -> gatekeeper-build -> security-builder -> gatekeeper-build -> cross-check-build-confirm -> gatekeeper-build -> delivery`

No mandatory phase may be skipped in the canonical pipeline, and a phase is not
accepted until `gatekeeper-build` approves it through build-management.

## Core Principle

> "Build-management drives implementation proactively. It does not wait for
> instructions between phases — it pushes forward, resolves ambiguity where
> possible, and only returns to the user when it cannot proceed without their
> input."

---

## Orchestration Protocol

### Phase 0: Intake and Validation

Upon receiving the design specification or implementation plan:

1. **Validate input completeness**: Does the specification include requirements, architecture, API contracts, and tech stack selection?
2. **Extract implementation requirements**: What modules, features, and components must be built?
3. **Identify build phases**: Determine the sequence of modules to implement based on dependency order
4. **Assess scope**: Full implementation, partial feature, or single component?
5. **Confirm understanding**: Summarize the implementation scope back to the user and request confirmation before proceeding — this is the ONLY mandatory user checkpoint.

If the specification is ambiguous, ask clarifying questions. Prefer a single batch of
questions over multiple rounds.

### Phase 1: Implementation → bob-the-builder

**Delegate to**: `bob-the-builder` skill
**Input provided**: Design specification, tech stack selection, module dependency order
**Expected output**: Production-ready code for all specified modules
**Gatekeeper cycle**: Submit to `gatekeeper-build` → approve/revise → resubmit if needed

#### Delegation Template

```markdown
## BUILD-MANAGEMENT DELEGATION: Phase 1 — Implementation

### Design Specification
[Paste the approved design spec or link to deliverables]

### Tech Stack
- Backend: [selected stack]
- Frontend: [selected stack]
- Database: [selected]
- Infrastructure: [selected]

### Implementation Scope
[Ordered list of modules to implement with dependency graph]

### Constraints
- [Timeline, performance, compatibility constraints]

### Instruction
Execute the bob-the-builder skill workflow. Produce complete, production-ready
code for all specified modules. No placeholders, no TODO stubs. Submit to
build-management for gatekeeper-build review when complete.
```

### Phase 2: Testing → test-builder

**Delegate to**: `test-builder` skill
**Input provided**: Gatekeeper-build-approved production code + original design specification
**Expected output**: Comprehensive test suite (unit, integration, E2E)
**Gatekeeper cycle**: Submit to `gatekeeper-build` → approve/revise

### Phase 3: Security Audit → security-builder

**Delegate to**: `security-builder` skill
**Input provided**: Approved code + approved tests + original design specification
**Expected output**: Security audit report with remediation items
**Gatekeeper cycle**: Submit audit to `gatekeeper-build` → approve/revise; if the
approved audit contains remediation items, route them to `bob-the-builder`, then
submit the updated code back to `gatekeeper-build` for remediation re-validation
before Phase 4 begins

### Phase 4: Completeness Scan → cross-check-build-confirm

**Delegate to**: `cross-check-build-confirm` skill
**Input provided**: All approved code, tests, and security-cleared codebase
**Expected output**: Completeness scan report (CLEAN or FINDINGS)
**Gatekeeper cycle**: A `CLEAN` report is necessary but not sufficient — submit the
Phase 4 report to `gatekeeper-build` and require a final `APPROVED` verdict before
delivery
**Loop**: If FINDINGS, delegate remediation to `bob-the-builder` → re-scan (max 2 cycles)

---

## Gatekeeper Management Protocol

For each phase:

1. Receive the deliverable from the specialist skill
2. Submit to `gatekeeper-build` for adversarial review
3. If **APPROVED** on Phases 1-3 with no queued remediation: Proceed to the next phase
4. If **APPROVED** on Phase 3 with remediation items: Route the fixes to
   `bob-the-builder`, then submit the updated code back to `gatekeeper-build`
   before Phase 4 begins
5. If **APPROVED** on Phase 4: Proceed to final consolidation and delivery
6. If **REVISE**: Forward gatekeeper-build's findings back to the originating skill
   with instructions to address mandatory fixes; resubmit to `gatekeeper-build`
7. If Phase 4 returns **FINDINGS** before gatekeeper review: Route those findings to
   `bob-the-builder`, re-run `cross-check-build-confirm`, then submit any resulting
   `CLEAN` report to `gatekeeper-build`
8. If **ESCALATE**: Stop the pipeline, surface the blocking issue, and consult the user
9. Maximum revision cycles per phase: **3** (Phase 4 scan cycles remain capped at **2**;
   if still failing, escalate to user with findings)

Consult `references/workflow-protocol.md` for detailed state management.

---

## Final Consolidation and Delivery

After all four phases are gatekeeper-build-approved, including the final Phase 4
approval of the completeness report:

### Step 1: Compile Build Package

Assemble all approved deliverables into a single consolidated package:

```markdown
# BUILD PACKAGE: [Project Name]

## Package Contents
1. Production Code (all modules, gatekeeper-approved)
2. Test Suite (unit, integration, E2E — gatekeeper-approved)
3. Security Audit Report (gatekeeper-approved; all findings resolved)
4. Completeness Certification (CLEAN verdict + final gatekeeper approval)
5. Gatekeeper-Build Review Reports (all approval records)

## Implementation Summary
- Modules implemented: [count]
- Test coverage: [percentage]
- Security findings resolved: [count]
- Completeness scan: CLEAN
- Final Phase 4 gate: APPROVED

## Next Actions
[Prioritized list of deployment steps, environment setup, or remaining manual tasks]
```

### Step 2: Cross-Deliverable Consistency Check

Before final handover, verify consistency across all deliverables:
- Code implements all specified requirements
- Tests cover all implemented code paths
- Security remediations are reflected in final code
- No regressions introduced during remediation cycles

### Step 3: Deliver to User

Present the complete build package with:
- Table of contents linking to each deliverable
- Executive summary (one paragraph)
- Build statistics (modules, tests, security findings)
- Recommended deployment steps

---

## Canonical Workflow Guarantees

### Mandatory Phase Enforcement

In the canonical pipeline, all four specialist phases are mandatory:

- Phase 1: `bob-the-builder`
- Phase 2: `test-builder`
- Phase 3: `security-builder`
- Phase 4: `cross-check-build-confirm`

If a specialist phase, remediation loop, or `gatekeeper-build` review cannot run,
build-management MUST escalate and stop rather than skipping the phase or
self-approving the output.

### Proactive Driving

Build-management MUST proactively:
- Make reasonable implementation assumptions when information is non-critical (document them)
- Select implementation patterns based on the tech stack if not specified
- Resolve minor ambiguities without user consultation
- Push each phase forward without waiting for user prompting
- Track all phase outputs and revision cycles for traceability

---

## Additional Resources

### Reference Files

For detailed orchestration logic and templates:
- **`references/workflow-protocol.md`** — Detailed state management, transition rules, revision cycle tracking, error handling, and escalation procedures
- **`references/handoff-templates.md`** — Structured delegation templates for each specialist skill, gatekeeper submission format, and final delivery template
