<p align="center">
  <img src="https://img.shields.io/badge/Code--Check-SkillSet-blueviolet?style=for-the-badge&logo=codereview&logoColor=white" alt="Code-Check SkillSet" />
  <br/>
  <em>Orchestrated, adversarial, multi-dimensional code review pipeline for AI coding agents</em>
</p>

# Code-Check SkillSet

> **Author:** [TykoDev](https://github.com/TykoDev)
> **License:** See repository for license details

A modular suite of **eight interconnected AI coding skills** orchestrated by the **code-chief** pipeline manager. The SkillSet provides systematic, evidence-based code review across correctness, design, quality, security, adversarial exploitation, and frontend analytics — all validated through adversarial meta-review before reaching the user. This README assumes the combined **AI SkillSets** repository is your primary install path, while still staying usable if this SkillSet is published as a separate mirror.

The canonical install rule is the same in both cases: copy the **inner skill folders** into your agent's skills directory, then start with the `code-chief` entry point. If your assistant does not auto-route installed skills, explicitly reference the relevant `<skill>/SKILL.md` file as a fallback.

**code-chief** is the canonical entry point for comprehensive reviews. It orchestrates all six specialist review skills, manages the gatekeeper-code validation cycle, and delivers a consolidated review package. Individual skills remain usable standalone for targeted reviews.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Skill Descriptions](#skill-descriptions)
  - [code-chief — Orchestrator](#code-chief)
  - [bug-review — Correctness Defects](#bug-review)
  - [code-review — Merge-Readiness Assessment](#code-review)
  - [quality-review — Maintainability & Standards](#quality-review)
  - [security-review — Exploitability & Compliance](#security-review)
  - [mr-robot — Adversarial Penetration Testing](#mr-robot)
  - [frontier — Frontend Analytics & UI/UX Audit](#frontier)
  - [gatekeeper-code — Adversarial Meta-Reviewer](#gatekeeper-code)
- [How the Skills Differ](#how-the-skills-differ)
- [How the Skills Connect](#how-the-skills-connect)
- [Step-by-Step Usage](#step-by-step-usage)
- [Installation Guide](#installation-guide)
- [Directory Structure](#directory-structure)

---

## Overview

The **Code-Check SkillSet** is a fully orchestrated pipeline of eight specialized skills that transform an AI coding agent into a rigorous, multi-perspective code reviewer. The **code-chief** orchestrator drives the pipeline proactively, delegating to six specialist review skills in sequence, then submitting all reports to the **gatekeeper-code** for adversarial validation. No report reaches the user until it passes the adversarial meta-review.

### Key Principles

| Principle | Description |
|---|---|
| **Orchestrated pipeline** | code-chief drives the full review lifecycle autonomously — no manual prompting between phases |
| **Separation of concerns** | Each skill owns one review dimension — no overlap, no ambiguity |
| **Evidence-based findings** | Every finding requires file paths, line numbers, and reproducible conditions |
| **Adversarial validation** | The Gatekeeper challenges every claim before reports reach the user |
| **Framework-anchored** | Findings map to CWE, OWASP, CVSS, NIST SSDF, WCAG, and other standards |
| **Offensive + Defensive** | security-review provides defensive analysis; mr-robot validates it offensively |
| **Tool-aware** | Skills recommend specific static/dynamic analysis tools per finding type |
| **Machine-readable output** | All reports include structured pipeline summary blocks for automated processing |

---

## Architecture

The SkillSet follows an **orchestrator-driven pipeline architecture** where code-chief delegates to six specialist review skills, collects their reports, and submits the consolidated package to gatekeeper-code for adversarial validation:

```
                        ┌─────────────────────┐
                        │     USER REQUEST     │
                        │ "run a full review"  │
                        └──────────┬──────────┘
                                   │
                        ┌──────────▼──────────┐
                        │     CODE-CHIEF       │
                        │   Pipeline           │
                        │   Orchestrator       │
                        └──────────┬──────────┘
                                   │
          ┌─────────┬─────────┬────┼────┬─────────┬──────────┐
          ▼         ▼         ▼         ▼         ▼          ▼
   ┌───────────┐┌──────────┐┌──────────┐┌──────────┐┌────────┐┌─────────┐
   │bug-review ││code-     ││quality-  ││security- ││mr-robot││frontier │
   │           ││review    ││review    ││review    ││        ││         │
   │Correctness││Holistic  ││Maintain. ││Exploit.  ││Offens. ││Frontend │
   │defects    ││8D review ││standards ││compliance││pen-test││UI/UX    │
   └─────┬─────┘└────┬─────┘└────┬─────┘└────┬─────┘└───┬────┘└────┬────┘
         │           │           │           │          │           │
         └───────────┴───────────┴─────┬─────┴──────────┴───────────┘
                                       │
                                       ▼
                        ┌─────────────────────┐
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
                  │   Package    │  │  (delegated  │
                  │   → User     │  │   back via   │
                  └──────────────┘  │  code-chief) │
                                    └──────────────┘
```

### Canonical Pipeline

```
code-chief → bug-review → code-review → quality-review → security-review
           → mr-robot → frontier → gatekeeper-code → [remediation loop] → delivery
```

### Data Flow

1. The user requests a comprehensive review (e.g., "run a full code review").
2. **code-chief** scopes the review, detects the technology stack, and identifies skip-eligible phases.
3. code-chief delegates to each specialist skill in sequence (Phase 1–6).
4. Each skill produces a structured report with findings, severity, and evidence.
5. code-chief consolidates all reports and submits them to **gatekeeper-code** for adversarial validation.
6. The gatekeeper challenges findings using 5 challenge types and routes delegation requests back through code-chief.
7. Only validated packages (marked **Ready** or **Ready-with-Disputes**) are delivered to the user.

---

## Skill Descriptions

### code-chief

**Role:** Pipeline Orchestrator — the single entry point for comprehensive reviews
**Invoked by:** Direct user request ("run a full code review", "review this codebase comprehensively")

| What It Does | What It Does NOT Do |
|---|---|
| Receives review requests and scopes the pipeline | Perform any code review itself |
| Delegates to all 6 specialist skills in sequence | Skip mandatory phases without justification |
| Manages gatekeeper-code approval cycles | Self-approve reports |
| Consolidates validated reports into a final package | Overrule gatekeeper-code verdicts |
| Proactively pushes phases forward | Wait for user prompting between phases |

**Key Files:**
- `code-chief/SKILL.md` — Orchestration protocol, skip logic, delivery format
- `code-chief/references/workflow-protocol.md` — State machine, revision tracking, error handling
- `code-chief/references/handoff-templates.md` — Phase delegation templates, gatekeeper submission format

---

### bug-review

**Role:** Correctness Defect Specialist — Phase 1 in the pipeline
**Focus:** Logic errors, crashes, data corruption, resource leaks, boundary conditions, concurrency hazards

| What It Does | What It Does NOT Do |
|---|---|
| Applies an 8-category defect checklist systematically | Assess exploitability (that's security-review) |
| Traces error paths through call chains | Evaluate design quality (that's code-review) |
| Evaluates test meaningfulness and regression value | Measure maintainability metrics (that's quality-review) |
| Recommends detection tools (fuzzing, mutation testing, SARIF) | Perform architecture assessment |

**Methodology:** CWE Top 25 (2025), Google's reviewer focus areas, mutation testing for test quality validation
**Key Files:** `bug-review/SKILL.md`, `references/checklist.md`, `references/detection-techniques.md`, `references/triage-workflow.md`

---

### code-review

**Role:** Holistic Merge-Readiness Specialist — Phase 2 in the pipeline
**Focus:** Google's 8-dimension framework (Design, Functionality, Complexity, Tests, Naming, Comments, Style, Documentation)

| What It Does | What It Does NOT Do |
|---|---|
| Evaluates all 8 review dimensions per change | Deep-dive into security exploits |
| Assesses PR size, risk tier, and Ship/Show/Ask classification | Measure technical debt quantitatively |
| Detects AI-generated code patterns for heightened scrutiny | Perform penetration testing |
| Provides merge recommendation (Approve / Approve with Nits / Request Changes) | Run automated tests or linters |

**Methodology:** Google Code Review Practices, risk-tiered PR assessment, AI-generated code detection
**Key Files:** `code-review/SKILL.md`, `references/review-dimensions.md`, `references/pr-workflow.md`, `references/feedback-guide.md`

---

### quality-review

**Role:** Maintainability & Standards Specialist — Phase 3 in the pipeline
**Focus:** Clean code, architecture drift, standards enforcement, efficiency, technical debt

| What It Does | What It Does NOT Do |
|---|---|
| Enforces the 3-layer automation stack (linters + semantic + AI) | Hunt for correctness bugs |
| Detects architecture drift against C4 models and ADRs | Assess security vulnerabilities |
| Measures technical debt (CodeClimate, SonarQube ratio, ISO 5055) | Evaluate UI/UX quality |
| Reviews algorithmic efficiency and resource utilization | Provide merge verdicts |

**Methodology:** Clean Code + "Clean as You Code", C4 architecture validation, ISO/IEC 5055, DORA + DX Core 4 metrics
**Key Files:** `quality-review/SKILL.md`, `references/standards-enforcement.md`, `references/architecture-review.md`, `references/metrics-and-debt.md`

---

### security-review

**Role:** Defensive Security Specialist — Phase 4 in the pipeline
**Focus:** Exploitability, security requirements compliance, threat modeling, supply chain security

| What It Does | What It Does NOT Do |
|---|---|
| Maps findings to NIST SSDF, OWASP ASVS/Top 10, CWE Top 25 | Construct exploit chains (that's mr-robot) |
| Applies STRIDE threat modeling on every trust boundary change | Audit frontend-specific security (that's frontier) |
| Evaluates supply chain security with reachability analysis | Perform UI/UX accessibility audits |
| Assesses AI/LLM-specific threats (prompt injection, agency) | Deep-dive component architecture |
| Cross-references OWASP API Security Top 10 for API codebases | |

**Methodology:** NIST SSDF v1.1, OWASP ASVS 5.0.0, OWASP Top 10:2025, CWE Top 25, Microsoft SDL, EU AI Act/RAD-AI
**Key Files:** `security-review/SKILL.md`, `references/frameworks.md`, `references/secure-coding-checklist.md`, `references/supply-chain.md`

---

### mr-robot

**Role:** Adversarial Penetration Testing Specialist — Phase 5 in the pipeline
**Focus:** Offensive exploitation analysis with proof-of-concept exploit chain construction

| What It Does | What It Does NOT Do |
|---|---|
| Maps attack surfaces (entry points, trust boundaries, data flows) | Perform defensive compliance audits (that's security-review) |
| Constructs multi-step exploit chains with preconditions and impact | Review code style or design |
| Simulates supply chain attacks (dependency confusion, typosquatting) | Measure maintainability or tech debt |
| Tests API abuse patterns (BOLA, BFLA, JWT attacks) | Audit frontend accessibility |
| Scores findings with CVSS 4.0 based on proven exploitability | Report theoretical vulnerabilities without exploitation evidence |
| Performs AI/LLM threat assessment (prompt injection PoC, jailbreak testing) | |

**Methodology:** OWASP Testing Guide v4.2, PTES, MITRE ATT&CK, CVSS 4.0 scoring
**Key Files:** `mr-robot/SKILL.md`, `references/attack-methodology.md`, `references/exploit-patterns.md`, `references/severity-scoring.md`

---

### frontier

**Role:** Frontend Analytics & UI/UX Audit Specialist — Phase 6 in the pipeline
**Focus:** Performance (Core Web Vitals), accessibility (WCAG 2.2), frontend security, component architecture, UI/UX quality
**Condition:** Skipped if the codebase has no user-facing frontend

| What It Does | What It Does NOT Do |
|---|---|
| Audits Core Web Vitals (LCP ≤2.5s, INP ≤200ms, CLS ≤0.1) | Review backend logic |
| Performs WCAG 2.2 Level AA accessibility compliance audit | Perform server-side security testing |
| Evaluates CSP, Trusted Types, SRI, and DOM-based XSS prevention | Hunt for backend bugs |
| Assesses component architecture (state management, render efficiency) | Evaluate API design |
| Reviews UI/UX quality (responsive design, interaction states, animations) | Compile merge verdicts |

**Methodology:** Core Web Vitals 2025 thresholds, WCAG 2.2, CSP Level 3, Trusted Types API
**Key Files:** `frontier/SKILL.md`, `references/performance-checklist.md`, `references/accessibility-checklist.md`, `references/frontend-security.md`

---

### gatekeeper-code

**Role:** Adversarial Meta-Reviewer — validates all reports before user delivery
**Focus:** Finding false positives, missed findings, inflated/deflated severity, unsupported claims, cross-skill contradictions

| What It Does | What It Does NOT Do |
|---|---|
| Challenges every finding using 5 challenge types | Perform original code review |
| Builds cross-validation matrix across all 6 review skills | Generate new findings |
| Delegates back to originating skills for resolution | Unilaterally overrule skills |
| Marks reports as Ready, Ready-with-Disputes, or Not-Ready | Skip challenges on Critical findings |
| Guards against its own biases (anti-gaming safeguards) | Create unnecessary challenge loops |

**Challenge Types:** Existence, Accuracy, Completeness, Proportionality, Consistency
**Key Files:** `gatekeeper-code/SKILL.md`, `references/challenge-protocol.md`, `references/delegation-workflow.md`, `references/cross-validation.md`

---

## How the Skills Differ

| Dimension | bug-review | code-review | quality-review | security-review | mr-robot | frontier |
|---|---|---|---|---|---|---|
| **Perspective** | Is it correct? | Is it merge-ready? | Is it sustainable? | Is it secure? | Can I exploit it? | Is the frontend good? |
| **Primary Output** | Defect list | Merge verdict | Quality score | Risk assessment | Exploit chains | 5-domain audit |
| **Key Framework** | CWE Top 25 | Google 8D | Clean Code / ISO 5055 | NIST SSDF / OWASP | OWASP Testing Guide / PTES | Core Web Vitals / WCAG 2.2 |
| **Severity Model** | Critical/Major/Minor | Blocking/Nit/Optional | Pass/Conditional/Fail | CVSS 4.0 mapped | CVSS 4.0 with PoC | Critical/Major/Minor per domain |
| **Pipeline Phase** | 1 | 2 | 3 | 4 | 5 | 6 (frontend only) |

---

## How the Skills Connect

### Pipeline Mode (via code-chief)

```
User → code-chief → [Phase 1: bug-review] → [Phase 2: code-review] → [Phase 3: quality-review]
     → [Phase 4: security-review] → [Phase 5: mr-robot] → [Phase 6: frontier*]
     → Consolidate → gatekeeper-code → [Remediation Loop] → Validated Package → User

* Phase 6 skipped for backend-only projects
```

In pipeline mode:
- code-chief is the sole owner of the gatekeeper-code validation cycle
- Specialist skills submit reports to code-chief, not directly to gatekeeper-code
- Gatekeeper delegation requests route through code-chief

### Standalone Mode (individual skills)

Each skill can be invoked independently without code-chief:
- The skill executes its full workflow
- The skill self-submits to gatekeeper-code for validation (or self-validates if gatekeeper-code is unavailable)
- Useful for targeted reviews where the full pipeline is unnecessary

---

## Step-by-Step Usage

> **Activation note:** The canonical entry point is `code-chief`. In agentic frameworks that auto-route installed skills, prompt that entry point directly. In assistants that do not auto-route skills, reference `code-chief/SKILL.md` or another specific skill file explicitly as a fallback.

Use the plain skill name in prompts by default. Some environments also expose installed skills as slash commands such as `/code-chief`.

### Full Pipeline Review (Recommended)

1. **Invoke code-chief** with your review request:
   ```
   Use code-chief to run a full review of this codebase.
   Use code-chief to do a full code review of this project.
   Use code-chief to review this PR with the full Code-Check SkillSet.
   ```

2. **Confirm scope**: code-chief will summarize the review scope and ask for confirmation — this is the only mandatory user checkpoint.

3. **Wait for pipeline completion**: code-chief autonomously runs all 6 phases, submits to gatekeeper-code, and handles any remediation loops.

4. **Receive the validated review package**: A consolidated report with findings from all phases, cross-skill risk summary, and prioritized remediation list.

### Targeted Review (Individual Skills)

For focused reviews, invoke the specific skill directly:

```
Use bug-review to inspect this file for correctness defects.
Use security-review to check this service for vulnerabilities.
Use mr-robot to penetration test this API.
Use frontier to audit the frontend accessibility.
```

---

## Installation Guide

Each skill relies on a `SKILL.md` file (the main instructions) and a `references/` directory (detailed review frameworks and checklists). To install them, copy the **inner skill folders** from `Code-Check_SkillSet/` into the skills directory your agent is configured to read.

This README assumes you are working from the combined AI SkillSets repository. If you are using a separately published mirror of this SkillSet, the same install rule applies; only the surrounding repo path changes.

### Canonical Install Rule

Copy these folders, not the top-level `Code-Check_SkillSet/` directory:

- `code-chief/`
- `bug-review/`
- `code-review/`
- `quality-review/`
- `security-review/`
- `mr-robot/`
- `frontier/`
- `gatekeeper-code/`

Use `.agents/skills/` when your environment supports a shared skills directory, or the configured equivalent for your assistant.

---

### Standard Assistants (Claude Code and GitHub Copilot)

These assistants do **not** always auto-route to installed skill files. Keep the same copied skill folders in an accessible location, then point the assistant at the right entry point or fallback `SKILL.md` file.

#### Claude Code
Claude Code does not use a guaranteed native skills directory, so the important part is keeping the copied folders somewhere your project instructions can reference consistently.

1. Copy the installed Code-Check skill folders into a location your workspace can reference, such as `.agents/skills/` or `.claude/skills/`.
2. Add a reference in your **`CLAUDE.md`** (project root):
   ```markdown
   ## Code Review Skills
   This project uses the Code-Check SkillSet. Start comprehensive reviews with the code-chief entry point. If Claude does not auto-route the installed skill, read `.agents/skills/code-chief/SKILL.md` first.
   ```

#### GitHub Copilot
GitHub Copilot does not guarantee automatic routing from a custom skills folder.

1. Copy the installed Code-Check skill folders into a location your repository instructions can reference.
2. Reference them in **`.github/copilot-instructions.md`**:
   ```markdown
   ## Code Review
   When asked to review code comprehensively, start with code-chief. If Copilot does not auto-route the installed skill, use `@workspace` and follow `.agents/skills/code-chief/SKILL.md`.
   ```

---

### Agentic Frameworks (Codex, Kilo Code, OpenCode)

If you are using a dedicated agentic runner that natively recognizes a `skills` directory structure:

1. Create the environment's skills directory, typically `.agents/skills/` or the framework-specific equivalent.
2. Copy the individual skill folders (`code-chief`, `bug-review`, and the rest) directly into that directory.
3. Prompt the `code-chief` entry point directly.
4. If the framework does not auto-route as expected, fall back to `code-chief/SKILL.md`.

---

### Verification

After installing in any agent, verify the skills are detected by asking:

```
"What skills do you have available?"
```

or directly trigger the pipeline:

```
"Use code-chief to run a full code review of this project."
```

If the agent responds with a phased review flow following code-chief's orchestration protocol, the installation was successful.

---

## Directory Structure

```
Code-Check_SkillSet/
├── README.md                          ← This file
├── QUICK-START.md                     ← Concise quick-start guide
│
├── code-chief/                        ← Pipeline Orchestrator
│   ├── SKILL.md                       ← Orchestration protocol
│   └── references/
│       ├── workflow-protocol.md       ← State machine and error handling
│       └── handoff-templates.md       ← Delegation templates per phase
│
├── bug-review/                        ← Phase 1: Correctness Defects
│   ├── SKILL.md                       ← Defect detection workflow
│   └── references/
│       ├── checklist.md               ← 8-category defect checklist
│       ├── detection-techniques.md    ← Tool recommendations
│       └── triage-workflow.md         ← Severity classification
│
├── code-review/                       ← Phase 2: Merge-Readiness
│   ├── SKILL.md                       ← 8-dimension review framework
│   └── references/
│       ├── review-dimensions.md       ← Detailed dimension guidance
│       ├── pr-workflow.md             ← PR lifecycle and assessment
│       └── feedback-guide.md          ← Feedback conventions
│
├── quality-review/                    ← Phase 3: Maintainability
│   ├── SKILL.md                       ← Quality assessment workflow
│   └── references/
│       ├── standards-enforcement.md   ← Tool configurations
│       ├── architecture-review.md     ← C4 and ADR evaluation
│       └── metrics-and-debt.md        ← DORA, ISO 5055, tech debt
│
├── security-review/                   ← Phase 4: Security
│   ├── SKILL.md                       ← Security analysis workflow
│   └── references/
│       ├── frameworks.md              ← NIST SSDF, OWASP, CWE
│       ├── secure-coding-checklist.md ← Comprehensive checklist
│       └── supply-chain.md            ← SBOM, SCA, SLSA
│
├── mr-robot/                          ← Phase 5: Adversarial Testing
│   ├── SKILL.md                       ← Penetration testing workflow
│   └── references/
│       ├── attack-methodology.md      ← OWASP Testing Guide, PTES, ATT&CK
│       ├── exploit-patterns.md        ← Language-specific exploits
│       └── severity-scoring.md        ← CVSS 4.0 scoring guide
│
├── frontier/                          ← Phase 6: Frontend Audit
│   ├── SKILL.md                       ← 5-domain frontend audit
│   └── references/
│       ├── performance-checklist.md   ← Core Web Vitals audit
│       ├── accessibility-checklist.md ← WCAG 2.2 Level AA
│       └── frontend-security.md       ← CSP, Trusted Types, SRI
│
└── gatekeeper-code/                   ← Adversarial Meta-Reviewer
    ├── SKILL.md                       ← Challenge protocol
    └── references/
        ├── challenge-protocol.md      ← 5-type challenge rubric
        ├── delegation-workflow.md     ← Delegation request/response
        └── cross-validation.md        ← Cross-skill matrix
```
