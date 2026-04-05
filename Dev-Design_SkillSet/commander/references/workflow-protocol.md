# Commander Workflow Protocol

## State Machine

The commander operates as a state machine with the following states:

```
INTAKE → PHASE_1_ACTIVE → PHASE_1_REVIEW → PHASE_2_ACTIVE → PHASE_2_REVIEW →
PHASE_3_ACTIVE → PHASE_3_REVIEW → PHASE_4_ACTIVE → PHASE_4_REVIEW →
PHASE_5_ACTIVE → PHASE_5_REVIEW → CONSOLIDATION → DELIVERED
```

### State Transitions

| From State | Event | To State | Action |
|-----------|-------|----------|--------|
| INTAKE | User confirms scope | PHASE_1_ACTIVE | Delegate to researcher |
| PHASE_N_ACTIVE | Skill completes | PHASE_N_REVIEW | Submit to gatekeeper-design |
| PHASE_N_REVIEW | Gatekeeper-design APPROVED | PHASE_N+1_ACTIVE | Delegate to next skill |
| PHASE_N_REVIEW | Gatekeeper-design REVISE | PHASE_N_ACTIVE | Return to skill with fixes |
| PHASE_N_REVIEW | Gatekeeper-design ESCALATE | INTAKE | Re-scope with user |
| PHASE_N_REVIEW | 3 revisions failed | ESCALATE_TO_USER | Show findings, request guidance |
| PHASE_5_REVIEW | Gatekeeper-design APPROVED | CONSOLIDATION | Compile final package |
| CONSOLIDATION | Consistency check passed | DELIVERED | Deliver to user |

---

## Error Handling

### Revision Cycle Management

Each phase has a maximum of 3 revision attempts:

```
Attempt 1:
  Specialist produces deliverable
  → Gatekeeper-design reviews
  → If REVISE: proceed to attempt 2

Attempt 2:
  Specialist addresses MANDATORY FIXES from gatekeeper-design
  → Gatekeeper-design re-reviews (only sections in RE-REVIEW SCOPE)
  → If REVISE: proceed to attempt 3

Attempt 3:
  Specialist addresses remaining fixes
  → Gatekeeper-design re-reviews
  → If REVISE: ESCALATE to commander → commander escalates to user
```

### Escalation to User

When escalating to the user, provide:
1. Which phase failed gatekeeper-design review
2. Summary of gatekeeper-design's findings across all attempts
3. What the gatekeeper-design requires for approval
4. Commander's recommendation for resolution
5. Specific questions for the user to resolve

### Phase Skip Handling

When a phase is skipped:
1. Document why the phase was skipped with rationale
2. Mark the phase as SKIPPED in the state tracker
3. Ensure downstream phases are aware of the gap
4. Note any assumptions that would change if the skipped phase were added later

---

## Context Handoff Rules

When delegating to a specialist skill, the commander MUST provide:

1. **Original user request** — verbatim, unmodified
2. **Phase context** — which phase this is, what came before
3. **Upstream deliverables** — all gatekeeper-design-approved documents from prior phases
4. **Constraints** — any constraints discovered during intake
5. **Specific instructions** — any phase-specific guidance based on commander's understanding

When receiving output from a specialist skill:
1. Do NOT modify the specialist's output
2. Submit to gatekeeper-design exactly as received
3. Forward gatekeeper-design findings exactly as received back to specialist
4. Only add commander context when forwarding rejection findings

---

## Quality Assurance Rules

### Commander's Self-Check Before Delivery

Before presenting the final package to the user, verify:

1. **All required phases completed** (or explicitly skipped with rationale)
2. **All gatekeeper-design approvals recorded** (one per phase)
3. **Cross-deliverable consistency**:
   - Requirements entities match architecture data model
   - Architecture API contracts match implementation routes
   - Frontend state management aligns with API design
   - Security requirements have implementation controls
   - NFR thresholds have observability coverage
4. **No orphaned references** — every document cross-references are valid
5. **Tech stack consistency** — same versions and tools across all deliverables

### Conflict Resolution

If cross-deliverable consistency check reveals conflicts:
1. Identify which deliverable is likely correct (later phases may have iterated)
2. Document the conflict
3. Recommend resolution
4. If critical: re-submit affected deliverable to gatekeeper-design with fix
5. If minor: note in the final package for implementation team awareness
