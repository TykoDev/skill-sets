<p align="center">
  <img src="https://img.shields.io/badge/Build--Team-SkillSet-blueviolet?style=for-the-badge&logo=hammer&logoColor=white" alt="Build-Team SkillSet" />
  <br/>
  <em>Autonomous multi-agent code implementation pipeline for AI coding agents</em>
</p>

# Build-Team SkillSet

> **Author:** [TykoDev](https://github.com/TykoDev)
> **License:** See repository for license details

A modular suite of **six specialized AI skills** that orchestrate a fully autonomous code implementation pipeline — from approved design specifications to production-ready, security-audited, completeness-verified code. This README assumes the combined **AI SkillSets** repository is your primary install path, while still staying usable if this SkillSet is published as a separate mirror.

The canonical install rule is the same in both cases: copy the **inner skill folders** into your agent's skills directory, then start with the `build-management` entry point. If your assistant does not auto-route installed skills, explicitly reference the relevant `<skill>/SKILL.md` file as a fallback.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Skill Descriptions](#skill-descriptions)
  - [Build Management](#build-management)
  - [Bob the Builder](#bob-the-builder)
  - [Test Builder](#test-builder)
  - [Security Builder](#security-builder)
  - [Gatekeeper Build](#gatekeeper-build)
  - [Cross-Check Build Confirm](#cross-check-build-confirm)
- [How the Skills Differ](#how-the-skills-differ)
- [How the Skills Connect](#how-the-skills-connect)
- [Step-by-Step Usage](#step-by-step-usage)
- [Installation Guide](#installation-guide)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)

---

## Overview

The **Build-Team SkillSet** transforms an AI coding agent into a complete software implementation team. It receives approved design specifications (from the Dev-Design SkillSet or any implementation plan) and autonomously produces production-ready code through a four-phase pipeline: implementation, testing, security audit, and completeness verification. Every phase output passes through an adversarial gatekeeper before the pipeline advances.

### Key Principles

| Principle | Description |
|---|---|
| **Spec-Driven Building** | Every line of code traces back to a design requirement — no ad-hoc features |
| **Separation of Concerns** | Each skill owns one build dimension — coding, testing, security, or completeness |
| **Adversarial Validation** | The Gatekeeper challenges every phase output before the pipeline advances |
| **Framework-Anchored** | Standards map to IEEE 730, ISO/IEC 25010, OWASP Top 10, CWE Top 25, NIST SSDF, CVSS v4.0 |
| **Completeness-First** | No placeholder code, no TODOs, no mocks in production paths — verified by dedicated scanner |

---

## Architecture

The SkillSet follows an **orchestrator-led pipeline** where `build-management` drives transitions through four core phases, and `gatekeeper-build` guards every handoff:

```
                    ┌─────────────────────────┐
                    │  APPROVED DESIGN PACKAGE │
                    │  (from Dev-Design or     │
                    │   user's impl. plan)     │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
             ┌─────│    BUILD-MANAGEMENT      │◄────────────┐
             │     │    (Orchestrator)         │             │
             │     └────────────┬────────────┘             │
             │                  │                           │
             │    ┌─────────────▼──────────────┐            │
             │    │      BOB-THE-BUILDER       │◄───┐       │
             │    │   (Production Code)        │    │       │
             │    └─────────────┬──────────────┘    │       │
             │                  │                   │       │
             │    ┌─────────────▼──────────────┐    │       │
             │    │     GATEKEEPER-BUILD       │────┘       │
             │    │   (Adversarial Gate)       │  REVISE    │
             │    └─────────────┬──────────────┘            │
             │                  │ APPROVED                  │
             │    ┌─────────────▼──────────────┐            │
             │    │       TEST-BUILDER         │            │
             │    │   (Test Suite Creation)    │            │
             │    └─────────────┬──────────────┘            │
             │                  │                           │
             │    ┌─────────────▼──────────────┐            │
             │    │     GATEKEEPER-BUILD       │            │
             │    └─────────────┬──────────────┘            │
             │                  │ APPROVED                  │
             │    ┌─────────────▼──────────────┐            │
             │    │     SECURITY-BUILDER       │            │
             │    │   (Security Audit)         │──remediate─┘
             │    └─────────────┬──────────────┘
             │                  │
             │    ┌─────────────▼──────────────┐
             │    │     GATEKEEPER-BUILD       │
             │    └─────────────┬──────────────┘
             │                  │ APPROVED
             │    ┌─────────────▼──────────────┐
             │    │ CROSS-CHECK-BUILD-CONFIRM  │──findings──► bob-the-builder
             │    │   (Completeness Scan)      │             (loop until clean)
             │    └─────────────┬──────────────┘
             │                  │ CLEAN
             │    ┌─────────────▼──────────────┐
             └───►│    FINAL BUILD PACKAGE     │
                  │    → Delivered to User     │
                  └───────────────────────────┘
```

### Data Flow

1. The user provides an approved design specification or implementation plan to `build-management`.
2. `build-management` validates the input and delegates to `bob-the-builder` for Phase 1 implementation.
3. `bob-the-builder` produces production code with a structured change manifest.
4. `gatekeeper-build` adversarially reviews the implementation against the design spec.
5. If approved, `build-management` delegates to `test-builder` for Phase 2 test creation.
6. `gatekeeper-build` validates test quality and coverage.
7. If approved, `build-management` delegates to `security-builder` for Phase 3 security audit.
8. `gatekeeper-build` validates the audit's accuracy and completeness.
9. `cross-check-build-confirm` performs a final completeness scan, delegating findings back to `bob-the-builder` until the codebase is clean.
10. `build-management` consolidates all approved deliverables into a final build package and delivers to the user.

### State Machine

```
INTAKE → PHASE_1_BUILD → PHASE_1_GATE →
         PHASE_2_TEST → PHASE_2_GATE →
         PHASE_3_SECURITY → PHASE_3_GATE →
         PHASE_4_COMPLETENESS → PHASE_4_GATE →
         CONSOLIDATION → DELIVERED
```

- Maximum 3 revision cycles per phase before escalation to user
- Maximum 2 completeness scan cycles before escalation to user

---

## Skill Descriptions

### Build Management

| | |
|---|---|
| **Skill name** | `Build Pipeline Orchestrator` |
| **Directory** | `build-management/` |
| **Focus** | Pipeline oversight, state management, phase delegation, and delivery |
| **Methodology** | Phased delegation protocol, autonomous state transitions, gatekeeper-gated handoffs |

The **Build Management** skill is the central orchestrator and single entry point for the Build-Team SkillSet. It receives design packages or implementation plans, creates a phased execution plan, delegates to each specialist skill in sequence, manages gatekeeper review cycles, and delivers the final consolidated build package. It drives work proactively — advancing autonomously and only returning to the user when blocked by unresolvable ambiguity.

#### Reference Files

| File | Contents |
|---|---|
| `references/workflow-protocol.md` | Full state machine with all states, transitions, revision cycle management, and error recovery |
| `references/handoff-templates.md` | Delegation templates for all 4 phases, gatekeeper submissions, and final delivery |

---

### Bob the Builder

| | |
|---|---|
| **Skill name** | `Senior Development Engineer` |
| **Directory** | `bob-the-builder/` |
| **Focus** | Production code implementation, remediation, and change management |
| **Methodology** | IEEE 730-2026, ISO/IEC 25010, Clean Code principles, domain-first architecture |

The **Bob the Builder** skill is the primary code producer. It translates design specifications into production-ready implementations — the workhorse of the Build Team. It analyzes delegation packages, plans implementation order by dependency, implements incrementally with full error handling, applies coding standards, performs self-review, and submits structured change manifests to the gatekeeper. It also handles remediation of findings from `gatekeeper-build`, `security-builder`, and `cross-check-build-confirm`.

#### Anti-Pattern Enforcement

Every output is held to a strict anti-pattern policy: zero placeholder code, zero TODO/FIXME/HACK comments, zero fake data, zero mock implementations in production paths, zero commented-out code, and zero hardcoded secrets.

#### Reference Files

| File | Contents |
|---|---|
| `references/coding-standards.md` | Clean Code principles, naming, function design, error handling, documentation, security |
| `references/implementation-patterns.md` | Domain-first organization, repository pattern, validation, DI, migrations, API patterns |
| `references/output-protocol.md` | Structured change manifest format, incremental delivery, remediation response format |

---

### Test Builder

| | |
|---|---|
| **Skill name** | `Test Engineering Specialist` |
| **Directory** | `test-builder/` |
| **Focus** | Comprehensive test suite creation, coverage, and test quality validation |
| **Methodology** | ISO/IEC 29119, ISTQB, test pyramid/trophy, AAA pattern, property-based testing |

The **Test Builder** skill is a dedicated test engineer that creates comprehensive, meaningful test suites as production-grade artifacts. It analyzes the implemented codebase, designs a test strategy following the test pyramid or trophy model, writes unit tests with specific assertions, writes integration tests for module boundaries, and validates that every test is meaningful — capable of failing when the production logic is inverted.

#### Test Quality Criteria

Every test must have at least one specific assertion (not just "does not throw"), test observable behavior (not implementation details), follow the Arrange-Act-Assert pattern, and be fully independent and deterministic.

#### Reference Files

| File | Contents |
|---|---|
| `references/test-strategy.md` | Test pyramid/trophy models, coverage targets, TDD methodology, property-based and contract testing |
| `references/test-patterns.md` | AAA pattern, fixtures, mocking strategies, database testing, API testing, anti-patterns |

---

### Security Builder

| | |
|---|---|
| **Skill name** | `Code Security Auditor` |
| **Directory** | `security-builder/` |
| **Focus** | Vulnerability detection, security compliance, and remediation handoff |
| **Methodology** | OWASP Top 10:2025, CWE Top 25:2025, NIST SSDF v1.1, OWASP ASVS 5.0, CVSS v4.0 |

The **Security Builder** skill audits the actual implemented code — not designs or plans — for vulnerabilities, misconfigurations, and security anti-patterns. It performs risk assessment, sweeps all OWASP Top 10 categories, traces every external input from entry to sink, audits authentication and authorization, scans dependencies against vulnerability databases, and detects hardcoded secrets. Findings are packaged as structured remediation items for `bob-the-builder`.

#### Severity Classification

Findings use CVSS v4.0 scoring with four severity tiers: Critical (CVSS 9.0-10.0), High (CVSS 7.0-8.9), Medium (CVSS 4.0-6.9), and Low (CVSS 0.1-3.9). Each finding includes CWE mapping, exploit scenario, and a specific remediation action.

#### Reference Files

| File | Contents |
|---|---|
| `references/security-checklist.md` | Complete OWASP Top 10 mapping, CWE Top 25 patterns, AI-specific threats, supply chain checks |
| `references/vulnerability-patterns.md` | Injection, auth bypass, access control, crypto failures, SSRF, deserialization, info disclosure |
| `references/remediation-guide.md` | Priority matrix, fix patterns per category, verification procedures, remediation report format |

---

### Gatekeeper Build

| | |
|---|---|
| **Skill name** | `Adversarial Implementation Validator` |
| **Directory** | `gatekeeper-build/` |
| **Focus** | Meta-review — validates the quality of other skills' deliverables |
| **Methodology** | 5-type challenge protocol, 7-dimension review, delegation workflow, adversarial scoring |

The **Gatekeeper Build** is fundamentally different from the other five skills. It **produces no code, no tests, and no designs**. Instead, it receives completed deliverables from the build skills and **challenges every claim** to ensure accuracy, completeness, and correctness before the pipeline advances. It reviews across 7 dimensions: spec alignment, code quality, security, testing, documentation, completeness, and correctness.

#### The 5 Challenge Types

| # | Challenge | What It Verifies |
|---|---|---|
| 1 | **Existence** | Does the cited code, test, or finding actually exist at the stated location? |
| 2 | **Accuracy** | Are implementations logically correct and classifications properly assigned? |
| 3 | **Completeness** | Does the deliverable cover the full scope defined by the design spec? |
| 4 | **Proportionality** | Do severity ratings and effort allocation match actual impact? |
| 5 | **Consistency** | Is there internal coherence within and across deliverables? |

#### Verdicts

| Verdict | Condition |
|---|---|
| **APPROVED** | All challenges resolved, evidence verified, no contradictions |
| **REVISE** | Specific issues identified — delegated back to originating skill with clear instructions |
| **ESCALATE** | Fundamental misalignment or 3+ revision cycles exhausted — requires user intervention |

#### Delegation Mechanism

When the Gatekeeper finds an issue, it sends a structured delegation request back to the originating skill. The skill responds with one of three resolutions: **corrected**, **defended**, or **withdrawn**. Maximum 2 rounds per finding — after Round 2, unresolved findings are marked **Disputed** with both positions documented for the user.

#### Reference Files

| File | Contents |
|---|---|
| `references/challenge-protocol.md` | Complete challenge rubric: 5 categories x 7 dimensions with examples and resolution criteria |
| `references/review-criteria.md` | Per-phase review checklists for code, tests, and security audit deliverables |
| `references/delegation-workflow.md` | Delegation formats, batch strategies, round management, escalation procedures |

---

### Cross-Check Build Confirm

| | |
|---|---|
| **Skill name** | `Implementation Completeness Scanner` |
| **Directory** | `cross-check-build-confirm/` |
| **Focus** | Final completeness verification — no scaffold, no placeholders, no unfinished code |
| **Methodology** | Static pattern scanning, structural analysis, behavioral verification, exhaustive completeness checklist |

The **Cross-Check Build Confirm** skill is the final quality gate before delivery. It performs an exhaustive six-step scan of the entire codebase to verify that no placeholder code, TODO markers, mock implementations, fake data, debug statements, or incomplete logic survived the build process. Every finding is non-negotiable — findings are delegated to `bob-the-builder` for remediation, and the scan repeats until the codebase achieves a CLEAN verdict.

#### Scan Methodology (6 Steps)

1. **Static pattern scan** — TODO, FIXME, HACK, XXX, PLACEHOLDER, MOCK, STUB, TEMP, DUMMY, FAKE, SAMPLE
2. **Structural completeness** — Verify all modules from the design spec have corresponding implementations
3. **Behavioral completeness** — Detect hardcoded returns, empty catch blocks, console.log debugging
4. **Data completeness** — Find lorem ipsum, example.com, test@test.com, fake addresses
5. **Configuration completeness** — Audit default configs, missing .env.example entries, hardcoded URLs
6. **Documentation completeness** — Detect INSERT HERE, blank sections, template text

#### CLEAN Verdict Requirements

Zero BLOCKER findings, zero WARNING findings (or all justified), 100% feature completeness, 100% API endpoint completeness, all environment variables documented, and no scaffold code in production paths.

#### Reference Files

| File | Contents |
|---|---|
| `references/scaffold-detection.md` | Exhaustive pattern catalog: comment markers, code patterns, data patterns, structural and config patterns |
| `references/completeness-checklist.md` | Feature matrix, module checklist, API endpoints, migrations, config, CLEAN verdict criteria |

---

## How the Skills Differ

The six skills are designed to be **complementary, not overlapping**. Each owns a distinct concern within the build pipeline:

| Aspect | build-management | bob-the-builder | test-builder | security-builder | gatekeeper-build | cross-check-build-confirm |
|---|---|---|---|---|---|---|
| **Core question** | "Is the pipeline on track?" | "Is the code correct?" | "Is the code tested?" | "Is the code secure?" | "Is the output valid?" | "Is the code complete?" |
| **Focus** | Orchestration & delivery | Production implementation | Test suite creation | Vulnerability detection | Adversarial validation | Completeness verification |
| **Produces** | Delegation & consolidation | Source code & manifests | Test files & coverage | Audit reports & findings | Review verdicts | Scan reports & verdicts |
| **Mindset** | Strategic / Managerial | Pragmatic / Engineering | Methodical / Analytical | Skeptical / Defensive | Adversarial / Skeptical | Exhaustive / Forensic |
| **Key standards** | State machine protocol | IEEE 730, ISO 25010 | ISO 29119, ISTQB | OWASP, CWE, NIST SSDF | 5-type challenge protocol | Pattern detection catalog |
| **Output verdict** | Pipeline state transitions | Change manifest | Test report + coverage | Security audit + CVSS | APPROVED / REVISE / ESCALATE | CLEAN / FINDINGS |

### When to Use Which

| Scenario | Skill(s) to invoke |
|---|---|
| "Build this project from a design spec" | `build-management` (orchestrates all others) |
| "Implement this feature" | `bob-the-builder` |
| "Write tests for this codebase" | `test-builder` |
| "Audit this code for security" | `security-builder` |
| "Validate this build output" | `gatekeeper-build` |
| "Check for TODOs and placeholder code" | `cross-check-build-confirm` |
| "Full autonomous build pipeline" | `build-management` → all skills in sequence |

---

## How the Skills Connect

```
┌──────────────────────────────────────────────────────────────────────┐
│                        BUILD PIPELINE                                │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐    │
│  │                    BUILD-MANAGEMENT                          │    │
│  │    Orchestrates phased handoffs between all specialists      │    │
│  └────┬──────────────┬──────────────┬──────────────┬───────────┘    │
│       │              │              │              │                 │
│       ▼              ▼              ▼              ▼                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────────┐       │
│  │ BOB-THE- │  │  TEST-   │  │SECURITY- │  │ CROSS-CHECK-  │       │
│  │ BUILDER  │  │  BUILDER │  │ BUILDER  │  │ BUILD-CONFIRM │       │
│  │          │  │          │  │          │  │               │       │
│  │ Prod.    │  │ Test     │  │ Security │  │ Completeness  │       │
│  │ Code     │  │ Suite    │  │ Audit    │  │ Scan          │       │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └───────┬───────┘       │
│       │              │              │              │                 │
│       └──────────────┴──────┬───────┴──────────────┘                │
│                             ▼                                        │
│                   ┌─────────────────────┐                            │
│                   │   GATEKEEPER-BUILD  │                            │
│                   │                     │                            │
│                   │  1. Review output   │                            │
│                   │  2. Challenge claims │                            │
│                   │  3. Delegate issues  │                            │
│                   │  4. Issue verdict    │                            │
│                   └──────────┬──────────┘                            │
│                              │                                       │
│                   ┌──────────▼──────────┐                            │
│                   │  FINAL BUILD PACKAGE │                            │
│                   │  → Delivered to user │                            │
│                   └─────────────────────┘                            │
└──────────────────────────────────────────────────────────────────────┘
```

**Connection points:**

1. **Phased Delegation:** `build-management` orchestrates the sequential workflow, advancing only when the gatekeeper approves each phase.
2. **Gatekeeper Gates:** `gatekeeper-build` sits between every phase transition. No phase output reaches the next skill without adversarial validation.
3. **Remediation Loops:** `gatekeeper-build` and `cross-check-build-confirm` both route findings back to `bob-the-builder` for correction, creating closed-loop quality feedback.
4. **Security Remediation:** `security-builder` findings flow through `build-management` back to `bob-the-builder`, with `gatekeeper-build` validating the fixes.
5. **Completeness Verification:** `cross-check-build-confirm` operates as a final sweep after all other phases, with its own loop back to `bob-the-builder` until the codebase is clean.

---

## Step-by-Step Usage

> **Activation note:** The canonical entry point is `build-management`. In agentic frameworks that auto-route installed skills, prompt that entry point directly. In assistants that do not auto-route skills, reference `build-management/SKILL.md` or another specific skill file explicitly as a fallback.

### Running the Full Autonomous Pipeline (Agentic Frameworks)

1. **Install the inner skill folders** in your configured skills directory.
2. **Open your AI coding agent** (Codex, Kilo Code, or OpenCode).
3. **Provide the design specification** (a design package from Dev-Design SkillSet, an SRS, or an implementation plan).
4. **Request the `build-management` entry point:**
   ```
   "Use the build-management skill to implement this design specification"
   ```
5. The `build-management` skill orchestrates all phases autonomously.
6. The `gatekeeper-build` skill validates every phase output behind the scenes.
7. The `cross-check-build-confirm` skill performs final completeness verification.
8. You receive a fully implemented, tested, security-audited, and completeness-verified build package.

### Running a Single Skill

You can bypass `build-management` and target a specific skill directly when you only need one part of the pipeline:

1. **Provide context (if required by your agent):** Explicitly attach or reference the relevant `SKILL.md` file (e.g., `@workspace`, or dragging the file into chat).
2. **Ask for a specific task** using natural language:
   - `"Read build-management/SKILL.md and implement this design specification"` → activates **build-management**
   - `"Read bob-the-builder/SKILL.md and implement this feature"` → activates **bob-the-builder**
   - `"Read test-builder/SKILL.md and write tests for this codebase"` → activates **test-builder**
   - `"Read security-builder/SKILL.md and audit this code for vulnerabilities"` → activates **security-builder**
   - `"Read gatekeeper-build/SKILL.md and validate this implementation"` → activates **gatekeeper-build**
   - `"Read cross-check-build-confirm/SKILL.md and scan for placeholder code"` → activates **cross-check-build-confirm**

### Trigger Phrases Quick Reference

| Skill | Example Prompting Phrases |
|---|---|
| **build-management** | "build this project", "implement this design", "start the build pipeline", "run the Build Team" |
| **bob-the-builder** | "write the code", "implement this feature", "build this component", "fix these findings" |
| **test-builder** | "write tests", "create test suite", "add unit tests", "improve test coverage" |
| **security-builder** | "audit code security", "check for vulnerabilities", "review security", "check OWASP compliance" |
| **gatekeeper-build** | "review this build", "validate the implementation", "gate-check the code", "approve this phase" |
| **cross-check-build-confirm** | "scan for incomplete code", "check for TODOs", "find placeholder code", "confirm build is production-ready" |

---

## Installation Guide

Each skill relies on a `SKILL.md` file (the main instructions) and a `references/` directory (detailed checklists and patterns). To install them, copy the **inner skill folders** from `Build-Team_SkillSet/` into the skills directory your agent is configured to read.

This README assumes you are working from the combined AI SkillSets repository. If you are using a separately published mirror of this SkillSet, the same install rule applies; only the surrounding repo path changes.

### Canonical Install Rule

Copy these folders, not the top-level `Build-Team_SkillSet/` directory:

- `build-management/`
- `bob-the-builder/`
- `test-builder/`
- `security-builder/`
- `gatekeeper-build/`
- `cross-check-build-confirm/`

Use `.agents/skills/` when your environment supports a shared skills directory, or the configured equivalent for your assistant.

---

### Standard Assistants (Claude Code and GitHub Copilot)

These assistants do **not** always auto-route to installed skill files. Keep the same copied skill folders in an accessible location, then point the assistant at the right entry point or fallback `SKILL.md` file.

#### Claude Code
Claude Code does not use a guaranteed native skills directory, so the important part is keeping the copied folders somewhere your project instructions can reference consistently.

1. Copy the installed Build-Team skill folders into a location your workspace can reference, such as `.agents/skills/` or `.claude/skills/`.
2. Add a reference in your **`CLAUDE.md`** (project root):
   ```markdown
   ## Build Team Skills
   This project uses the Build-Team SkillSet. Start implementation work with the build-management entry point. If Claude does not auto-route the installed skill, read `.agents/skills/build-management/SKILL.md` first.
   ```

#### GitHub Copilot
GitHub Copilot does not guarantee automatic routing from a custom skills folder.

1. Copy the installed Build-Team skill folders into a location your repository instructions can reference.
2. Reference them in **`.github/copilot-instructions.md`**:
   ```markdown
   ## Build Implementation
   When asked to implement designs or build features, start with build-management. If Copilot does not auto-route the installed skill, use `@workspace` and follow `.agents/skills/build-management/SKILL.md`.
   ```

---

### Agentic Frameworks (Codex, Kilo Code, OpenCode)

If you are using a dedicated agentic runner that natively recognizes a `skills` directory structure:

1. Create the environment's skills directory, typically `.agents/skills/` or the framework-specific equivalent.
2. Copy the individual skill folders (`build-management`, `bob-the-builder`, and the rest) directly into that directory.
3. Prompt the `build-management` entry point directly.
4. If the framework does not auto-route as expected, fall back to `build-management/SKILL.md`.

---

### Verification

After installing in any agent, verify the skills are detected by asking:

```
"What skills do you have available?"
```

or directly trigger the pipeline:

```
"Build this project from the attached design specification"
```

If the agent responds with a phased build plan following the build-management's orchestration protocol, the installation was successful.

---

## Directory Structure

```
Build-Team_SkillSet/
├── README.md                              # This file
├── QUICK-START.md                         # Brief onboarding guide
├── build-management/                      # Build Pipeline Orchestrator
│   ├── SKILL.md
│   └── references/
│       ├── workflow-protocol.md           # State machine and transition rules
│       └── handoff-templates.md           # Delegation templates for all phases
├── bob-the-builder/                       # Senior Development Engineer
│   ├── SKILL.md
│   └── references/
│       ├── coding-standards.md            # Clean Code, IEEE 730, ISO 25010
│       ├── implementation-patterns.md     # Domain-first org, repository, DI, API patterns
│       └── output-protocol.md             # Change manifest and delivery format
├── test-builder/                          # Test Engineering Specialist
│   ├── SKILL.md
│   └── references/
│       ├── test-strategy.md               # Test pyramid, coverage targets, TDD
│       └── test-patterns.md               # AAA, fixtures, mocking, anti-patterns
├── security-builder/                      # Code Security Auditor
│   ├── SKILL.md
│   └── references/
│       ├── security-checklist.md          # OWASP Top 10, CWE Top 25, AI threats
│       ├── vulnerability-patterns.md      # Injection, auth, access control, crypto
│       └── remediation-guide.md           # Priority matrix, fix patterns, verification
├── gatekeeper-build/                      # Adversarial Implementation Validator
│   ├── SKILL.md
│   └── references/
│       ├── challenge-protocol.md          # 5-type challenge rubric with examples
│       ├── review-criteria.md             # Per-phase checklists (code, tests, security)
│       └── delegation-workflow.md         # Delegation formats and escalation
└── cross-check-build-confirm/             # Implementation Completeness Scanner
    ├── SKILL.md
    └── references/
        ├── scaffold-detection.md          # Pattern catalog with regex and false positives
        └── completeness-checklist.md      # Feature matrix, CLEAN verdict criteria
```

---

## Contributing

Contributions are welcome! If you'd like to improve the skills, add new reference material, or fix issues:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improve-coding-standards`)
3. Make your changes
4. Submit a pull request

For questions or suggestions, open an issue or discussion in the repository where you found this SkillSet.

---

<p align="center">
  <sub>Built by <a href="https://github.com/TykoDev">TykoDev</a> · Build-Team SkillSet</sub>
</p>
