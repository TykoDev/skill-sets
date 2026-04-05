---
name: gatekeeper-code
description: >
  This skill should be used when the user asks to "validate review reports",
  "challenge findings", "verify review accuracy", "cross-check reports",
  "gate the review", "quality-check the review output", "run the gatekeeper-code",
  "verify the bug report", "challenge the security findings", or
  "ensure review correctness". It is the adversarial meta-reviewer that
  challenges reports from bug-review, code-review, quality-review, and
  security-review skills. It checks for false positives, missed findings,
  inflated severity, unsupported claims, and contradictions between reports.
  Reports are only forwarded to the user after this skill marks them as validated.
---

# Adversarial Report Validator (Gatekeeper)

## Purpose

This skill is the adversarial meta-reviewer for the review skill suite. It receives completed reports from all four specialized review skills (bug-review, code-review, quality-review, security-review) and systematically challenges every claim. It does NOT perform original code review — it validates the quality, accuracy, and completeness of others' reviews.

The gatekeeper is incentivized to find flaws in reports:
- **False positives** that waste developer time on non-issues
- **Missed findings** that allow real bugs or vulnerabilities to escape
- **Inflated severity** that causes alarm fatigue and erodes trust
- **Deflated severity** that hides genuinely dangerous issues
- **Unsupported claims** lacking verifiable evidence
- **Contradictions** between skill reports examining the same code

A report only reaches the user after the gatekeeper marks it as validated. This ensures every finding presented to the user is truthful, accurately classified, and backed by evidence.

## Challenge Protocol

Apply five challenge categories to every report received. Process each report individually before cross-validating across reports.

### 1. Existence Challenge

Verify that cited evidence actually exists. For every finding:
- Confirm the referenced file path and line number contain the described code
- Verify the described code path actually executes (is it reachable from production entry points?)
- Check that function names, variable names, and API calls mentioned in the finding match the actual code
- For claims about behavior ("this function returns null when..."), verify the stated conditions produce the stated result

**Challenge rate:** Verify 100% of Critical/Major findings. Sample 30% of Minor findings.

### 2. Accuracy Challenge

Verify that classifications are correct. For each finding:
- If a bug is labeled "concurrency hazard," confirm shared mutable state is actually accessed from multiple execution contexts
- If a security finding maps to a CWE ID (e.g., CWE-79 XSS), confirm the code actually matches the weakness pattern described by that CWE
- If a CVSS score is assigned, verify the score components (attack vector, complexity, impact) match the actual exploit scenario
- If a quality finding claims "architecture drift," verify the architectural constraint being violated is documented (in an ADR, C4 model, or codebase convention)

**Challenge rate:** Verify 100% of Critical findings. Sample 50% of Major findings.

### 3. Completeness Challenge

Verify that the skill applied its full checklist and did not skip categories without justification:
- Compare the report's "Checklist Coverage" section against the skill's documented checklist (in its `references/checklist.md` or equivalent)
- If bug-review's checklist has 8 categories but the report only addressed 5, challenge the gaps — were the missing categories not applicable to this code change, or were they overlooked?
- If security-review did not evaluate AI-specific threats for code that interacts with AI components, challenge the omission
- If code-review did not assess a dimension (e.g., Documentation) without justification, flag it

### 4. Proportionality Challenge

Verify that severity assignments match actual impact:
- A missing null check in a logging utility is Minor, not Major
- A SQL injection in a user-facing endpoint is Critical, not Medium
- A style inconsistency is not a bug — flag reports that inflate cosmetic issues
- A race condition in a single-threaded application is not a concurrency hazard
- Cross-reference severity against the skill's own rubric definition

Flag both **inflation** (finding classified higher than warranted, creating noise) and **deflation** (finding classified lower than warranted, hiding risk).

### 5. Consistency Challenge

Verify coherence across multiple skill reports (applied during cross-validation phase):
- If code-review approved the design but quality-review flagged architecture drift in the same module, one of them is wrong — or the findings need reconciliation
- If security-review found no input validation issues but bug-review flagged boundary condition bugs on the same inputs, there may be a gap
- If two skills found the same issue, verify they assigned consistent severity

Consult `references/challenge-protocol.md` for the complete rubric with examples, resolution criteria, and edge case guidance for each challenge type.

## Delegation Mechanism

When a challenge identifies an issue, construct a delegation request and send it back to the originating skill for resolution.

### Delegation Request Format

```
DELEGATION REQUEST
Target Skill:    [bug-review | code-review | quality-review | security-review]
Finding ID:      [ID from original report, or "GAP-" prefix for missing findings]
Challenge Type:  [existence | accuracy | completeness | proportionality | consistency]
Specific Question: [Precise, answerable question the skill must address]
Evidence Required: [What the skill must provide to resolve the challenge]
Round:           [1 | 2]
```

### Delegation Response Format

The originating skill must respond with one of three resolutions:

```
DELEGATION RESPONSE
Finding ID:      [matching ID]
Resolution:      [corrected | defended | withdrawn]
Evidence:        [specific evidence addressing the challenge]
Amended Finding: [if corrected, the revised finding with updated classification]
```

### Round Limits and Escalation

- **Maximum 2 delegation rounds per finding.** This prevents infinite back-and-forth loops.
- **Round 1:** Initial challenge. The skill provides evidence or corrects the finding.
- **Round 2:** If the Round 1 response is unconvincing, issue a follow-up challenge with a specific rebuttal explaining why the evidence is insufficient.
- **After Round 2:** If still unresolved, mark the finding as **"Disputed"** with both the skill's position and the gatekeeper's position documented. Present both positions to the user for final judgment.

**Batch delegation:** Group related challenges targeting the same skill into a single delegation request to reduce round-trip overhead.

Consult `references/delegation-workflow.md` for detailed examples, batch strategies, and escalation procedures.

## Cross-Validation Protocol

After validating each report individually, perform cross-skill consistency analysis.

**Partial Report Suites.** If fewer than four skills produced reports (e.g., only bug-review and security-review were invoked), exclude missing skills from the matrix columns and note "Not in scope" in the gap analysis for those skills. Do not challenge a skill that was never invoked.

**Build the Cross-Validation Matrix.** Map every changed file and function against findings from all invoked skills:

```
| File/Function     | bug-review    | code-review   | quality-review | security-review | Status      |
|-------------------|---------------|---------------|----------------|-----------------|-------------|
| auth/login.py:42  | BUG-003 (Maj) | —             | —              | SEC-001 (Crit)  | Check overlap|
| utils/parse.py    | —             | CR-Design (Nit)| QR-Drift (Maj)| —               | Contradiction?|
| api/handler.py    | —             | —             | —              | —               | Gap?        |
```

**Detect Overlaps.** When multiple skills flag the same code area, verify severity consistency. An input validation issue found by both bug-review (as a boundary condition bug) and security-review (as an injection vulnerability) may warrant the higher severity — but both should agree on the core finding.

**Detect Gaps.** Changed code areas with no findings from any skill may be genuinely clean — or may represent review blind spots. Challenge the skill most relevant to that code area.

**Detect Contradictions.** When findings from different skills conflict, escalate the contradiction. The gatekeeper does not unilaterally resolve disagreements — it documents both positions and requests reconciliation.

Consult `references/cross-validation.md` for the matrix template, gap analysis methodology, and contradiction resolution procedures.

## Completion Criteria

### Report Suite Marked "Ready" When ALL Are True:

- Every Critical and Major finding has verified, specific evidence (file paths, line numbers, code snippets, reproducible conditions)
- No unresolved existence or accuracy challenges remain on Critical or Major findings
- All skills documented their checklist coverage with justifications for skipped categories
- Severity assignments are defensible (no more than 1 disputed proportionality challenge on Critical findings)
- No unresolved contradictions between skill reports
- Cross-validation matrix shows no unexplained gaps in coverage of changed code

### Report Suite Marked "Ready-with-Disputes" When:

- All "Ready" criteria are met except for findings in Disputed status
- Disputed items are fully documented with both skill and gatekeeper positions
- The user must resolve disputes before acting on the report
- No more than 2 Critical findings are in Disputed status

### Report Suite Marked "Not-Ready" When ANY Are True:

- A Critical finding lacks verifiable evidence after 2 delegation rounds
- Two or more Critical/Major findings have unresolved accuracy challenges
- A skill skipped checklist categories without justification and has not responded to delegation
- Multiple contradictions between skills remain unresolved
- The cross-validation matrix reveals significant uncovered code areas without explanation
- More than 2 Critical findings are in Disputed status

## Output Format

Structure the gatekeeper validation report as follows:

```
## Gatekeeper Validation Report
### Overall Verdict: Ready | Not-Ready

### Per-Skill Validation
#### bug-review
- Status: Validated | Validated-with-amendments | Requires-rework
- Challenges Issued: [count by type]
- Challenges Resolved: [count]
- Amendments Applied: [list]

#### code-review
- [same structure]

#### quality-review
- [same structure]

#### security-review
- [same structure]

### Cross-Validation Findings
- Overlaps: [findings flagged by multiple skills with severity reconciliation]
- Gaps: [code areas with no coverage, with justification or challenge]
- Contradictions: [conflicting findings with resolution or dispute status]

### Disputed Items
- [Finding ID]: [skill position] vs [gatekeeper position] — User judgment required

### Confidence Assessment
- Review coverage: [percentage of changed code covered by at least one skill]
- Challenge acceptance rate: [challenges upheld vs overturned — calibration indicator]
```

## Anti-Gaming Safeguards

The gatekeeper must also guard against its own biases to remain a useful quality gate rather than a bottleneck.

**Avoid challenge-for-challenge's-sake.** Do not nitpick valid findings just to demonstrate rigor. A finding with clear evidence, correct classification, and proportionate severity should be accepted without challenge. The goal is accuracy, not adversarial performance.

**Focus on high-impact challenges.** Prioritize challenges where being wrong causes real harm: a false positive that triggers unnecessary rework on a critical production path, or a missed vulnerability that could be exploited. Minor severity disputes on Minor findings are not worth delegation rounds.

**Track calibration metrics.** Monitor the challenge acceptance rate (how often challenged findings are actually corrected vs defended). If nearly all challenges are overturned, the gatekeeper is being too aggressive. If none are overturned, it is not adding value. A healthy acceptance rate indicates the gatekeeper is finding real issues.

**Respect evidence.** When a skill provides specific, verifiable evidence defending a finding, accept it. Do not engage in rhetorical escalation — evaluate evidence on its merits.

## Additional Resources

### Reference Files

For detailed challenge rubrics, delegation formats, and cross-validation procedures, consult:

- **`references/challenge-protocol.md`** — Complete adversarial challenge rubric with detailed definitions, trigger conditions, example challenges, expected response formats, and resolution criteria for each of the 5 challenge types (existence, accuracy, completeness, proportionality, consistency)
- **`references/delegation-workflow.md`** — Detailed delegation request and response formats with worked examples, batch delegation strategies, escalation procedures, resolution tracking ledger, and round limit enforcement
- **`references/cross-validation.md`** — Cross-skill validation matrix template, overlap detection with severity reconciliation, gap analysis methodology, contradiction resolution process, coverage scoring, and final report assembly procedures
