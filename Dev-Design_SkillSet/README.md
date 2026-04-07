<p align="center">
  <img src="https://img.shields.io/badge/Dev--Design-SkillSet-blueviolet?style=for-the-badge&logo=architecture&logoColor=white" alt="Dev-Design SkillSet" />
  <br/>
  <em>Autonomous multi-agent software design specification workflow for AI coding agents</em>
</p>

# Dev-Design SkillSet

> **Author:** [TykoDev](https://github.com/TykoDev)
> **License:** See repository for license details

A modular suite of **seven specialized AI skills plus a tech-stack library** that orchestrates a fully autonomous software design specification workflow. This README assumes the combined **AI SkillSets** repository is your primary install path, while still staying usable if this SkillSet is published as a separate mirror.

The canonical install rule is the same in both cases: copy the **inner skill folders** into your agent's skills directory as siblings, include the sibling `tech-stacks/` folder when you want the stack templates available, and start with the `commander` entry point. With that sibling layout, direct skill references to overlays resolve as `../tech-stacks/<overlay>.md`. If your assistant does not auto-route installed skills, explicitly reference the relevant `<skill>/SKILL.md` file as a fallback.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Skill Descriptions](#skill-descriptions)
  - [Commander](#commander)
  - [Researcher](#researcher)
  - [Planner](#planner)
  - [Architect](#architect)
  - [Designer](#designer)
  - [Engineer](#engineer)
  - [Gatekeeper-Design](#gatekeeper-design)
- [Tech Stacks Library](#tech-stacks-library)
- [How the Skills Differ](#how-the-skills-differ)
- [How the Skills Connect](#how-the-skills-connect)
- [Step-by-Step Usage](#step-by-step-usage)
- [Installation Guide](#installation-guide)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)

---

## Overview

The **Dev-Design SkillSet** transforms an AI coding agent into a complete software design team. From a single user prompt, it coordinates a phased spec-driven development process where requirements, planning, architecture, UI design, and implementation steps are drafted by specialized agents and strictly validated by an adversarial gatekeeper.

### Key Principles

| Principle | Description |
|---|---|
| **Spec-Driven Development** | Exhaustive design upfront (ISO 29148, C4, Arc42) to minimize coding rework |
| **Separation of Concerns** | Each skill owns one design phase — requirements, architecture, or planning |
| **Adversarial Validation** | The Gatekeeper challenges outputs at every handoff using a strict checklist |
| **Framework-Anchored** | Mapped to OWASP, NIST SSDF, Domain-Driven Design (DDD), and Clean Architecture |
| **Tech-Stack Overlays** | Core logic delegates to one of 14 specific backend/frontend tech stack templates |

---

## Architecture

The SkillSet follows an **orchestrator-led pipeline** where the `commander` manages transitions through five core phases, owns the gatekeeper review cycle in pipeline mode, and `gatekeeper-design` guards every handoff:

```text
                        ┌─────────────────────┐
                        │     USER INPUT      │
                        └──────────┬──────────┘
                                   │
                                   ▼
                        ┌─────────────────────┐
                 ┌─────▶│      COMMANDER      │─────┐
                 │      └──────────┬──────────┘     │
                 │                 │                │
                 │        Delegates in order        │ (After all
                 │  Researcher → Planner →          │  required phases
                 │  Architect → Designer → Engineer │  are approved)
                 │                 │                │
                 │                 ▼                ▼
                 │      ┌─────────────────────┐  ┌─────────────────────┐
    [APPROVE]    │ ┌───▶│ Specialist returns  │  │                     │
  Output sent    │ │    │ deliverable +       │  │FINAL DESIGN PACKAGE │
 back to CMD     │ │    │ review packet       │  │                     │
 to step forward │ │    └──────────┬──────────┘  └─────────────────────┘
                 │ │               │
                 │ │   Commander submits review
                 │ │               │
                 │ │               ▼
                 │ │    ┌─────────────────────┐
                 └─┼────┤  GATEKEEPER-DESIGN  │
                   │    └──────────┬──────────┘
                   │               │
                   │               ▼
                   │            [REJECT]
                   └─────── Commander routes
                             findings back to
                             the same phase
```

### Data Flow

1. The user provides a product idea or feature request to the `commander`.
2. The `commander` activates the `researcher`, then `planner`, then `architect`, then `designer`, then `engineer` as needed.
3. In pipeline mode, each specialist returns a deliverable plus a review packet to `commander`.
4. The `commander` submits every phase to `gatekeeper-design`, handles approval cycles, and routes findings back to the correct specialist.
5. The `commander` compiles all gatekeeper-design-approved modules into a cohesive final design package.

---

## Skill Descriptions

### Commander

| | |
|---|---|
| **Skill name** | `Orchestration Specialist` |
| **Directory** | `commander/` |
| **Focus** | Pipeline oversight, state management, and user delivery |
| **Methodology** | Phased delegation protocol, autonomous step transitions |

The **Commander** is the central orchestrator. It receives user input, creates a phased execution plan, delegates to each specialist skill in sequence, owns the gatekeeper-design review loop in pipeline mode, collects approved outputs, and delivers the final consolidated design specification package. It also maintains the stack-lock registry across phases so downstream work inherits the correct overlays.

#### Reference Files
| File | Contents |
|---|---|
| `references/workflow-protocol.md` | Detailed step-by-step orchestration logic and state management |
| `references/handoff-templates.md` | Structured templates for delegating to each specialist |

---

### Researcher

| | |
|---|---|
| **Skill name** | `Requirements & Domain Specialist` |
| **Directory** | `researcher/` |
| **Focus** | Functional/non-functional requirements, Domain-Driven Design (DDD) |
| **Methodology** | ISO 29148, ISO 25010, Event Storming |

The **Researcher** identifies what the system needs to do. It establishes domain boundaries, translates loose ideas into strict verifiable requirements, and outlines acceptance criteria.

#### Reference Files
| File | Contents |
|---|---|
| `references/requirements-template.md` | Full SRS template, ISO 25010 Quality attributes framework |
| `references/domain-analysis.md` | Event Storming methodology and Bounded Context mapping |

---

### Planner

| | |
|---|---|
| **Skill name** | `Project Planning Specialist` |
| **Directory** | `planner/` |
| **Focus** | Milestones, risk management, delivery phasing |
| **Methodology** | 12-factor methodology, CI/CD rollout patterns |

The **Planner** outlines project strategy and delivery timelines. It establishes risks, creates a risk register, and designs a feature-flag or phased rollout structure to ensure a safe delivery pipeline.

#### Reference Files
| File | Contents |
|---|---|
| `references/project-plan-template.md` | Full project plan template and Risk assessment matrix |

---

### Architect

| | |
|---|---|
| **Skill name** | `System Architecture Specialist` |
| **Directory** | `architect/` |
| **Focus** | C4 Models, Arc42, backend structures, APIs |
| **Methodology** | Clean/Hexagonal Architecture, Domain-First Design |

The **Architect** designs the technical foundations. It builds C4 diagrams using Mermaid DSL, authors Architecture Decision Records (ADRs), and defines API contracts via OpenAPI or gRPC standards.

#### Reference Files
| File | Contents |
|---|---|
| `references/arc42-template.md` | Complete Arc42 v9.0 template adapted for AI |
| `references/architecture-patterns.md` | Clean/Hexagonal layers, Event-Driven patterns |

---

### Designer

| | |
|---|---|
| **Skill name** | `UI/UX Architecture Specialist` |
| **Directory** | `designer/` |
| **Focus** | Frontend patterns, component design, WCAG accessibility |
| **Methodology** | Islands architecture, component-driven design systems |

The **Designer** crafts the frontend strategy. It focuses on the UI framework patterns, accessibility checklists, Core Web Vitals targets, and managing global state over the application's view layers.

#### Reference Files
| File | Contents |
|---|---|
| `references/design-system-template.md` | Component specification format and accessibility checklists |
| `references/frontend-patterns.md` | Monolithic vs micro-frontends vs Jamstack tradeoffs |
| `references/layout-patterns.md` | Grid vs Flexbox selection, standard layout patterns, responsive strategy |
| `references/styling-decision-matrix.md` | Styling framework and component library decision tree |
| `references/ui-ux-standards.md` | Usability heuristics, interaction design, form design, navigation patterns |
| `references/visual-design-guide.md` | Typography, color, spacing, theming, animation, and visual identity |
| `references/page-templates/` | 19 page-template resources, including 1 selection guide and 18 concrete page templates |

---

### Engineer

| | |
|---|---|
| **Skill name** | `Implementation & DevOps Specialist` |
| **Directory** | `engineer/` |
| **Focus** | Code organization, DevOps, security implementation (OWASP) |
| **Methodology** | Multi-stage Docker, OpenTelemetry, SLSA Supply Chain |

The **Engineer** defines how the designs will be physically coded and deployed. It sets the exact repository structure, validation constraints (Zod/Pydantic), error handling paths, and the complete CI/CD and DevOps lifecycle.

#### Reference Files
| File | Contents |
|---|---|
| `references/implementation-patterns.md` | Repository patterns, ORM selection, validation patterns |
| `references/devops-patterns.md` | Github Actions templates, Docker patterns, IaC (OpenTofu) |

---

### Gatekeeper-Design

| | |
|---|---|
| **Skill name** | `Adversarial Validator` |
| **Directory** | `gatekeeper-design/` |
| **Focus** | Multi-dimensional constraint checking, error finding |
| **Methodology** | Adversarial review, scoring rewards |

The **Gatekeeper** is fundamentally different from the creative skills. It **produces no designs**. It receives completed blueprints and **ruthlessly challenges them** to ensure accuracy, safety, and coherence against upstream inputs. In the full pipeline, `commander` owns the review loop and submits every phase. In standalone direct skill use, a specialist or user may submit a deliverable directly. It uses a scoring system highly incentivized to reject faulty logic.

#### Reference Files
| File | Contents |
|---|---|
| `references/review-criteria.md` | Detailed review checklist and severity classification |
| `references/adversarial-protocol.md` | Mindset rules, rejection format, scoring mechanics |

---

## Tech Stacks Library

The Dev-Design SkillSet includes a `tech-stacks` library holding 14 stack overlay templates. The expected install layout is a sibling skills root containing `architect/`, `designer/`, `engineer/`, and `tech-stacks/`, so direct skill references resolve as `../tech-stacks/<overlay>.md`.

Ownership is explicit:
- `architect` locks the backend/runtime overlay
- `designer` locks the frontend overlay
- `engineer` inherits both locks and may not silently substitute stacks

| Backend Templates | Frontend Templates |
|---|---|
| `node-typescript.md` (Express/Fastify) | `react-nextjs.md` (Next.js 15) |
| `bun-typescript.md` (Elysia/Hono) | `react-tanstack.md` (TanStack Router/Query) |
| `deno-typescript.md` (Fresh/Oak) | `vue-nuxt.md` (Vue 3 + Nuxt 3) |
| `python-fastapi.md` (FastAPI) | `angular.md` (Angular 19+ Signals) |
| `rust-axum.md` (Rust + Axum) | `astro.md` (Astro 5 Islands) |
| `go-gin.md` (Go + Gin/Chi) | `vite-spa.md` (Vite 8 baseline) |
| `dotnet-aspnet.md` (.NET 9 + EF Core) | `svelte-sveltekit.md` (Svelte 5 + SvelteKit) |

---

## How the Skills Differ

| Aspect | Researcher | Planner | Architect | Designer | Engineer | Gatekeeper-Design |
|---|---|---|---|---|---|---|
| **Output** | SRS & Domain Map | Project Roadmap & Risks | System Design & ADRs | UI/UX & Components | Repo Setup & DevOps | Meta-Review Report |
| **Focus** | "What do we build?" | "When & how safely?" | "How is it structured?" | "How does it look/feel?"| "How is it coded/deployed?"| "Is this output valid?" |
| **Mindset** | Exploratory/Analytic | Strategic/Precautionary | Structural/Patterns | Creative/User-Centric | Pragmatic/Ops-Focused | Adversarial/Skeptical |
| **Key Standard**| ISO 29148 / DDD | 12-factor / Agile | C4 / Arc42 | WCAG / Core Web Vitals | OWASP / OpenTelemetry | Custom Constraint Matrix |

---

## How the Skills Connect

```text
USER REQUEST
  ↓
COMMANDER
  ↓
RESEARCHER → gatekeeper-design → commander
  ↓
PLANNER → gatekeeper-design → commander
  ↓
ARCHITECT + Backend Stack Lock → gatekeeper-design → commander
  ↓
DESIGNER + Frontend Stack Lock → gatekeeper-design → commander
  ↓
ENGINEER + Inherited Stack Locks → gatekeeper-design → commander
  ↓
FINAL DESIGN PACKAGE
```

**Connection points:**
1. **Phased Delegation:** `commander` orchestrates the sequential workflow.
2. **Context Passing:** The `researcher`'s domain model and captured tech constraints define boundaries that later phases must follow.
3. **Template Locking:** `architect` locks the backend overlay from the sibling `tech-stacks/` library, `designer` locks the frontend overlay, and `engineer` inherits both.
4. **Adversarial Handoffs:** Every handoff is blocked directly by `gatekeeper-design`. In pipeline mode, `commander` owns that review cycle and cannot advance to phase N+1 until approval is recorded.

---

## Step-by-Step Usage

> **Activation note:** The canonical entry point is `commander`. In agentic frameworks that auto-route installed skills, prompt that entry point directly. In assistants that do not auto-route skills, reference `commander/SKILL.md` or another specific skill file explicitly as a fallback.

Use the plain skill name in prompts by default. Some environments also expose installed skills as slash commands such as `/commander`.

### Running the Full Autonomous Workflow (Agentic Frameworks)

1. **Install the inner skill folders** in your configured skills directory.
2. **Open your AI coding agent** (Codex, Kilo Code, or OpenCode).
3. **Request the `commander` entry point:**
   ```
   "Use commander to design a new serverless booking system"
   ```
4. The `commander` skill runs the full phase order: `researcher -> planner -> architect -> designer -> engineer` as applicable.
5. Each specialist returns a deliverable and review packet to `commander`.
6. `commander` submits each phase to `gatekeeper-design` behind the scenes and manages approval loops.
7. You receive a fully specified, architecturally sound design package with backend/frontend stack locks.

### Running a Single Skill

You can bypass the `commander` and target a specific discipline directly. In that standalone mode, the specialist may self-run `gatekeeper-design` for its own deliverable.

1. **Provide context (if required by your agent):** Explicitly attach or reference the relevant `SKILL.md` file (e.g., `@workspace`, or dragging the file into chat).
2. **Ask for a specific design task** using natural language:
   - `"Read researcher/SKILL.md and map bounded contexts for an e-commerce platform"`
   - `"Read architect/SKILL.md and generate a C4 diagram for my current codebase"`
   - `"Read gatekeeper-design/SKILL.md and validate the attached SRS document"`
   - `"Read designer/SKILL.md and use the sibling tech-stacks/react-nextjs.md overlay as the frontend reference for this design"`

---

## Installation Guide

Each skill relies on a `SKILL.md` file (the main instructions) and a `references/` directory. To install them, copy the **inner skill folders** from `Dev-Design_SkillSet/` into the skills directory your agent is configured to read. Copy `tech-stacks/` as a sibling folder as well when you want the 14 stack overlay templates available to the design flow.

This README assumes you are working from the combined AI SkillSets repository. If you are using a separately published mirror of this SkillSet, the same install rule applies; only the surrounding repo path changes.

### Canonical Install Rule

Copy these folders, not the top-level `Dev-Design_SkillSet/` directory:

- `commander/`
- `researcher/`
- `planner/`
- `architect/`
- `designer/`
- `engineer/`
- `gatekeeper-design/`
- `tech-stacks/`

Use `.agents/skills/` when your environment supports a shared skills directory, or the configured equivalent for your assistant.

Keep them as siblings under the same skills root so references like `references/...`
and `../tech-stacks/<overlay>.md` resolve correctly.

### Standard Assistants (Claude Code and GitHub Copilot)

These assistants do **not** always auto-route to installed skill files. Keep the same copied folders in an accessible location, then point the assistant at the right entry point or fallback `SKILL.md` file.

#### Claude Code
Claude Code does not use a guaranteed native skills directory, so the important part is keeping the copied folders somewhere your project instructions can reference consistently.

1. Copy the installed Dev-Design skill folders into a location your workspace can reference, such as `.agents/skills/` or `.claude/skills/`.
2. Add a reference in your **`CLAUDE.md`**:
   ```markdown
   ## Design Skills
   This project uses the Dev-Design SkillSet. Start design work with the commander entry point. If Claude does not auto-route the installed skill, read `.agents/skills/commander/SKILL.md` first.
   ```

#### GitHub Copilot
GitHub Copilot does not guarantee automatic routing from a custom skills folder.

1. Copy the installed Dev-Design skill folders into a location your repository instructions can reference.
2. Reference them in **`.github/copilot-instructions.md`**:
   ```markdown
   ## Software Design Spec
   When asked to design new features or architectures, start with commander. If Copilot does not auto-route the installed skill, use `@workspace` and follow `.agents/skills/commander/SKILL.md`.
   ```

### Agentic Frameworks (Codex, Kilo Code, OpenCode)

If you are using a dedicated agentic runner:
1. Create the environment's skills directory, typically `.agents/skills/` or the framework-specific equivalent.
2. Copy the individual folders (`commander`, `researcher`, and the rest) into that directory, plus the sibling `tech-stacks/` folder when you want the template library available.
3. Prompt the `commander` entry point directly.
4. If the framework does not auto-route as expected, fall back to `commander/SKILL.md`.

---

## Directory Structure

```text
Dev-Design_SkillSet/
├── README.md                          # This file
├── QUICK-START.md                     # Brief onboarding guide
├── commander/                         # Central Orchestrator
│   ├── SKILL.md
│   └── references/
├── researcher/                        # Requirements & Domain Specialist
│   ├── SKILL.md
│   └── references/
├── planner/                           # Project Planning Specialist
│   ├── SKILL.md
│   └── references/
├── architect/                         # System Architecture Specialist
│   ├── SKILL.md
│   └── references/
├── designer/                          # UI/UX Architecture Specialist
│   ├── SKILL.md
│   └── references/
│       ├── design-system-template.md
│       ├── frontend-patterns.md
│       ├── layout-patterns.md
│       ├── styling-decision-matrix.md
│       ├── ui-ux-standards.md
│       ├── visual-design-guide.md
│       └── page-templates/            # 19 page-template resources (1 selection guide + 18 concrete templates)
├── engineer/                          # Implementation & DevOps Specialist
│   ├── SKILL.md
│   └── references/
├── gatekeeper-design/                  # Adversarial Validator
│   ├── SKILL.md
│   └── references/
└── tech-stacks/                       # Tech stack overlay templates
    ├── angular.md
    ├── astro.md
    ├── bun-typescript.md
    ├── deno-typescript.md
    ├── dotnet-aspnet.md
    ├── go-gin.md
    ├── node-typescript.md
    ├── python-fastapi.md
    ├── react-nextjs.md
    ├── react-tanstack.md
    ├── rust-axum.md
    ├── svelte-sveltekit.md
    ├── vite-spa.md
    └── vue-nuxt.md
```

---

## Contributing

Contributions are welcome! To improve templates or add tech stacks:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/add-svelte-stack`)
3. Make your changes
4. Submit a pull request

For questions or suggestions, open an issue or discussion in the repository where you found this SkillSet.

---

<p align="center">
  <sub>Built by <a href="https://github.com/TykoDev">TykoDev</a> · Dev-Design SkillSet</sub>
</p>
