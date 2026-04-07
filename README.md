<p align="center">
  <img src="https://img.shields.io/badge/TykoDev-AI%20SkillSets-blueviolet?style=for-the-badge&logo=brain&logoColor=white" alt="TykoDev AI SkillSets" />
  <br/>
  <em>Modular AI agent skill libraries for autonomous software design, implementation, and review</em>
</p>

<p align="center">
  <a href="https://youtu.be/NE5zkgg04pQ" target="_blank" rel="noopener noreferrer">
    <img src="https://img.youtube.com/vi/NE5zkgg04pQ/maxresdefault.jpg" alt="Watch the AI SkillSets video overview" width="720" />
  </a>
  <br/>
  <sub>Click the preview to watch the video walkthrough</sub>
</p>

# AI SkillSets

> **Author:** [TykoDev](https://github.com/TykoDev)

This repo is easiest to use as a practical three-step workflow across software delivery:

- use `commander` to design and plan the work
- use `build-management` to implement the approved plan
- use `code-chief` to review the resulting codebase, PR, or release candidate when that review orchestrator is available in your agent setup

Use `commander` when you have an idea, feature request, or rough scope and need a structured output such as requirements, architecture, milestones, and an implementation-ready plan.

Use `build-management` after the plan is approved. It is the main implementation entry point and is designed to coordinate coding, tests, security checks, and completeness validation without you manually driving each build skill.

Use `code-chief` when you want a full review pipeline over completed work and that orchestrator is installed in your agent environment. It is the clearest entry point for "review everything" requests, while the individual Code-Check skills still work well for targeted checks such as bug-only, quality-only, or security-only review.

**Typical prompts:**

- `"Use commander to design and plan a multi-tenant booking platform for small clinics."`
- `"Use build-management to implement this approved design specification."`
- `"Use code-chief to do a full code review of this branch before merge."`
- `"Use build-management to implement this plan and include a code-chief review before delivery."`
- `"Use commander to plan this feature, then use build-management to build it, then use code-chief to review the result."`

Use the plain skill name in prompts by default. Some agent environments also expose installed skills as slash commands such as `/commander`, `/build-management`, and `/code-chief`.

This gives you a simple default path: plan first, build second, review third. If `code-chief` is not installed in your setup, use the direct Code-Check review skills instead.

---

## Table of Contents

- [Installation](#installation)
- [How to Read These Docs](#how-to-read-these-docs)
- [Quick Start](#quick-start)
- [The Three SkillSets](#the-three-skillsets)
- [How They Connect](#how-they-connect)
- [Dev-Design SkillSet](#dev-design-skillset)
- [Build-Team SkillSet](#build-team-skillset)
- [Code-Check SkillSet](#code-check-skillset)
- [End-to-End Pipeline](#end-to-end-pipeline)
- [Repository Structure](#repository-structure)

---

## Installation

The repo is split into **three separate SkillSet groups**. You can install one, two, or all three depending on how you want to work.

- **Dev-Design_SkillSet** — planning, requirements, architecture, and implementation design
- **Build-Team_SkillSet** — implementation, testing, security checks, and build validation
- **Code-Check_SkillSet** — orchestrated code review pipeline: bug finding, code review, quality review, security review, adversarial penetration testing, and frontend audit

The goal is to keep setup simple: copy the skills you want into your agent’s skills folder and use them directly.

**Important:** copy the **actual skill folders inside each SkillSet**, not the top-level group folder itself.

For example, copy folders like:

- `commander/`
- `build-management/`
- `bug-review/`
- `quality-review/`
- `gatekeeper-build/`
- `gatekeeper-code/`

into your agent’s skills directory such as `.agents/skills/`.

This means you can stay lightweight:

- only install design skills if you want planning help
- only install build skills if you already have a plan
- only install review skills if you just want code checks

No extra framework is required beyond whatever skill-capable agent setup you already use.

---

## How to Read These Docs

This repository is the **primary install path** for the SkillSets, so the root README is the canonical setup guide.

The per-SkillSet `README.md` and `QUICK-START.md` files are still written to work if a SkillSet is published separately. In those mirror cases, the install rule and entry points stay the same, but the top-level clone path and surrounding repo layout may differ.

---

## Quick Start

### 1. Pick a SkillSet

Choose the group that matches what you need right now:

- want a plan: use **Dev-Design_SkillSet**
- want code built: use **Build-Team_SkillSet**
- want code reviewed: use **Code-Check_SkillSet**

### 2. Copy the skills you want

Copy the inner skill folders into your agent’s skills directory, usually `.agents/skills/`.

For Dev-Design installs, also copy the sibling `tech-stacks/` folder when you want the overlay templates available.

### 3. Start with the right entry point

Use one of these:

- **Dev-Design_SkillSet:** start with `commander`
- **Build-Team_SkillSet:** start with `build-management`
- **Code-Check_SkillSet:** start with `code-chief` for full pipeline reviews, or use individual skills like `bug-review` or `security-review` for targeted reviews

### Example prompts

```text
Use commander to turn this product idea into a build-ready plan.

Use build-management to implement this approved plan.

Use code-chief to do a full code review of this codebase.
```

That’s the whole idea: pick the group, install the skills, and use the entry point that fits the job.

Some assistants expose the same entry points as slash commands such as `/commander`; if yours does, the same guidance still applies.

If your assistant does **not** auto-route skills, use the same entry point concept but reference the relevant `<skill>/SKILL.md` file explicitly as a fallback.

---

## The Three SkillSets

### Dev-Design_SkillSet

Start with **`commander`** and give it your idea, context, constraints, and goals. It will coordinate the planning and design flow for you and produce a structured plan.

Best for:

- new app ideas
- feature design
- architecture planning
- requirements and implementation specs

### Build-Team_SkillSet

Hand your approved plan to **`build-management`** and let it manage implementation. This SkillSet is designed to take the design output and push it through coding, testing, security review, and completeness checks.

Best for:

- building from an existing plan
- delegating implementation
- adding tests and validation
- making sure the build output is actually complete

### Code-Check_SkillSet

Start with **`code-chief`** and it will orchestrate all six specialist review skills, submit reports to the adversarial meta-reviewer, and deliver a consolidated validated package. Individual skills can also be invoked directly for targeted reviews.

Best for:

- full-pipeline code review
- targeted bug, quality, or security review
- adversarial penetration testing
- frontend performance and accessibility audits

### Shared Traits

All three SkillSets follow the same general philosophy:

- each group includes a `gatekeeper-` skill that acts as an adversarial reviewer
- the design/planning side combines **14 tech-stack templates** with stack-agnostic requirements like auth, validation, encryption, and operational concerns
- everything is written in **plain Markdown**
- the documentation is meant to be readable and easy to verify for yourself

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
 │   │  code-chief → bug-review → code-review → │                           │
 │   │  quality-review → security-review →      │                           │
 │   │  mr-robot → frontier                     │                           │
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

The **Code-Check SkillSet** provides systematic, evidence-based code review through 6 specialist review skills, 1 pipeline orchestrator, and 1 adversarial meta-reviewer (8 skills total). It uses an **orchestrator-driven pipeline** architecture — `code-chief` delegates to each specialist in sequence, consolidates their reports, and submits the package to `gatekeeper-code` for adversarial validation before delivery.

### Skills (8)

| Skill | Role | Key Standards |
|---|---|---|
| **code-chief** | Pipeline orchestrator — delegates phases, manages gatekeeper cycles, delivers final package | State machine protocol |
| **bug-review** | Correctness defects — crashes, data corruption, logic errors, resource leaks | CWE Top 25, IEEE 1044, IBM ODC |
| **code-review** | Holistic merge-readiness across 8 review dimensions | Google's code review framework |
| **quality-review** | Long-term code health — maintainability, architecture, efficiency, tech debt | Clean Code, C4, DORA, IEEE 730 |
| **security-review** | Exploitability and compliance — vulnerability detection and threat modeling | NIST SSDF, OWASP ASVS, STRIDE |
| **mr-robot** | Adversarial penetration testing — offensive exploit chain construction | OWASP Testing Guide, PTES, CVSS 4.0 |
| **frontier** | Frontend analytics and UI/UX audit — performance, accessibility, frontend security | Core Web Vitals, WCAG 2.2, CSP |
| **gatekeeper-code** | Adversarial meta-reviewer — challenges every finding for accuracy before delivery | 5-type challenge protocol |

### Architecture (Orchestrator-Driven Pipeline)

```
User Request → code-chief → [bug-review → code-review → quality-review →
               security-review → mr-robot → frontier*] → gatekeeper-code
                                                                │
                                                       ┌────────┼────────┐
                                                       ▼                 ▼
                                               Validated Package   Not Ready
                                                   → User         (revision loop
                                                                   via code-chief)

* frontier skipped for backend-only projects
```

### Trigger Phrases

```
"Run code-chief on this codebase"
"Do a full code review of this project"
"Find bugs in this code"
"Review this PR for merge readiness"
"Run mr-robot to penetration test this API"
"Run frontier to audit the frontend"
"Review for security vulnerabilities"
```

> **Full documentation:** [`Code-Check_SkillSet/README.md`](Code-Check_SkillSet/README.md) | **Quick start:** [`Code-Check_SkillSet/QUICK-START.md`](Code-Check_SkillSet/QUICK-START.md)

---

## End-to-End Pipeline

### Full Autonomous Workflow

For a complete idea-to-reviewed-code pipeline, invoke the SkillSets in sequence:

1. **Design Phase** — Start with `commander` from Dev-Design SkillSet:
   ```
   "Use commander to design a new REST API for a booking system"
   ```
   Output: Approved design package with SRS, architecture, API contracts, and implementation plan.

2. **Build Phase** — Feed the design into `build-management` from Build-Team SkillSet:
   ```
   "Use build-management to implement the approved design specification"
   ```
   Output: Production code, test suite, security audit, and completeness verification.

3. **Review Phase** — Run the build output through Code-Check SkillSet:
   ```
   "Use code-chief to run a full code review of this project"
   ```
   Output: Adversarially-validated review reports with merge recommendation.

### Using Individual SkillSets

Each SkillSet works independently. Common standalone scenarios:

| Scenario | SkillSet | Entry Point |
|---|---|---|
| Design a new feature from scratch | Dev-Design | `commander` |
| Implement an existing design or plan | Build-Team | `build-management` |
| Write code for a specific module | Build-Team | `bob-the-builder` (direct) |
| Full pipeline code review | Code-Check | `code-chief` |
| Review existing code for bugs | Code-Check | `bug-review` |
| Full PR review before merge | Code-Check | `code-review` |
| Security audit of a codebase | Code-Check or Build-Team | `security-review` or `security-builder` |
| Adversarial penetration testing | Code-Check | `mr-robot` |
| Frontend performance/accessibility audit | Code-Check | `frontier` |

### Using Individual Skills

Every skill can be invoked directly by referencing its `SKILL.md` file (after copying the skill folder to your skills directory):

```
"Read skills/commander/SKILL.md and [task description]"
```

That manual `SKILL.md` pattern is a fallback for assistants that do not auto-route skills from the installed folders.

---

## Repository Structure

```
/
├── README.md                              # This file — root guidance
├── Code-Check_SkillSet/                   # Code review (8 skills)
│   ├── README.md
│   ├── QUICK-START.md
│   ├── code-chief/                        # Pipeline orchestrator
│   │   ├── SKILL.md
│   │   └── references/                    # workflow-protocol, handoff-templates
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
│   ├── mr-robot/                          # Adversarial penetration testing
│   │   ├── SKILL.md
│   │   └── references/                    # attack-methodology, exploit-patterns, severity-scoring
│   ├── frontier/                          # Frontend analytics and UI/UX audit
│   │   ├── SKILL.md
│   │   └── references/                    # performance-checklist, accessibility-checklist, frontend-security
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
│   │   └── references/
│   │       ├── design-system-template.md
│   │       ├── frontend-patterns.md
│   │       ├── layout-patterns.md
│   │       ├── styling-decision-matrix.md
│   │       ├── ui-ux-standards.md
│   │       ├── visual-design-guide.md
│   │       └── page-templates/            # 19 page-template resources (1 selection guide + 18 concrete templates)
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
```

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
  <sub>Built by <a href="https://github.com/TykoDev">TykoDev</a> · AI SkillSets</sub>
</p>
