# Build Pipeline Workflow Protocol

## State Machine Definition

Build-management operates as a deterministic state machine. Each state has defined entry conditions, actions, and exit transitions. No state may be skipped without explicit documentation.

### State Diagram

```
INTAKE ──► PHASE_1_BUILD ──► PHASE_1_GATE ──┐
                                              │
           ┌──────────────────────────────────┘
           │
           ├─[APPROVED]──► PHASE_2_TEST ──► PHASE_2_GATE ──┐
           │                                                 │
           └─[REVISE]───► PHASE_1_BUILD (retry)             │
                                              ┌──────────────┘
                                              │
           ┌──────────────────────────────────┘
           │
           ├─[APPROVED]──► PHASE_3_SECURITY ──► PHASE_3_GATE ──┐
           │                                                     │
           └─[REVISE]───► PHASE_2_TEST (retry)                  │
                                              ┌──────────────────┘
                                              │
           ┌──────────────────────────────────┘
           │
           ├─[APPROVED]──► PHASE_4_COMPLETENESS ──► PHASE_4_GATE ──┐
           │                                                         │
           └─[REVISE]───► PHASE_1_BUILD (remediation)               │
                                              ┌──────────────────────┘
                                              │
           ┌──────────────────────────────────┘
           │
           ├─[CLEAN]────► CONSOLIDATION ──► DELIVERED
           │
           └─[FINDINGS]─► PHASE_1_BUILD (remediation) ──► re-scan
```

### State Transition Table

| Current State | Event | Next State | Action |
|---------------|-------|------------|--------|
| INTAKE | Spec validated, user confirmed | PHASE_1_BUILD | Delegate to bob-the-builder |
| PHASE_1_BUILD | Code complete | PHASE_1_GATE | Submit to gatekeeper-build |
| PHASE_1_GATE | APPROVED | PHASE_2_TEST | Delegate to test-builder |
| PHASE_1_GATE | REVISE (attempt < 3) | PHASE_1_BUILD | Return findings to bob-the-builder |
| PHASE_1_GATE | REVISE (attempt = 3) | ESCALATE | Present findings to user |
| PHASE_1_GATE | ESCALATE | INTAKE | Re-scope with user |
| PHASE_2_TEST | Tests complete | PHASE_2_GATE | Submit to gatekeeper-build |
| PHASE_2_GATE | APPROVED | PHASE_3_SECURITY | Delegate to security-builder |
| PHASE_2_GATE | REVISE (attempt < 3) | PHASE_2_TEST | Return findings to test-builder |
| PHASE_2_GATE | REVISE (attempt = 3) | ESCALATE | Present findings to user |
| PHASE_3_SECURITY | Audit complete | PHASE_3_GATE | Submit to gatekeeper-build; queue remediation |
| PHASE_3_GATE | APPROVED, no remediation | PHASE_4_COMPLETENESS | Delegate to cross-check |
| PHASE_3_GATE | APPROVED, has remediation | PHASE_1_BUILD | Route remediation to bob-the-builder |
| PHASE_3_GATE | REVISE (attempt < 3) | PHASE_3_SECURITY | Return findings to security-builder |
| PHASE_4_COMPLETENESS | CLEAN | CONSOLIDATION | Compile delivery package |
| PHASE_4_COMPLETENESS | FINDINGS (cycle < 2) | PHASE_1_BUILD | Delegate to bob-the-builder |
| PHASE_4_COMPLETENESS | FINDINGS (cycle = 2) | ESCALATE | Present findings to user |
| CONSOLIDATION | Package assembled | DELIVERED | Deliver to user |

---

## Revision Cycle Management

### Per-Phase Revision Tracking

Maintain a revision counter for each phase:

```
Phase 1 (bob-the-builder):          attempt 0/3
Phase 2 (test-builder):             attempt 0/3
Phase 3 (security-builder):         attempt 0/3
Phase 4 (cross-check-build-confirm): cycle 0/2
```

### Revision Rules

1. **Increment on REVISE**: Each REVISE verdict increments the phase counter
2. **Reset on phase change**: Counters reset only when moving to a NEW phase (not when looping)
3. **Escalation threshold**: Counter reaches maximum → escalate to user with full findings history
4. **User override**: User may authorize additional revision cycles beyond the maximum

### Escalation Format

When escalating to the user:

```markdown
## ESCALATION: Phase [N] — [Phase Name]

### Summary
[Phase] has failed gatekeeper-build review [N] times.

### Revision History
| Attempt | Key Findings | Resolution Attempted | Outcome |
|---------|-------------|---------------------|---------|
| 1 | [summary] | [what was changed] | REVISE |
| 2 | [summary] | [what was changed] | REVISE |
| 3 | [summary] | [what was changed] | REVISE |

### Remaining Unresolved Findings
[List of findings that persist across all attempts]

### Recommended Action
[Suggest re-scoping, clarifying requirements, or alternative approach]
```

---

## Phase Skip Handling

### Skip Conditions

A phase may be skipped when its deliverable already exists or is not applicable. Document every skip.

| Phase | Skip Condition | Documentation Required |
|-------|---------------|----------------------|
| Phase 2 (test-builder) | Comprehensive tests already exist and pass | Test coverage report, test run output |
| Phase 3 (security-builder) | External security audit completed recently | Audit report reference, date, scope |
| Phase 4 (cross-check) | Codebase is a minor patch (< 20 LOC changed) | Change manifest showing scope |

### Skip Documentation Format

```markdown
## PHASE SKIP NOTICE

### Phase Skipped: [Phase N — Name]
### Reason: [Detailed justification]
### Evidence: [Test reports, audit references, or scope analysis]
### Risk Assessment: [What risks are accepted by skipping]
### Approved By: [User confirmation if required]
```

---

## Context Handoff Rules

### What to Pass to Each Specialist

Each specialist receives only what it needs — not the entire accumulated history.

| Specialist | Required Context | Optional Context |
|------------|-----------------|------------------|
| **bob-the-builder** | Design spec, tech stack, module scope, constraints | Prior gatekeeper findings (for remediation) |
| **test-builder** | Approved code, design spec (for requirements), tech stack | Architecture decisions relevant to test design |
| **security-builder** | Complete codebase (code + tests), tech stack, deployment model | Threat model from design phase |
| **cross-check-build-confirm** | Complete codebase, original design spec (for completeness comparison) | Build manifest listing all expected modules |

### Context Filtering

Build-management MUST prevent context bloat by:
- Passing only approved deliverables (not revision history) to downstream skills
- Summarizing gatekeeper reports rather than forwarding full text when only key findings are relevant
- Excluding skipped phase documentation from downstream handoffs
- Forwarding gatekeeper findings exactly as-is when delegating remediation (no modification)

---

## Quality Assurance Rules

### Pre-Delivery Self-Check

Before marking DELIVERED, build-management must verify:

1. **Phase completion**: All non-skipped phases reached APPROVED status
2. **Remediation closure**: All security remediation items are addressed and re-validated
3. **Completeness certification**: cross-check-build-confirm issued a CLEAN verdict
4. **Consistency**: No contradictions between phase outputs (e.g., security findings that were not actually fixed)
5. **Traceability**: Every requirement from the design spec maps to implemented code

### Audit Trail

Maintain a running log of all phase transitions:

```markdown
## Build Audit Trail

| Timestamp | State | Action | Outcome |
|-----------|-------|--------|---------|
| [time] | INTAKE | Validated design spec | Confirmed with user |
| [time] | PHASE_1_BUILD | Delegated to bob-the-builder | Code delivered |
| [time] | PHASE_1_GATE | Submitted to gatekeeper-build | APPROVED |
| [time] | PHASE_2_TEST | Delegated to test-builder | Tests delivered |
```

---

## Error Recovery

### Specialist Failure

If a specialist skill fails to produce output (timeout, error, or incomplete delivery):

1. **Retry once**: Re-delegate with clarified instructions
2. **If retry fails**: Escalate to user with the failure details
3. **Never skip a failed phase**: A failed phase cannot be treated as skipped

### Gatekeeper Unavailability

If gatekeeper-build is unavailable:

1. Build-management performs a self-review using the gatekeeper's review dimensions (spec alignment, code quality, security, testing, documentation, completeness, correctness)
2. Document the self-review explicitly as a "Self-Gate" with reduced confidence
3. Recommend the user run a manual gatekeeper review before deploying

### Conflict Between Phases

When a later phase's findings require changes that invalidate an earlier phase's approval:

1. Route the changes to bob-the-builder for implementation
2. Re-submit the affected code to gatekeeper-build for re-validation
3. Do NOT re-run phases that were not affected by the change
