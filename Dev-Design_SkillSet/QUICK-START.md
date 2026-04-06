# Dev-Design SkillSet: Quick Start

Use this SkillSet when you want to turn an idea, feature request, or rough requirements into a structured design package.

## 1. Pick This SkillSet

Choose **Dev-Design_SkillSet** when you want:

- `commander` to orchestrate the full design workflow
- `researcher`, `planner`, `architect`, `designer`, or `engineer` for focused design tasks
- `tech-stacks/` guidance to anchor the plan to one of the 14 supported stack templates

## 2. Copy the Inner Skill Folders

Copy the actual skill folders from `Dev-Design_SkillSet/` into your agent's skills directory, usually `.agents/skills/` or the configured equivalent.

Do **not** copy the top-level `Dev-Design_SkillSet/` folder itself.

Typical folders to copy:

- `commander/`
- `researcher/`
- `planner/`
- `architect/`
- `designer/`
- `engineer/`
- `gatekeeper-design/`
- `tech-stacks/`

## 3. Start With the Entry Point

The canonical entry point is `commander`.

Example prompts:

```text
Use /commander to turn this product idea into a build-ready plan.

Use commander to design a new serverless booking system.

Start the design workflow for this feature request.
```

If your assistant does not auto-route installed skills, reference the fallback file directly:

```text
Read commander/SKILL.md and design this application.
```
