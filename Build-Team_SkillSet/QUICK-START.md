# Build-Team SkillSet: Quick Start

Use this SkillSet when you already have an approved plan or design package and want the implementation pipeline to build, test, audit, and complete the codebase.

## 1. Pick This SkillSet

Choose **Build-Team_SkillSet** when you want:

- `build-management` to orchestrate the full build pipeline
- `bob-the-builder` for direct implementation work
- `test-builder`, `security-builder`, or `cross-check-build-confirm` for focused follow-up tasks

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

Example prompts:

```text
Use /build-management to implement this approved design specification.

Build this project from the design spec.

Use build-management to implement this plan, then validate completeness before delivery.
```

If your assistant does not auto-route installed skills, reference the fallback file directly:

```text
Read build-management/SKILL.md and implement this approved design specification.
```
