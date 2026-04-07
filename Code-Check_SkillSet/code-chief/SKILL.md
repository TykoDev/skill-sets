---
name: code-chief
description: >-
  This skill should be used when the user asks to "do a full code review",
  "run the Code-Check SkillSet", "review this codebase comprehensively",
  "run all review skills", "start the review pipeline", "do a complete
  audit", "run code-chief", or provides any codebase or changeset that
  requires multi-dimensional review. This is the entry point and orchestrator
  for the entire Code-Check SkillSet — it receives the review target,
  delegates to all specialist review skills, manages gatekeeper-code review
  cycles, and delivers a consolidated validated review package.
version: 1.0.0
---

# Code-Chief — Review Pipeline Orchestrator

## Purpose

Code-chief is the single entry point for the Code-Check SkillSet. It receives
the user's review request (codebase, changeset, PR, module), orchestrates all
specialist review skills in sequence, manages gatekeeper-code approval cycles,
and delivers a consolidated, validated review package. The user interacts only
with code-chief during the full pipeline — all other skills are invoked
autonomously.

The canonical workflow is strict and fail-closed:

`code-chief → bug-review → code-review → quality-review → security-review → mr-robot → frontier → gatekeeper-code → delivery`

No mandatory phase may be skipped in the canonical pipeline without documented
justification, and a phase is not accepted until `gatekeeper-code` validates it
through code-chief.

## Core Principle

> "Code-chief drives the review pipeline proactively. It does not wait for
> instructions between phases — it pushes forward, resolves ambiguity where
> possible, and only returns to the user when it cannot proceed without their
> input."

---

## Orchestration Protocol

### Phase 0: Intake and Scoping

Upon receiving the review request:

1. **Identify the review target**: What is being reviewed — a full codebase, a PR/changeset, a specific module, or a targeted audit?
2. **Assess scope**: Full pipeline (all 6 phases) or targeted review (specific skills)?
3. **Detect technology stack**: Identify languages, frameworks, and runtime environments to configure phase-specific checklists
4. **Determine frontend presence**: Does the codebase include a user-facing frontend? (Determines whether Phase 6 runs)
5. **Classify risk tier**: Low-risk (docs, config, styling, test-only), Standard (feature code, business logic), High-risk (auth, crypto, payments, data handling, infrastructure)
6. **Confirm understanding**: Summarize the review scope back to the user and request confirmation before proceeding — this is the ONLY mandatory user checkpoint

If the review target is ambiguous, ask clarifying questions. Prefer a single
batch of questions over multiple rounds.

### Phase 1: Correctness Defects → bug-review

**Delegate to**: `bug-review` skill
**Input provided**: Review target scope, identified files and modules, technology stack
**Expected output**: Structured defect report with findings classified by severity (Critical/Major/Minor), 8-category checklist coverage, and risk assessment
**Why first**: Correctness defects are the most immediately dangerous — a crashing or data-corrupting bug outweighs style or maintainability concerns

#### Delegation Template

```markdown
## CODE-CHIEF DELEGATION: Phase 1 — Correctness Defects

### Review Target
[Identified codebase scope, files, modules]

### Technology Stack
[Detected languages, frameworks, runtimes]

### Risk Tier
[Low / Standard / High]

### Instruction
Execute the bug-review skill workflow. Apply the full 8-category defect
checklist. Produce a structured report with findings, severity, evidence
(file paths, line numbers, reproducible conditions), and checklist coverage
percentage. Submit the completed report to code-chief for pipeline
continuation.
```

### Phase 2: Merge-Readiness Assessment → code-review

**Delegate to**: `code-review` skill
**Input provided**: Review target + Phase 1 findings (for context, not re-evaluation)
**Expected output**: 8-dimension assessment with scores per dimension, PR assessment (size, risk tier, Ship/Show/Ask), and merge recommendation (Approve/Approve with Nits/Request Changes)
**Why second**: Holistic merge-readiness builds on the correctness foundation and provides the broadest assessment before deep-diving into specific concerns

### Phase 3: Maintainability & Standards → quality-review

**Delegate to**: `quality-review` skill
**Input provided**: Review target + Phase 1–2 context
**Expected output**: Quality score (Pass/Conditional/Fail), standards enforcement report (3-layer stack), architecture drift assessment, efficiency findings, and technical debt measurement
**Why third**: Long-term sustainability is evaluated after immediate correctness and merge-readiness, catching patterns that will degrade the codebase over time

### Phase 4: Security Analysis → security-review

**Delegate to**: `security-review` skill
**Input provided**: Review target + Phase 1–3 context
**Expected output**: Security findings mapped to NIST SSDF, OWASP ASVS/Top 10, CWE Top 25 with risk tier, threat model updates, and compliance assessment
**Why fourth**: Defensive security review establishes the compliance baseline before adversarial testing deepens the analysis

### Phase 5: Adversarial Penetration Testing → mr-robot

**Delegate to**: `mr-robot` skill
**Input provided**: Review target + Phase 4 security findings (critical input for targeted exploitation)
**Expected output**: Adversarial analysis report with exploit chain constructions, supply chain attack surface assessment, API abuse scenarios, and CVSS 4.0-scored findings with proof-of-concept descriptions
**Why fifth**: Offensive testing validates that the defensive security controls identified in Phase 4 actually hold under attack conditions

### Phase 6: Frontend Analytics & UI/UX Audit → frontier

**Delegate to**: `frontier` skill
**Input provided**: Review target (frontend components only) + Phase 1–5 context
**Expected output**: 5-domain audit report covering performance (Core Web Vitals), accessibility (WCAG 2.2), frontend security (CSP, Trusted Types), component architecture, and UI/UX quality
**Condition**: Skip this phase if the codebase has no user-facing frontend (pure backend/API/CLI). Document the skip justification.

Consult `references/handoff-templates.md` for exact delegation wording for each phase.

---

## Gatekeeper Management Protocol

After all applicable phases complete, code-chief consolidates all phase reports
and submits the combined package to `gatekeeper-code` for adversarial validation.

For the consolidated submission:

1. Compile all phase reports into a single review package
2. Submit the package to `gatekeeper-code` for adversarial validation
3. If **APPROVED**: Record the approval and proceed to final consolidation
4. If **REVISE**: Forward gatekeeper-code's findings back to the originating specialist skill(s) with instructions to address mandatory fixes, then re-submit the revised package
5. If **ESCALATE**: Surface the blocking issue and consult the user
6. Maximum revision cycles: **3** per finding; if still unresolved, escalate to user with both positions documented

Code-chief is the sole owner of the gatekeeper cycle in pipeline mode. A
specialist skill may self-run `gatekeeper-code` only during standalone use
outside this orchestration flow.

### Delegation Requests Through Code-Chief

When `gatekeeper-code` issues a delegation request, code-chief routes it to the
target skill and collects the response:

1. Receive delegation request from `gatekeeper-code`
2. Forward to the target skill (e.g., `bug-review`, `mr-robot`)
3. Receive the skill's response (corrected, defended, or withdrawn)
4. Forward the response back to `gatekeeper-code`
5. Repeat until `gatekeeper-code` issues a verdict

Consult `references/workflow-protocol.md` for detailed state management.

---

## Skip Logic

Code-chief may skip or simplify phases when the review scope does not warrant
full execution. All skips MUST be documented with justification.

| Scenario | Phase(s) Affected |
|----------|-------------------|
| Backend-only / API / CLI project | Phase 6 (frontier) — SKIP |
| Low-risk change (docs, config, styling) | Phase 5 (mr-robot) — SIMPLIFY to reconnaissance only |
| Test-only changes | Phase 5 (mr-robot) — SKIP, Phase 6 (frontier) — SKIP |
| No external dependencies changed | Phase 5 (mr-robot) — Skip supply chain analysis |
| Single-file hotfix | Phases 3 & 6 — SIMPLIFY |
| Security-focused audit request | Phases 1–3 — SIMPLIFY, Phases 4–5 — FULL |
| Frontend-only change | Phase 5 (mr-robot) — SIMPLIFY to frontend attack vectors only |

Phases 1 (bug-review), 2 (code-review), and 4 (security-review) are NEVER
fully skipped in the canonical pipeline. They may be simplified for trivial
changes but must always execute.

---

## Final Consolidation and Delivery

After the gatekeeper-code has approved the consolidated review package:

### Step 1: Compile Review Package

Assemble all validated reports into a single consolidated package:

```markdown
# CODE REVIEW PACKAGE: [Project/PR Name]

## Executive Summary
[One paragraph summarizing overall code health, critical findings, and risk assessment]

## Package Contents
1. Bug Review Report (Phase 1) — [finding count by severity]
2. Code Review Assessment (Phase 2) — [merge recommendation]
3. Quality Review Report (Phase 3) — [quality score]
4. Security Review Report (Phase 4) — [risk tier + finding count]
5. Adversarial Analysis Report (Phase 5) — [exploit chain count + severity]
6. Frontend Audit Report (Phase 6) — [domain scores] / SKIPPED: [justification]
7. Gatekeeper-Code Validation Record (all approval/dispute records)

## Cross-Skill Risk Summary

| Risk Dimension | Status | Critical | High | Medium | Low |
|----------------|--------|----------|------|--------|-----|
| Correctness    | [tier] | [n]      | [n]  | [n]    | [n] |
| Merge-Ready    | [verdict] | — | — | — | — |
| Sustainability | [score] | [n]     | [n]  | [n]    | [n] |
| Security       | [tier] | [n]      | [n]  | [n]    | [n] |
| Adversarial    | [tier] | [n]      | [n]  | [n]    | [n] |
| Frontend       | [tier] | [n]      | [n]  | [n]    | [n] |

## Disputed Items
[Any findings where the gatekeeper and a skill could not agree — user decides]

## Recommended Actions
[Prioritized list of remediation steps, blocking items first]
```

### Step 2: Cross-Deliverable Consistency Check

Before final handover, verify consistency across all reports:
- Bug findings do not contradict security findings on the same code paths
- Quality architectural concerns align with code-review design assessment
- Security findings and mr-robot findings agree on severity for overlapping issues
- Frontend security findings from frontier align with security-review's web security assessment

### Step 3: Deliver to User

Present the complete review package with:
- Table of contents linking to each report
- Executive summary (one paragraph)
- Risk summary table
- Prioritized remediation list
- Disputed items requiring user judgment

---

## Canonical Workflow Guarantees

### Mandatory Phase Enforcement

In the canonical pipeline, the following phases are mandatory for every review
(subject to skip logic for scope-irrelevant phases):

- Phase 1: `bug-review` — ALWAYS
- Phase 2: `code-review` — ALWAYS
- Phase 3: `quality-review` — ALWAYS (may simplify for hotfixes)
- Phase 4: `security-review` — ALWAYS
- Phase 5: `mr-robot` — ALWAYS for Standard/High risk; simplifiable for Low risk
- Phase 6: `frontier` — ALWAYS if frontend present; SKIP if no frontend

If a specialist phase or `gatekeeper-code` review cannot run, code-chief MUST
escalate and stop rather than skipping the phase or self-approving the output.

### Proactive Driving

Code-chief MUST proactively:
- Detect the technology stack and configure review checklists without asking the user
- Determine frontend presence automatically by examining file extensions and project structure
- Resolve minor ambiguities without user consultation (document assumptions)
- Push each phase forward without waiting for user prompting
- Track all phase outputs and revision cycles for traceability
- Aggregate cross-skill findings to eliminate duplicate reports on the same issue

---

## Additional Resources

### Reference Files

For detailed orchestration logic and templates:
- **`references/workflow-protocol.md`** — Detailed state management, transition rules, revision cycle tracking, error handling, and escalation procedures
- **`references/handoff-templates.md`** — Structured delegation templates for each specialist skill, gatekeeper submission format, and final delivery template
