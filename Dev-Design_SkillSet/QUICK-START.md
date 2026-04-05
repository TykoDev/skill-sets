# Dev-Design SkillSet: Quick Start

A suite of 7 specialized AI domain skills and a rich library of tech-stack patterns that orchestrate standard AI coding assistants into an autonomous software design team.

## 1. Installation

Instead of installing software, you provide these folders as **context** to your AI agent so it learns how to autonomously coordinate and perform detailed design tasks.

1. Clone or download this repository.
2. Copy the skill folders into your project workspace (e.g., `skills/`).
3. **Tell your AI where to look:**
   - **Claude Code:** Add to `CLAUDE.md`: *"Read `skills/commander/SKILL.md` to initiate design workflows."*
   - **GitHub Copilot:** Add to `.github/copilot-instructions.md`: *"Use `@workspace` and read `skills/commander/SKILL.md` for architectural design tasks."*
   - **Agentic Frameworks (Codex, Kilo):** Drop them into `.agents/skills/` (or the specific configured folder) for auto-routing.

## 2. Usage

To trigger the full design workflow, start with the orchestrator (`commander`). Alternatively, target a single skill for specific tasks.

**Prompting Examples:**
* *"Read `skills/commander/SKILL.md` and design an architecture for a real-time chat application."*
* *"Read `skills/architect/SKILL.md` and generate a C4 diagram for my current login system."*
* *"Using the `researcher` skillset, map the bounded contexts of my platform."*
* *"Read `skills/gatekeeper-design/SKILL.md` and ruthlessly validate this SRS document."*

*(If using an Agentic Framework that natively auto-routes skills based on folder structure, you can simply ask naturally: "Use the commander to design my app").*

### The Skills
1. **commander:** Phased orchestrator that delegates tasks and compiles the final package.
2. **researcher:** Outlines SRS requirements and models the domain boundaries (DDD).
3. **planner:** Creates project roadmaps, feature milestones, and risk registers.
4. **architect:** Establishes Arc42 architecture, backend patterns, API contracts, and C4 models.
5. **designer:** Designs component logic, state management, and accessibility layers (WCAG).
6. **engineer:** Dictates DevOps pipelines, repository folder structures, and implementation rules.
7. **gatekeeper-design:** Adversarial meta-agent that evaluates other skills' outputs, rejecting any sloppy work.
*(Includes `tech-stacks/` — a library of 14 specific frontend and backend standards).*
