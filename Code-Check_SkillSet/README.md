<p align="center">
  <img src="https://img.shields.io/badge/Code--Check-SkillSet-blueviolet?style=for-the-badge&logo=codereview&logoColor=white" alt="Code-Check SkillSet" />
  <br/>
  <em>Adversarial, multi-dimensional code review skills for AI coding agents</em>
</p>

# Code-Check SkillSet

> **Author:** [TykoDev](https://github.com/TykoDev)
> **License:** See repository for license details

A modular suite of **five interconnected AI coding skills** that provide systematic, evidence-based code review with adversarial validation. This README assumes the combined **AI SkillSets** repository is your primary install path, while still staying usable if this SkillSet is published as a separate mirror.

The canonical install rule is the same in both cases: copy the **inner skill folders** into your agent's skills directory, then start with the specific review skill that matches the task. There is no single default orchestrator for ordinary reviews. If your assistant does not auto-route installed skills, explicitly reference the relevant `<skill>/SKILL.md` file as a fallback.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Skill Descriptions](#skill-descriptions)
  - [Bug Review](#bug-review)
  - [Code Review](#code-review)
  - [Quality Review](#quality-review)
  - [Security Review](#security-review)
  - [Gatekeeper-Code](#gatekeeper-code)
- [How the Skills Differ](#how-the-skills-differ)
- [How the Skills Connect](#how-the-skills-connect)
- [Step-by-Step Usage](#step-by-step-usage)
- [Installation Guide](#installation-guide)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)

---

## Overview

The **Code-Check SkillSet** is a collection of five specialized review skills that transform an AI coding agent into a rigorous, multi-perspective code reviewer. Each skill examines code through a distinct lens, and the **Gatekeeper** skill acts as an adversarial meta-reviewer that validates all reports before they reach the user.

### Key Principles

| Principle | Description |
|---|---|
| **Separation of concerns** | Each skill owns one review dimension — no overlap, no ambiguity |
| **Evidence-based findings** | Every finding requires file paths, line numbers, and reproducible conditions |
| **Adversarial validation** | The Gatekeeper challenges every claim before reports reach the user |
| **Framework-anchored** | Findings map to CWE, OWASP, CVSS, IEEE, NIST, and other industry standards |
| **Tool-aware** | Skills recommend specific static/dynamic analysis tools per finding type |

---

## Architecture

The SkillSet follows a **hub-and-spoke architecture** where four specialized review skills produce reports and the Gatekeeper validates them:

```
                        ┌─────────────────────┐
                        │     USER REQUEST     │
                        │   "review this code" │
                        └──────────┬──────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    ▼              ▼              ▼              ▼
           ┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────┐
           │  bug-review   ││ code-review   ││quality-review││security-review│
           │              ││              ││              ││              │
           │ Correctness  ││ Holistic 8D  ││ Maintainab.  ││ Exploitab.  │
           │ defects      ││ assessment   ││ & standards  ││ & compliance│
           └──────┬───────┘└──────┬───────┘└──────┬───────┘└──────┬───────┘
                  │               │               │               │
                  │         Reports submitted      │               │
                  └───────────────┼───────────────┘               │
                                  ▼                               │
                        ┌─────────────────────┐◄──────────────────┘
                        │   GATEKEEPER-CODE    │
                        │                     │
                        │  Adversarial meta-  │
                        │  reviewer validates │
                        │  all reports        │
                        └──────────┬──────────┘
                                   │
                          ┌────────┼────────┐
                          ▼                 ▼
                  ┌──────────────┐  ┌──────────────┐
                  │   VALIDATED  │  │  NOT READY   │
                  │   Reports    │  │  (delegated  │
                  │   → User     │  │   back for   │
                  └──────────────┘  │   rework)    │
                                    └──────────────┘
```

### Data Flow

1. The user requests a review (e.g., "find bugs", "review security", "check quality").
2. The AI agent activates the appropriate skill(s) based on the request.
3. Each activated skill produces a structured report with findings, severity, and evidence.
4. All reports are submitted to the **Gatekeeper** for adversarial validation.
5. The Gatekeeper challenges findings using 5 challenge types, delegates issues back if needed.
6. Only validated reports (marked **Ready** or **Ready-with-Disputes**) are presented to the user.

---

## Skill Descriptions

### Bug Review

| | |
|---|---|
| **Skill name** | `Bug Finding Specialist` |
| **Directory** | `bug-review/` |
| **Focus** | Correctness defects, logic errors, reliability issues |
| **Methodology** | CWE Top 25 defect checklist, Google reviewer focus areas, test-driven review |

The **Bug Review** skill performs systematic bug detection focused exclusively on code that will produce **incorrect results**, **crash**, **leak resources**, **corrupt data**, or **behave unpredictably**. It is not concerned with exploitability (that's security-review) or long-term maintainability (that's quality-review).

#### What It Checks (8 Bug Class Categories)

1. **Input validation and parsing edge cases**
2. **Error handling and fail-closed behavior**
3. **Concurrency hazards and unsafe shared state**
4. **Resource management** (handles, sockets, memory, connections)
5. **Boundary conditions and integer arithmetic**
6. **Null/optional handling across API boundaries**
7. **Logging and observability correctness**
8. **Test meaningfulness and regression coverage**

#### Workflow

1. Identify change boundaries (functions, modules, data flows)
2. Apply the 8-category defect checklist
3. Trace error paths through the call chain
4. Evaluate concurrency and shared state
5. Assess test quality (assertions, boundary tests, regression value)
6. Classify findings by severity (Critical / Major / Minor)

#### Severity Rubric

| Severity | Examples |
|---|---|
| **Critical** | Data corruption, crash under normal operation, infinite loop, deadlock, silent data loss |
| **Major** | Incorrect output under reachable conditions, resource leaks, race conditions, missing error handling |
| **Minor** | Cosmetic logic issues, unreachable dead code, redundant checks, minor test improvements |

#### Tool Recommendations

Static analysis (Infer, SonarQube), sanitizers (ASan, MSan, TSan), fuzzing (libFuzzer, OSS-Fuzz), mutation testing (PIT, Stryker), property-based testing (Hypothesis, fast-check), formal verification (Dafny, TLA+).

#### Reference Files

| File | Contents |
|---|---|
| `references/checklist.md` | Complete 8-category defect checklist with language-specific patterns |
| `references/detection-techniques.md` | Deep dive on static analysis, sanitizers, fuzzing, mutation testing |
| `references/triage-workflow.md` | IEEE 1044 / IBM ODC triage, severity rubrics, defect metrics |

---

### Code Review

| | |
|---|---|
| **Skill name** | `Code Reviewing Specialist` |
| **Directory** | `code-review/` |
| **Focus** | Holistic merge-readiness assessment across 8 dimensions |
| **Methodology** | Google's 8-dimension code review framework with risk-tiered PR assessment |

The **Code Review** skill is the **broadest** of the four review skills. It evaluates every code change across **all eight dimensions simultaneously** and provides a unified **merge recommendation** (Approve / Approve with Nits / Request Changes). It is distinct because it doesn't go deep on any single dimension — instead it provides the complete picture.

#### The 8 Review Dimensions

| # | Dimension | What It Evaluates |
|---|---|---|
| 1 | **Design** | Architecture, abstractions, separation of concerns, module boundaries |
| 2 | **Functionality** | Correct behavior for all inputs, edge cases, user interaction |
| 3 | **Complexity** | Readability, over-engineering, clarity on first read |
| 4 | **Tests** | Test design, meaningfulness, coverage of changed paths |
| 5 | **Naming** | Clarity, consistency, scope-proportional length |
| 6 | **Comments** | "Why" over "what", freshness, intent documentation |
| 7 | **Style** | Project style guide adherence (not personal preference) |
| 8 | **Documentation** | READMEs, API docs, changelogs, migration guides |

#### PR Assessment Protocol

- **Size check:** Target 200–400 LOC; flag >1,000 LOC PRs for splitting
- **Risk tier:** Low / Medium / High based on change scope
- **Ship/Show/Ask:** Trivial → ship; awareness-only → show; complex → ask (blocking review)
- **Reviewer routing:** CODEOWNERS, domain expertise for high-risk changes

#### Feedback Conventions

| Prefix | Meaning |
|---|---|
| **Blocking** | Must fix before merge (correctness, security, design) |
| **Nit** | Non-blocking polish suggestion |
| **Optional** | Improvement to consider, not required |
| **Question** | Seeking understanding, not requesting changes |

#### Reference Files

| File | Contents |
|---|---|
| `references/review-dimensions.md` | Detailed guidance, common mistakes, example comments per dimension |
| `references/pr-workflow.md` | Risk-tiered PR lifecycle, Ship/Show/Ask, stacked PRs, merge queues |
| `references/feedback-guide.md` | Constructive feedback examples, severity prefixes, time-boxing |

---

### Quality Review

| | |
|---|---|
| **Skill name** | `Code Quality, Efficiency & Best Practices Specialist` |
| **Directory** | `quality-review/` |
| **Focus** | Long-term code health, maintainability, architecture, efficiency |
| **Methodology** | Clean Code principles, C4 architecture model, DORA/DX metrics, IEEE 730-2026 |

The **Quality Review** skill goes **deep on sustainability** — it evaluates whether code will remain maintainable, performant, and structurally sound over time. While code-review assesses merge readiness *today*, quality-review asks whether the codebase will still be healthy *next year*.

#### What It Covers

**Standards Enforcement (3-Layer Stack):**

| Layer | Scope | Tools |
|---|---|---|
| Layer 1 — Baseline Hygiene | Formatting, linting, type checking | Prettier, Black, Ruff, ESLint, mypy, Pyright |
| Layer 2 — Semantic Analysis | Deep code meaning beyond syntax | SonarQube, Infer, CodeQL, Semgrep |
| Layer 3 — AI-Assisted Review | Design, intent, cross-file deps | AI reviewers, change risk assessment |

**Architecture Review:**
- C4 Model validation (System Context → Containers → Components → Code)
- ADR (Architecture Decision Record) compliance
- Drift detection (boundary violations, coupling creep, interface bypass)

**Efficiency Assessment:**
- Algorithmic complexity (O(n²) where O(n) exists)
- Resource utilization (memory, connections, caching)
- Database query optimization (N+1 queries, missing indices)
- Unnecessary computation (redundant calls, missed memoization)

**Technical Debt Measurement:**
- Code duplication / DRY violations
- Cyclomatic and cognitive complexity
- Dependency staleness
- TODO/FIXME/HACK density and age
- DORA metrics and DX Core 4 Framework integration

#### Reference Files

| File | Contents |
|---|---|
| `references/standards-enforcement.md` | Language-specific style guides and tool configurations |
| `references/architecture-review.md` | C4 evaluation, ADR lifecycle, drift detection methodology |
| `references/metrics-and-debt.md` | DORA metrics, DX Core 4, IEEE 730-2026, tech debt measurement |

---

### Security Review

| | |
|---|---|
| **Skill name** | `Security Review Specialist` |
| **Directory** | `security-review/` |
| **Focus** | Exploitability, security compliance, threat mitigation |
| **Methodology** | NIST SSDF, OWASP ASVS/Top 10, CWE Top 25, STRIDE, MITRE ATT&CK |

The **Security Review** skill is exclusively concerned with **how adversaries can exploit the code**. It maps every finding to established security frameworks and weakness taxonomies.

#### Frameworks Applied

| Framework | Use |
|---|---|
| **NIST SSDF v1.1** | Secure development practices (PW.7: code review mandate) |
| **OWASP ASVS 5.0.0** | Technical control requirements (L1/L2/L3 compliance) |
| **OWASP Top 10:2025** | Most prevalent web application risks |
| **CWE Top 25 (2025)** | Weakness taxonomy for classifying findings |
| **Microsoft Continuous SDL** | AI-specific threat guidance |
| **EU AI Act / RAD-AI** | AI system compliance for high-risk contexts |

#### Threat Modeling Integration

- **STRIDE** classification: Spoofing, Tampering, Repudiation, Info Disclosure, DoS, Elevation of Privilege
- **MITRE ATT&CK** cross-reference for real-world adversary techniques
- Lightweight threat model updates triggered by new data flows, trust boundary changes, auth modifications

#### Secure Coding Checklist Areas

1. Input Validation
2. Output Encoding
3. Authentication
4. Session Management
5. Access Control
6. Error Handling
7. Cryptography
8. Logging

#### AI-Specific Threat Assessment

| Threat | Description |
|---|---|
| **Prompt Injection** | Can user inputs manipulate AI system instructions? |
| **Data/Model Poisoning** | Is training data integrity validated? |
| **Excessive Agency** | Are AI agent permissions scoped with RBAC? |
| **Shadow AI** | Are there ungoverned data flows or unauthorized AI tools? |
| **PII Leakage** | Can sensitive data leak through prompts or telemetry? |

#### Reference Files

| File | Contents |
|---|---|
| `references/frameworks.md` | NIST SSDF, OWASP ASVS/SAMM, CWE Top 25, MITRE ATT&CK mapping |
| `references/secure-coding-checklist.md` | Complete secure coding checklist with verification procedures |
| `references/supply-chain.md` | SBOM, SCA, SLSA, Sigstore, secrets detection, supply chain gates |

---

### Gatekeeper-Code

| | |
|---|---|
| **Skill name** | `Adversarial Report Validator` |
| **Directory** | `gatekeeper-code/` |
| **Focus** | Meta-review — validates the quality of other skills' reports |
| **Methodology** | 5-type challenge protocol, delegation workflow, cross-validation matrix |

The **Gatekeeper** is fundamentally different from the four review skills. It **does not review code**. Instead, it receives completed reports from the review skills and **challenges every claim** to ensure accuracy before reports reach the user.

#### The 5 Challenge Types

| # | Challenge | What It Verifies |
|---|---|---|
| 1 | **Existence** | Does the cited evidence (file, line, code path) actually exist? |
| 2 | **Accuracy** | Are classifications correct (CWE mapping, CVSS score, category)? |
| 3 | **Completeness** | Did the skill apply its full checklist? Were skips justified? |
| 4 | **Proportionality** | Does severity match actual impact? (catches inflation AND deflation) |
| 5 | **Consistency** | Do findings across different skills agree where they overlap? |

#### Delegation Mechanism

When the Gatekeeper finds an issue, it sends a **Delegation Request** back to the originating skill:

```
DELEGATION REQUEST
Target Skill:      [bug-review | code-review | quality-review | security-review]
Finding ID:        [ID from original report]
Challenge Type:    [existence | accuracy | completeness | proportionality | consistency]
Specific Question: [Precise question to address]
Evidence Required: [What must be provided to resolve]
Round:             [1 | 2]
```

The skill responds with one of three resolutions: **corrected**, **defended**, or **withdrawn**.

- **Maximum 2 rounds** per finding to prevent infinite loops
- After Round 2, unresolved findings are marked **"Disputed"** with both positions documented
- The user makes the final judgment on disputed items

#### Cross-Validation Matrix

The Gatekeeper builds a matrix mapping every changed file/function against findings from all invoked skills:

```
| File/Function     | bug-review    | code-review   | quality-review | security-review | Status       |
|-------------------|---------------|---------------|----------------|-----------------|--------------|
| auth/login.py:42  | BUG-003 (Maj) | —             | —              | SEC-001 (Crit)  | Check overlap|
| utils/parse.py    | —             | CR-Design     | QR-Drift (Maj) | —               | Contradiction|
| api/handler.py    | —             | —             | —              | —               | Gap?         |
```

This detects **overlaps** (same issue, different severity), **gaps** (code with no coverage), and **contradictions** (conflicting findings).

#### Verdicts

| Verdict | Condition |
|---|---|
| **Ready** | All challenges resolved, evidence verified, no contradictions |
| **Ready-with-Disputes** | Met all criteria except disputed items (≤2 Critical) |
| **Not-Ready** | Critical findings lack evidence, unresolved contradictions, missing coverage |

#### Reference Files

| File | Contents |
|---|---|
| `references/challenge-protocol.md` | Complete challenge rubric with examples and resolution criteria |
| `references/delegation-workflow.md` | Delegation formats, batch strategies, escalation procedures |
| `references/cross-validation.md` | Matrix template, gap analysis, contradiction resolution |

---

## How the Skills Differ

The four review skills are designed to be **complementary, not overlapping**. Each owns a distinct concern:

| Aspect | bug-review | code-review | quality-review | security-review |
|---|---|---|---|---|
| **Core question** | "Will this code break?" | "Is this code ready to merge?" | "Will this code stay healthy?" | "Can this code be exploited?" |
| **Focus** | Correctness defects | Holistic assessment | Long-term sustainability | Adversarial exploitation |
| **Depth** | Deep on defects | Broad across 8 dimensions | Deep on architecture & metrics | Deep on security frameworks |
| **Time horizon** | Now (will it crash today?) | Now (should we merge today?) | Future (maintainable next year?) | Now & future (attack surface) |
| **Severity model** | Critical / Major / Minor | Blocking / Nit / Optional / Question | Pass / Conditional / Fail | Critical / High / Medium / Low (CVSS) |
| **Standards** | CWE Top 25, IEEE 1044, IBM ODC | Google's 8 dimensions | Clean Code, C4, DORA, IEEE 730 | NIST SSDF, OWASP, CWE, STRIDE |
| **Output verdict** | Risk assessment (High/Med/Low) | Approve / Approve with Nits / Request Changes | Quality Score (Pass/Conditional/Fail) | Risk Tier + Finding severity |
| **Tool recs** | Sanitizers, fuzzing, mutation testing | Linters, CI gates | SonarQube, Ruff, CodeQL, Semgrep | CodeQL, Semgrep, SCA, SBOM |

### When to Use Which

| Scenario | Skill(s) to invoke |
|---|---|
| "Find bugs in this code" | `bug-review` |
| "Review this PR" | `code-review` (optionally + others for thorough review) |
| "Check code quality and standards" | `quality-review` |
| "Review for security vulnerabilities" | `security-review` |
| "Do a full review" | All four → `gatekeeper-code` |
| "Validate these review results" | `gatekeeper-code` |

---

## How the Skills Connect

```
┌──────────────────────────────────────────────────────────────┐
│                    REVIEW PIPELINE                           │
│                                                              │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐│
│  │ bug-review │ │code-review │ │quality-rev.│ │security-rev││
│  │            │ │            │ │            │ │            ││
│  │ Report +   │ │ Report +   │ │ Report +   │ │ Report +   ││
│  │ Checklist  │ │ 8D scores  │ │ Drift score│ │ CWE/CVSS   ││
│  │ Coverage   │ │ + Verdict  │ │ + Metrics  │ │ + Threat   ││
│  └─────┬──────┘ └─────┬──────┘ └─────┬──────┘ └─────┬──────┘│
│        │              │              │              │        │
│        └──────────────┼──────────────┼──────────────┘        │
│                       ▼              ▼                       │
│              ┌─────────────────────────────┐                 │
│              │     GATEKEEPER-CODE         │                 │
│              │                             │                 │
│              │  1. Validate each report    │                 │
│              │  2. Cross-validate matrix   │                 │
│              │  3. Delegate challenges     │                 │
│              │  4. Resolve or dispute      │                 │
│              │  5. Issue verdict           │                 │
│              └──────────┬──────────────────┘                 │
│                         │                                    │
│              ┌──────────▼──────────┐                         │
│              │  VALIDATED REPORTS   │                         │
│              │  → Presented to user │                         │
│              └─────────────────────┘                         │
└──────────────────────────────────────────────────────────────┘
```

**Connection points:**

1. **Handoff:** Each review skill explicitly hands its report to the Gatekeeper after completion.
2. **Delegation:** The Gatekeeper sends challenges *back* to the originating skill for correction or defense.
3. **Cross-validation:** The Gatekeeper compares findings across skills to detect overlaps, gaps, and contradictions.
4. **Fallback:** If the Gatekeeper is not available, each skill performs a self-review pass using a subset of the Gatekeeper's challenge protocol.

---

## Step-by-Step Usage

> **Activation note:** Start with the review skill that matches the job, such as `bug-review`, `code-review`, `quality-review`, or `security-review`. Use `gatekeeper-code` when you need to validate review reports. In assistants that do not auto-route installed skills, reference the relevant `<skill>/SKILL.md` file explicitly as a fallback.

### Running a Single Review Skill

1. **Install the inner skill folders** in your configured skills directory.
2. **Open your AI coding agent** (Codex, Claude Code, Copilot, Kilo Code, or OpenCode).
3. **Start with the review skill that matches the task**:
   - `"Run bug-review on this codebase"` → activates **bug-review**
   - `"Review this PR with code-review"` → activates **code-review**
   - `"Use quality-review to assess maintainability"` → activates **quality-review**
   - `"Use security-review to check this service for vulnerabilities"` → activates **security-review**
4. **If your assistant needs manual context, fall back to the skill file directly**:
   - `"Find bugs in this code using the bug-review/SKILL.md instructions"` → activates **bug-review**
   - `"Review this PR for merge readiness using the code-review/SKILL.md instructions"` → activates **code-review**
   - `"Check code quality using quality-review/SKILL.md"` → activates **quality-review**
   - `"Review this code for security vulnerabilities using security-review/SKILL.md"` → activates **security-review**
5. **Receive the structured report** with findings, severity, evidence, and recommendations.

### Running a Full Review Pipeline (Agentic Frameworks Only)

If your environment natively supports multi-agent workflows and the `.skills` directory structure:
1. **Request a comprehensive review:**
   ```
   "Do a full code review using all installed review skills"
   ```
2. The system activates relevant review skills.
3. The **Gatekeeper** receives all reports and performs adversarial validation.
4. You receive the validated reports with any disputed items clearly marked.

### Running the Gatekeeper Standalone

If you already have review results and want to validate them:
```
"Run the gatekeeper-code using gatekeeper-code/SKILL.md to validate these review findings"
```

### Trigger Phrases Quick Reference

| Skill | Example Prompting phrases |
|---|---|
| **bug-review** | "find bugs using bug-review", "check for edge cases" |
| **code-review** | "review this code using code-review", "check this PR" |
| **quality-review** | "check coding standards using quality-review", "assess maintainability" |
| **security-review** | "review security using security-review", "check for vulnerabilities" |
| **gatekeeper-code** | "validate these review reports using the gatekeeper-code instructions" |

---

## Installation Guide

Each skill relies on a `SKILL.md` file (the main instructions) and a `references/` directory (detailed checklists). To install them, copy the **inner skill folders** from `Code-Check_SkillSet/` into the skills directory your agent is configured to read.

This README assumes you are working from the combined AI SkillSets repository. If you are using a separately published mirror of this SkillSet, the same install rule applies; only the surrounding repo path changes.

### Canonical Install Rule

Copy these folders, not the top-level `Code-Check_SkillSet/` directory:

- `bug-review/`
- `code-review/`
- `quality-review/`
- `security-review/`
- `gatekeeper-code/`

Use `.agents/skills/` when your environment supports a shared skills directory, or the configured equivalent for your assistant.

---

### Standard Assistants (Claude Code and GitHub Copilot)

These assistants do **not** always auto-route to installed skill files. Keep the same copied skill folders in an accessible location, then point the assistant at the right review skill or fallback `SKILL.md` file.

#### Claude Code
Claude Code does not use a guaranteed native skills directory, so the important part is keeping the copied folders somewhere your project instructions can reference consistently.

1. Copy the installed Code-Check skill folders into a location your workspace can reference, such as `.agents/skills/` or `.claude/skills/`.
2. Add a reference in your **`CLAUDE.md`** (project root):
   ```markdown
   ## Code Review Skills
   This project uses the Code-Check SkillSet. Start with the review skill that matches the task. If Claude does not auto-route the installed skill, read `.agents/skills/[skill-name]/SKILL.md` first.
   ```

#### GitHub Copilot
GitHub Copilot does not guarantee automatic routing from a custom skills folder.

1. Copy the installed Code-Check skill folders into a location your repository instructions can reference.
2. Reference them in **`.github/copilot-instructions.md`**:
   ```markdown
   ## Custom Code Review
   When asked to review code, start with the matching review skill. If Copilot does not auto-route the installed skill, use `@workspace` and follow `.agents/skills/[skill-name]/SKILL.md`.
   ```

---

### Agentic Frameworks (Codex, Kilo Code, OpenCode)

If you are using a dedicated agentic runner that natively recognizes a `skills` directory structure:

1. Create the environment's skills directory, typically `.agents/skills/` or the framework-specific equivalent.
2. Copy the individual skill folders (`bug-review`, `code-review`, and the rest) directly into that directory.
3. Prompt the review skill that matches the task.
4. Use `gatekeeper-code` only when you want report validation.
5. If the framework does not auto-route as expected, fall back to `<skill>/SKILL.md`.

---

### Verification

After installing in any agent, verify the skills are detected by asking:

```
"What skills do you have available?"
```

or directly trigger one:

```
"Find bugs in the auth module"
```

If the agent responds with a structured report following the skill's output format, the installation was successful.

---

## Directory Structure

```
Code-Check_SkillSet/
├── README.md                          # This file
├── QUICK-START.md                     # Brief onboarding guide
├── bug-review/
│   ├── SKILL.md                       # Bug Finding Specialist instructions
│   └── references/
│       ├── checklist.md               # 8-category defect checklist
│       ├── detection-techniques.md    # Static analysis, fuzzing, sanitizers
│       └── triage-workflow.md         # IEEE 1044 / IBM ODC triage process
├── code-review/
│   ├── SKILL.md                       # Code Reviewing Specialist instructions
│   └── references/
│       ├── review-dimensions.md       # Google's 8 dimensions detailed guidance
│       ├── pr-workflow.md             # Risk-tiered PR lifecycle
│       └── feedback-guide.md          # Constructive feedback conventions
├── quality-review/
│   ├── SKILL.md                       # Quality & Best Practices Specialist
│   └── references/
│       ├── standards-enforcement.md   # Language-specific tool configs
│       ├── architecture-review.md     # C4 model, ADRs, drift detection
│       └── metrics-and-debt.md        # DORA, DX Core 4, IEEE 730-2026
├── security-review/
│   ├── SKILL.md                       # Security Review Specialist instructions
│   └── references/
│       ├── frameworks.md              # NIST, OWASP, CWE, MITRE mappings
│       ├── secure-coding-checklist.md # Complete secure coding checklist
│       └── supply-chain.md            # SBOM, SCA, SLSA, secrets detection
└── gatekeeper-code/
    ├── SKILL.md                       # Adversarial Report Validator instructions
    └── references/
        ├── challenge-protocol.md      # 5-type challenge rubric
        ├── delegation-workflow.md     # Delegation formats and escalation
        └── cross-validation.md        # Cross-skill validation matrix
```

---

## Contributing

Contributions are welcome! If you'd like to improve the skills, add new reference material, or fix issues:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improve-bug-checklist`)
3. Make your changes
4. Submit a pull request

For questions or suggestions, open an issue or discussion in the repository where you found this SkillSet.

---

<p align="center">
  <sub>Built by <a href="https://github.com/TykoDev">TykoDev</a> · Code-Check SkillSet</sub>
</p>
