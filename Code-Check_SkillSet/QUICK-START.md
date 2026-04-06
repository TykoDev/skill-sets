# Code-Check SkillSet: Quick Start

Use this SkillSet when you want targeted code review. Unlike the other two SkillSets, there is no single orchestrator entry point for ordinary review work.

## 1. Pick This SkillSet

Choose **Code-Check_SkillSet** when you want:

- `bug-review` for correctness defects
- `code-review` for overall merge-readiness
- `quality-review` for maintainability and architecture drift
- `security-review` for exploitability and security findings

`gatekeeper-code` validates review reports, but it is **not** the default starting point for ordinary reviews.

## 2. Copy the Inner Skill Folders

Copy the actual skill folders from `Code-Check_SkillSet/` into your agent's skills directory, usually `.agents/skills/` or the configured equivalent.

Do **not** copy the top-level `Code-Check_SkillSet/` folder itself.

Typical folders to copy:

- `bug-review/`
- `code-review/`
- `quality-review/`
- `security-review/`
- `gatekeeper-code/`

## 3. Start With the Right Review Skill

Start with the review skill that matches the job.

Example prompts:

```text
Run bug-review on this codebase.

Review this PR with code-review.

Use security-review to check this service for vulnerabilities.
```

If your assistant does not auto-route installed skills, reference the fallback file directly:

```text
Read bug-review/SKILL.md and find bugs in this file.
```
