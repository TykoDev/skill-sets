<p align="center">
  <img src="https://img.shields.io/badge/TykoDev-AI%20SkillSets-blueviolet?style=for-the-badge&logo=brain&logoColor=white" alt="TykoDev AI SkillSets" />
  <br/>
  <em>Modular AI agent skill libraries for autonomous software design, implementation, and review</em>
</p>

# TykoDev AI SkillSets

> **Author:** [TykoDev](https://github.com/TykoDev)
> **License:** See individual SkillSet repositories for license details

Three modular, interconnected **AI SkillSets** that together form a complete autonomous software development pipeline — from initial idea to production-ready, security-audited, quality-verified code. Each SkillSet contains specialized AI skills that plug into any agent supporting skill/tool folders: **Codex**, **Claude Code**, **GitHub Copilot**, **Kilo Code**, and **OpenCode**.

**18 skills. 43 reference documents. 14 tech-stack templates. One unified pipeline.**

---

## Table of Contents

- [The Three SkillSets](#the-three-skillsets)
- [How They Connect](#how-they-connect)
- [Dev-Design SkillSet](#dev-design-skillset)
- [Build-Team SkillSet](#build-team-skillset)
- [Code-Check SkillSet](#code-check-skillset)
- [End-to-End Pipeline](#end-to-end-pipeline)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Repository Structure](#repository-structure)
- [Supporting Files](#supporting-files)

---

## The Three SkillSets

| SkillSet | Purpose | Skills | Architecture |
|---|---|---|---|
| [**Dev-Design**](#dev-design-skillset) | Design specifications — from idea to architecture | 7 skills + 14 tech-stack templates | Orchestrator-led pipeline |
| [**Build-Team**](#build-team-skillset) | Code implementation — from design to production code | 6 skills | Orchestrator-led pipeline |
| [**Code-Check**](#code-check-skillset) | Code review — multi-dimensional quality validation | 5 skills | Hub-and-spoke |

Each SkillSet is **self-contained** and can be used independently. When used together, they form a complete pipeline where the output of one SkillSet feeds directly into the next.

### Shared Design Principles

Every SkillSet follows the same foundational conventions:

| Principle | How It Is Applied |
|---|---|
| **Separation of Concerns** | Each skill owns exactly one responsibility — no overlap between skills |
| **Adversarial Validation** | Every SkillSet includes a dedicated Gatekeeper that challenges outputs before delivery |
| **Framework-Anchored** | All findings and decisions map to industry standards (IEEE, ISO, OWASP, CWE, NIST, CVSS) |
| **Evidence-Based** | Every claim requires file paths, line numbers, code references, or spec traceability |
| **Progressive Disclosure** | Lean SKILL.md bodies (~1,200-1,800 words) with detailed reference files (~1,200-2,000 words) |

---

## How They Connect

The three SkillSets form a sequential pipeline. Each produces a structured output package that the next SkillSet consumes:

```
 ┌──────────────────────────────────────────────────────────────────────────┐
 │                    COMPLETE DEVELOPMENT PIPELINE                         │
 │                                                                          │
 │   USER IDEA                                                              │
 │       │                                                                  │
 │       ▼                                                                  │
 │   ┌──────────────────────────────────────────┐                           │
 │   │          DEV-DESIGN SKILLSET             │                           │
 │   │                                          │                           │
 │   │  "What are we building and how?"         │                           │
 │   │                                          │                           │
 │   │  commander → researcher → planner →      │                           │
 │   │  architect → designer → engineer         │                           │
 │   │  (gatekeeper-design validates each)      │                           │
 │   │                                          │                           │
 │   │  Output: Approved Design Package         │                           │
 │   │  (SRS, architecture, API contracts,      │                           │
 │   │   component specs, implementation plan)  │                           │
 │   └──────────────────┬───────────────────────┘                           │
 │                      │                                                   │
 │                      ▼                                                   │
 │   ┌──────────────────────────────────────────┐                           │
 │   │          BUILD-TEAM SKILLSET             │                           │
 │   │                                          │                           │
 │   │  "Build it, test it, secure it."         │                           │
 │   │                                          │                           │
 │   │  build-management → bob-the-builder →    │                           │
 │   │  test-builder → security-builder →       │                           │
 │   │  cross-check-build-confirm               │                           │
 │   │  (gatekeeper-build validates each)       │                           │
 │   │                                          │                           │
 │   │  Output: Production-Ready Code           │                           │
 │   │  (source, tests, security audit,         │                           │
 │   │   completeness verification)             │                           │
 │   └──────────────────┬───────────────────────┘                           │
 │                      │                                                   │
 │                      ▼                                                   │
 │   ┌──────────────────────────────────────────┐                           │
 │   │          CODE-CHECK SKILLSET             │                           │
 │   │                                          │                           │
 │   │  "Is it actually good?"                  │                           │
 │   │                                          │                           │
 │   │  bug-review + code-review +              │                           │
 │   │  quality-review + security-review        │                           │
 │   │  (gatekeeper-code validates all)         │                           │
 │   │                                          │                           │
 │   │  Output: Validated Review Reports        │                           │
 │   │  (bugs, quality, security, merge         │                           │
 │   │   readiness — adversarially verified)    │                           │
 │   └──────────────────┬───────────────────────┘                           │
 │                      │                                                   │
 │                      ▼                                                   │
 │              PRODUCTION-READY                                            │
 │          Designed, built, and reviewed                                   │
 └──────────────────────────────────────────────────────────────────────────┘
```

### Handoff Points

| From | To | What Is Passed |
|---|---|---|
| **Dev-Design** → **Build-Team** | Design Package: SRS, C4 architecture, API contracts, component specs, implementation plan, tech-stack selections |
| **Build-Team** → **Code-Check** | Build Package: production source code, test suites, security audit report, completeness scan results |
| **Code-Check** → **User** | Validated review reports with adversarially-verified findings, severity ratings, and merge recommendation |

---

## Dev-Design SkillSet

> *Autonomous multi-agent software design specification workflow*

The **Dev-Design SkillSet** transforms a product idea or feature request into a complete, gatekeeper-approved design specification. It coordinates 7 specialized skills through a phased pipeline managed by the `commander` orchestrator.

### Skills (7 + Tech Stacks Library)

| Skill | Role | Key Standards |
|---|---|---|
| **commander** | Pipeline orchestrator — delegates phases and compiles the final design package | Phased delegation protocol |
| **researcher** | Requirements and domain analysis — SRS, bounded contexts, acceptance criteria | ISO 29148, ISO 25010, DDD |
| **planner** | Project strategy — milestones, risk register, delivery phasing | 12-Factor, CI/CD rollout |
| **architect** | System design — C4 diagrams, ADRs, API contracts, backend structure | C4/Arc42, Clean Architecture |
| **designer** | Frontend strategy — UI patterns, component design, accessibility | WCAG, Core Web Vitals |
| **engineer** | Implementation specification — repo structure, DevOps, validation, CI/CD | OWASP, OpenTelemetry, SLSA |
| **gatekeeper-design** | Adversarial validator — challenges every phase output before advancement | 7-dimension review matrix |

The **tech-stacks** library provides 14 specific backend and frontend stack templates (Node/TypeScript, Python/FastAPI, Rust/Axum, Go/Gin, .NET, React/Next.js, Vue/Nuxt, Angular, Svelte, Astro, and more) that skills reference to ensure implementation instructions match real-world configurations.

### Pipeline Flow

```
User Idea → commander → researcher → gatekeeper ✓ → planner → gatekeeper ✓ →
architect → gatekeeper ✓ → designer → gatekeeper ✓ → engineer → gatekeeper ✓ →
Final Design Package
```

### Trigger Phrases

```
"Design a new application for..."
"Use the commander skill to design..."
"Create an architecture for..."
"Map the bounded contexts of..."
```

> **Full documentation:** [`Dev-Design_SkillSet/README.md`](Dev-Design_SkillSet/README.md) | **Quick start:** [`Dev-Design_SkillSet/QUICK-START.md`](Dev-Design_SkillSet/QUICK-START.md)

---

## Build-Team SkillSet

> *Autonomous multi-agent code implementation pipeline*

The **Build-Team SkillSet** takes an approved design specification and autonomously produces production-ready, tested, security-audited, completeness-verified code. It coordinates 6 specialized skills through a four-phase pipeline managed by the `build-management` orchestrator.

### Skills (6)

| Skill | Role | Key Standards |
|---|---|---|
| **build-management** | Pipeline orchestrator — delegates phases, manages revision cycles, delivers final package | State machine protocol |
| **bob-the-builder** | Senior development engineer — writes production code from design specs | IEEE 730, ISO/IEC 25010 |
| **test-builder** | Test engineer — creates comprehensive, meaningful test suites | ISO/IEC 29119, ISTQB |
| **security-builder** | Security auditor — audits implemented code for vulnerabilities | OWASP Top 10, CWE Top 25, NIST SSDF |
| **gatekeeper-build** | Adversarial validator — challenges every phase output before advancement | 5-type challenge protocol |
| **cross-check-build-confirm** | Completeness scanner — verifies zero placeholders, TODOs, or scaffold remain | Exhaustive pattern catalog |

### Pipeline Flow

```
Design Package → build-management →
  Phase 1: bob-the-builder (code) → gatekeeper ✓ →
  Phase 2: test-builder (tests) → gatekeeper ✓ →
  Phase 3: security-builder (audit) → gatekeeper ✓ →
  Phase 4: cross-check-build-confirm (completeness) → gatekeeper ✓ →
Final Build Package
```

### Trigger Phrases

```
"Build this project from the design spec"
"Implement this design"
"Start the build pipeline"
"Write tests for this codebase"
"Audit this code for security vulnerabilities"
"Scan for placeholder code and TODOs"
```

> **Full documentation:** [`Build-Team_SkillSet/README.md`](Build-Team_SkillSet/README.md) | **Quick start:** [`Build-Team_SkillSet/QUICK-START.md`](Build-Team_SkillSet/QUICK-START.md)

---

## Code-Check SkillSet

> *Adversarial, multi-dimensional code review*

The **Code-Check SkillSet** provides systematic, evidence-based code review through 4 specialized review skills and 1 adversarial meta-reviewer. Unlike the other two SkillSets, it uses a **hub-and-spoke** architecture — review skills operate independently and the Gatekeeper validates all reports before they reach the user.

### Skills (5)

| Skill | Role | Key Standards |
|---|---|---|
| **bug-review** | Correctness defects — crashes, data corruption, logic errors, resource leaks | CWE Top 25, IEEE 1044, IBM ODC |
| **code-review** | Holistic merge-readiness across 8 review dimensions | Google's code review framework |
| **quality-review** | Long-term code health — maintainability, architecture, efficiency, tech debt | Clean Code, C4, DORA, IEEE 730 |
| **security-review** | Exploitability and compliance — vulnerability detection and threat modeling | NIST SSDF, OWASP ASVS, STRIDE |
| **gatekeeper-code** | Adversarial meta-reviewer — challenges every finding for accuracy before delivery | 5-type challenge protocol |

### Architecture (Hub-and-Spoke)

```
User Request → [bug-review, code-review, quality-review, security-review]
                    │            │              │              │
                    └────────────┴──────┬───────┴──────────────┘
                                        ▼
                                 gatekeeper-code
                                        │
                                        ▼
                               Validated Reports → User
```

### Trigger Phrases

```
"Find bugs in this code"
"Review this PR for merge readiness"
"Check code quality and standards"
"Review for security vulnerabilities"
"Validate these review findings"
```

> **Full documentation:** [`Code-Check_SkillSet/README.md`](Code-Check_SkillSet/README.md) | **Quick start:** [`Code-Check_SkillSet/QUICK-START.md`](Code-Check_SkillSet/QUICK-START.md)

---

## End-to-End Pipeline

### Full Autonomous Workflow

For a complete idea-to-reviewed-code pipeline, invoke the SkillSets in sequence:

1. **Design Phase** — Start with `commander` from Dev-Design SkillSet:
   ```
   "Use the commander skill to design a new REST API for a booking system"
   ```
   Output: Approved design package with SRS, architecture, API contracts, and implementation plan.

2. **Build Phase** — Feed the design into `build-management` from Build-Team SkillSet:
   ```
   "Use the build-management skill to implement the approved design specification"
   ```
   Output: Production code, test suite, security audit, and completeness verification.

3. **Review Phase** — Run the build output through Code-Check SkillSet:
   ```
   "Do a full code review using all Code-Check skills"
   ```
   Output: Adversarially-validated review reports with merge recommendation.

### Using Individual SkillSets

Each SkillSet works independently. Common standalone scenarios:

| Scenario | SkillSet | Entry Point |
|---|---|---|
| Design a new feature from scratch | Dev-Design | `commander` |
| Implement an existing design or plan | Build-Team | `build-management` |
| Write code for a specific module | Build-Team | `bob-the-builder` (direct) |
| Review existing code for bugs | Code-Check | `bug-review` |
| Full PR review before merge | Code-Check | `code-review` |
| Security audit of a codebase | Code-Check or Build-Team | `security-review` or `security-builder` |

### Using Individual Skills

Every skill can be invoked directly by referencing its `SKILL.md` file:

```
"Read skills/[skillset]/[skill]/SKILL.md and [task description]"
```

---

## Quick Start

### 1. Get the SkillSets

Clone or download the repository containing the SkillSet directories.

### 2. Copy Into Your Project

Copy the SkillSet folders you need into your project workspace:

```bash
# All three SkillSets
cp -r Code-Check_SkillSet/  your-project/skills/
cp -r Dev-Design_SkillSet/  your-project/skills/
cp -r Build-Team_SkillSet/  your-project/skills/

# Or just the one you need
cp -r Code-Check_SkillSet/  your-project/skills/
```

### 3. Configure Your AI Agent

**Claude Code** — Add to your `CLAUDE.md`:
```markdown
## AI SkillSets
This project uses the TykoDev AI SkillSets:
- For design: read `skills/Dev-Design_SkillSet/commander/SKILL.md`
- For building: read `skills/Build-Team_SkillSet/build-management/SKILL.md`
- For review: read `skills/Code-Check_SkillSet/[skill]/SKILL.md`
```

**GitHub Copilot** — Add to `.github/copilot-instructions.md`:
```markdown
## AI SkillSets
Use `@workspace` and read the appropriate SKILL.md from `skills/` before performing design, build, or review tasks.
```

**Agentic Frameworks (Codex, Kilo Code, OpenCode)** — Copy skill folders into the framework's skills directory (`.agents/skills/`, `.codex/skills/`, etc.) for automatic routing.

### 4. Start Using

```
"Design a new e-commerce platform"              → Dev-Design pipeline
"Implement this design specification"            → Build-Team pipeline
"Find bugs in the authentication module"         → Code-Check bug-review
"Do a full code review of this PR"               → Code-Check full pipeline
```

---

## Installation

### Standard AI Assistants (Claude Code, GitHub Copilot)

These tools do not auto-route to skill files. Provide context by:
1. Copying skill folders into your workspace
2. Referencing the appropriate `SKILL.md` in your system instructions or chat context
3. Explicitly asking the agent to read the SKILL.md before performing tasks

### Agentic Frameworks (Codex, Kilo Code, OpenCode)

These frameworks natively recognize skill directory structures:
1. Copy skill folders into the framework's configured skills directory
2. The framework parses frontmatter metadata and routes requests based on trigger phrases
3. Simply ask naturally — the framework selects the right skill

### Verification

After installation, verify by asking:
```
"What skills do you have available?"
```

---

## Repository Structure

```
/
├── README.md                              # This file — root guidance
├── AGENTS.md                              # Senior Development Architect persona
│
├── Code-Check_SkillSet/                   # Code review (5 skills)
│   ├── README.md
│   ├── QUICK-START.md
│   ├── bug-review/                        # Correctness defect detection
│   │   ├── SKILL.md
│   │   └── references/                    # checklist, detection-techniques, triage-workflow
│   ├── code-review/                       # Holistic 8-dimension PR review
│   │   ├── SKILL.md
│   │   └── references/                    # review-dimensions, pr-workflow, feedback-guide
│   ├── quality-review/                    # Maintainability, architecture, efficiency
│   │   ├── SKILL.md
│   │   └── references/                    # standards-enforcement, architecture-review, metrics-and-debt
│   ├── security-review/                   # Vulnerability detection and compliance
│   │   ├── SKILL.md
│   │   └── references/                    # frameworks, secure-coding-checklist, supply-chain
│   └── gatekeeper-code/                   # Adversarial report validator
│       ├── SKILL.md
│       └── references/                    # challenge-protocol, delegation-workflow, cross-validation
│
├── Dev-Design_SkillSet/                   # Design specification (7 skills + tech-stacks)
│   ├── README.md
│   ├── QUICK-START.md
│   ├── commander/                         # Pipeline orchestrator
│   │   ├── SKILL.md
│   │   └── references/                    # workflow-protocol, handoff-templates
│   ├── researcher/                        # Requirements and domain analysis
│   │   ├── SKILL.md
│   │   └── references/                    # requirements-template, domain-analysis
│   ├── planner/                           # Project strategy and risk management
│   │   ├── SKILL.md
│   │   └── references/                    # project-plan-template
│   ├── architect/                         # System architecture and API contracts
│   │   ├── SKILL.md
│   │   └── references/                    # arc42-template, architecture-patterns
│   ├── designer/                          # UI/UX and frontend strategy
│   │   ├── SKILL.md
│   │   └── references/                    # frontend-patterns, design-system-template
│   ├── engineer/                          # Implementation and DevOps specification
│   │   ├── SKILL.md
│   │   └── references/                    # implementation-patterns, devops-patterns
│   ├── gatekeeper-design/                 # Adversarial design validator
│   │   ├── SKILL.md
│   │   └── references/                    # review-criteria, adversarial-protocol
│   └── tech-stacks/                       # 14 backend + frontend stack templates
│       ├── node-typescript.md
│       ├── python-fastapi.md
│       ├── rust-axum.md
│       ├── go-gin.md
│       ├── dotnet-aspnet.md
│       ├── bun-typescript.md
│       ├── deno-typescript.md
│       ├── react-nextjs.md
│       ├── react-tanstack.md
│       ├── vue-nuxt.md
│       ├── angular.md
│       ├── svelte-sveltekit.md
│       ├── astro.md
│       └── vite-spa.md
│
├── Build-Team_SkillSet/                   # Code implementation (6 skills)
│   ├── README.md
│   ├── QUICK-START.md
│   ├── build-management/                  # Pipeline orchestrator
│   │   ├── SKILL.md
│   │   └── references/                    # workflow-protocol, handoff-templates
│   ├── bob-the-builder/                   # Senior development engineer
│   │   ├── SKILL.md
│   │   └── references/                    # coding-standards, implementation-patterns, output-protocol
│   ├── test-builder/                      # Test engineering specialist
│   │   ├── SKILL.md
│   │   └── references/                    # test-strategy, test-patterns
│   ├── security-builder/                  # Code security auditor
│   │   ├── SKILL.md
│   │   └── references/                    # security-checklist, vulnerability-patterns, remediation-guide
│   ├── gatekeeper-build/                  # Adversarial implementation validator
│   │   ├── SKILL.md
│   │   └── references/                    # challenge-protocol, review-criteria, delegation-workflow
│   └── cross-check-build-confirm/         # Implementation completeness scanner
│       ├── SKILL.md
│       └── references/                    # scaffold-detection, completeness-checklist
│
└── _BUILD/                                # Skill development tooling
    ├── review-agent.md                    # Base review agent template
    └── .skills/
        ├── Create_SKILL.md                # Skill creation guide
        └── skill-reviewer.md              # Skill quality reviewer
```

---

## Supporting Files

### AGENTS.md

The `AGENTS.md` file at the repository root defines a **Senior Development Architect** persona with an initialization protocol, documentation-first workflow, and multi-agent coordination rules. It provides the overarching agent behavior that complements the specialized skills within each SkillSet.

### _BUILD Directory

The `_BUILD/` directory contains tooling for creating and reviewing new skills:

| File | Purpose |
|---|---|
| `review-agent.md` | Base template for adversarial review agents (Gatekeepers are derived from this) |
| `.skills/Create_SKILL.md` | Step-by-step guide for creating new skills following SkillSet conventions |
| `.skills/skill-reviewer.md` | Quality reviewer that validates new skills against standards |

These tools are used during SkillSet development, not during normal operation.

---

## The Gatekeeper Pattern

Every SkillSet includes a dedicated **Gatekeeper** — an adversarial meta-reviewer that produces no creative output of its own. Instead, it challenges the work of other skills using a standardized protocol:

| SkillSet | Gatekeeper | What It Validates |
|---|---|---|
| Dev-Design | `gatekeeper-design` | Design specifications, architecture decisions, requirement completeness |
| Build-Team | `gatekeeper-build` | Production code, test quality, security audit accuracy |
| Code-Check | `gatekeeper-code` | Review report accuracy, finding evidence, severity calibration |

All three Gatekeepers share the same **5 challenge categories**:

1. **Existence** — Does the claimed evidence actually exist?
2. **Accuracy** — Are classifications and logic correct?
3. **Completeness** — Does the output cover the full required scope?
4. **Proportionality** — Do severity ratings match actual impact?
5. **Consistency** — Is there internal coherence within and across outputs?

Challenges follow a **2-round maximum** — after Round 2, unresolved items are marked as **Disputed** and escalated to the user for final judgment.

---

<p align="center">
  <sub>Built by <a href="https://github.com/TykoDev">TykoDev</a> · TykoDev AI SkillSets</sub>
</p>
