# Code-Check SkillSet: Quick Start

A suite of 5 adversarial code review skills that integrate with your AI coding assistant. 

## 1. Installation

Instead of installing software, you provide these folders as **context** to your AI agent so it learns how to perform specialized code reviews.

1. Clone or download this repository.
2. Copy the skill folders into your project workspace (e.g., `skills/`).
3. **Tell your AI where to look:**
   - **Claude Code:** Add to `CLAUDE.md`: *"Read `skills/[skill]/SKILL.md` before performing reviews."*
   - **GitHub Copilot:** Add to `.github/copilot-instructions.md`: *"Use `@workspace` and read `skills/[skill]/SKILL.md` before reviewing."*
   - **Agentic Frameworks (Codex, Kilo):** Drop them into `.agents/skills/` (or the specific configured folder) for auto-routing.

## 2. Usage

To trigger a review, ensure your AI reads the instructions (`SKILL.md`) for that specific skill.  

**Prompting Examples:**
* *"Read `skills/bug-review/SKILL.md` and find bugs in this file."*
* *"Read `skills/security-review/SKILL.md` and check this code for vulnerabilities."*
* *"Using the `quality-review` skillset, assess the maintainability of this PR."*
* *"Read `skills/gatekeeper-code/SKILL.md` and validate these review findings."*

*(If using an Agentic Framework that natively auto-routes skills based on folder structure, you can simply ask naturally: "Find bugs in this code").*

### The 5 Skills
1. **bug-review:** Finds crashes, data corruption, and correctness defects.
2. **code-review:** Assesses overall PR merge-readiness across 8 dimensions.
3. **quality-review:** Detects tech debt, architectural drift, and efficiency issues.
4. **security-review:** Maps vulnerabilities to OWASP, CWE, and checks exploitability.
5. **gatekeeper-code:** Adversarial meta-reviewer that challenges other reports for accuracy before you see them.
