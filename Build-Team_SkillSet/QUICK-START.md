# Build-Team SkillSet: Quick Start

A suite of 6 specialized AI skills that orchestrate standard AI coding assistants into an autonomous code implementation team — from design specs to production-ready, tested, security-audited code.

## 1. Installation

Instead of installing software, you provide these folders as **context** to your AI agent so it learns how to autonomously coordinate and perform implementation tasks.

1. Clone or download this repository.
2. Copy the skill folders into your project workspace (e.g., `skills/`).
3. **Tell your AI where to look:**
   - **Claude Code:** Add to `CLAUDE.md`: *"Read `skills/build-management/SKILL.md` to initiate the build pipeline."*
   - **GitHub Copilot:** Add to `.github/copilot-instructions.md`: *"Use `@workspace` and read `skills/build-management/SKILL.md` for implementation tasks."*
   - **Agentic Frameworks (Codex, Kilo):** Drop them into `.agents/skills/` (or the specific configured folder) for auto-routing.

## 2. Usage

To trigger the full build pipeline, start with the orchestrator (`build-management`). Alternatively, target a single skill for specific tasks.

**Prompting Examples:**
* *"Read `skills/build-management/SKILL.md` and implement this design specification for a REST API."*
* *"Read `skills/bob-the-builder/SKILL.md` and implement the user authentication module."*
* *"Read `skills/test-builder/SKILL.md` and write comprehensive tests for the order service."*
* *"Read `skills/security-builder/SKILL.md` and audit this codebase for vulnerabilities."*
* *"Read `skills/cross-check-build-confirm/SKILL.md` and verify no placeholder code remains."*

*(If using an Agentic Framework that natively auto-routes skills based on folder structure, you can simply ask naturally: "Build this project from the design spec").*

### The 6 Skills
1. **build-management:** Pipeline orchestrator that delegates phases and compiles the final build package.
2. **bob-the-builder:** Senior development engineer that writes production-ready code from design specs.
3. **test-builder:** Test engineering specialist that creates comprehensive, meaningful test suites.
4. **security-builder:** Code security auditor that maps vulnerabilities to OWASP, CWE, and NIST standards.
5. **gatekeeper-build:** Adversarial meta-reviewer that challenges every phase output before the pipeline advances.
6. **cross-check-build-confirm:** Completeness scanner that ensures zero TODOs, placeholders, or scaffold code ship to production.
