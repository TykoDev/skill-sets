# Dev-Design SkillSet: Quick Start

Use this SkillSet when you want to turn an idea, feature request, or rough requirements into a structured design package.

## 1. Pick This SkillSet

Choose **Dev-Design_SkillSet** when you want:

- `commander` to orchestrate the full design workflow and own the gatekeeper-design review cycle in pipeline mode
- `researcher`, `planner`, `architect`, `designer`, or `engineer` for focused design tasks
- the sibling `tech-stacks/` library to anchor the design to one of the 14 supported stack templates

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

Keep these folders as siblings under the same skills root. That layout is what
allows skill references such as `references/...` and `../tech-stacks/<overlay>.md`
to resolve correctly.

## 3. Start With the Entry Point

The canonical entry point is `commander`.

Use the plain skill name in prompts by default. Some environments also expose
installed skills as slash commands such as `/commander`.

In the full pipeline, the phase order is:

```text
researcher -> planner -> architect -> designer -> engineer
```

`architect` locks the backend/runtime overlay, `designer` locks the frontend
overlay, and `engineer` inherits both.

When you reference `tech-stacks/*.md`, treat those files as overlay references
for `architect`, `designer`, or `engineer`; they are not standalone skill
entry points.

Example prompts:

```text
Use commander to turn this product idea into a build-ready plan.

Use commander to design a new serverless booking system.

Start the design workflow for this feature request.
```

If you run a specialist directly instead of `commander`, that skill can operate
in standalone mode and self-run `gatekeeper-design` for its own deliverable.

## 4. Fallback

If your assistant does not auto-route installed skills, reference the fallback file directly:

```text
Read commander/SKILL.md and design this application.
```
