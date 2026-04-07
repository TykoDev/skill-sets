# Build-Team SkillSet: Quick Start

Use this SkillSet when you already have an approved plan or design package and want the implementation pipeline to build, test, audit, and complete the codebase.

## 1. Pick This SkillSet

Choose **Build-Team_SkillSet** when you want:

- `build-management` to orchestrate the full build pipeline
- `bob-the-builder` for direct implementation work
- `test-builder`, `security-builder`, or `cross-check-build-confirm` for focused follow-up tasks

The canonical workflow is strict:

`build-management -> bob-the-builder -> gatekeeper-build -> test-builder -> gatekeeper-build -> security-builder -> gatekeeper-build -> cross-check-build-confirm -> gatekeeper-build -> delivery`

Direct specialist invocation is supported for manual, out-of-band work, but it is
not the canonical fully gated pipeline.

## 2. Copy the Inner Skill Folders

Copy the actual skill folders from `Build-Team_SkillSet/` into your agent's skills directory, usually `.agents/skills/` or the configured equivalent.

Do **not** copy the top-level `Build-Team_SkillSet/` folder itself.

Typical folders to copy:

- `build-management/`
- `bob-the-builder/`
- `test-builder/`
- `security-builder/`
- `gatekeeper-build/`
- `cross-check-build-confirm/`

## 3. Start With the Entry Point

The canonical entry point is `build-management`.

In the canonical pipeline, every specialist output is reviewed by
`gatekeeper-build`, including the final `cross-check-build-confirm` report.

Use the plain skill name in prompts by default. Some environments also expose
installed skills as slash commands such as `/build-management`.

Example prompts:

```text
Use build-management to implement this approved design specification.

Use build-management to implement this plan, then validate completeness before delivery.

Use bob-the-builder to implement this feature directly; skip the full gated pipeline for this task.
```

## 4. Fallback

If your assistant does not auto-route installed skills, reference the fallback file directly:

```text
Read build-management/SKILL.md and implement this approved design specification.
```
