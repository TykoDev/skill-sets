# Code-Check SkillSet: Quick Start

Use this SkillSet when you want systematic, multi-dimensional code review with adversarial validation.

## 1. Pick This SkillSet

Choose **Code-Check_SkillSet** when you want comprehensive, multi-dimensional code review with adversarial validation.

**Start with `code-chief`** for full-pipeline reviews covering all dimensions. Use individual skills for targeted reviews.

| Skill | Use When You Need |
|-------|-------------------|
| `code-chief` | Full pipeline review (orchestrates all skills below) |
| `bug-review` | Correctness defect hunting |
| `code-review` | Holistic merge-readiness assessment |
| `quality-review` | Maintainability, standards, and architecture drift |
| `security-review` | Exploitability, compliance, and threat modeling |
| `mr-robot` | Adversarial penetration testing and exploit chain construction |
| `frontier` | Frontend performance, accessibility, security, and UI/UX audit |

`gatekeeper-code` validates all review reports — it runs automatically in the pipeline.

## 2. Copy the Inner Skill Folders

Copy the skill folders from `Code-Check_SkillSet/` into your agent's skills directory (usually `.agents/skills/`).

Folders to copy:

- `code-chief/` — Pipeline orchestrator
- `bug-review/` — Phase 1
- `code-review/` — Phase 2
- `quality-review/` — Phase 3
- `security-review/` — Phase 4
- `mr-robot/` — Phase 5
- `frontier/` — Phase 6
- `gatekeeper-code/` — Adversarial validator

Do **not** copy the top-level `Code-Check_SkillSet/` folder itself.

## 3. Start With the Entry Point

The canonical entry point is `code-chief`.

In the canonical pipeline, `gatekeeper-code` validates the consolidated review
package automatically.

Use the plain skill name in prompts by default. Some environments also expose
installed skills as slash commands such as `/code-chief`.

Example prompts:

```text
Use code-chief to run a full review of this codebase.

Use code-chief to review this PR with the full Code-Check SkillSet.

Use security-review to check this service for vulnerabilities.
```

For focused analysis, you can still invoke individual skills directly, such as
`bug-review`, `quality-review`, `mr-robot`, or `frontier`.

## 4. Fallback

If your assistant does not auto-route installed skills, reference the skill file directly:

```text
Read code-chief/SKILL.md and run a full code review on this project.
```
