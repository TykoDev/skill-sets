---
name: cross-check-build-confirm
description: >-
  This skill should be used when the user or build-management asks to
  "scan for incomplete code", "check for TODOs", "find placeholder code",
  "verify no scaffold remains", "confirm implementation completeness",
  "check for fake data", "scan for mockups", "verify no temporary code",
  "ensure nothing was missed", "final completeness check", "confirm
  build is production-ready", "check for stubs", "find leftover
  scaffolding", or "verify all features are implemented". It is the
  final quality gate that scans the entire codebase for scaffold code,
  TODOs, placeholders, mockups, fake data, temporary implementations,
  and incomplete features — delegating all findings back to
  bob-the-builder until the codebase is clean.
version: 1.0.0
---

# Cross-Check Build Confirm — Implementation Completeness Scanner

## Purpose

Cross-Check Build Confirm is the final quality gate in the Build Team SkillSet
pipeline. It performs a systematic, exhaustive scan of the entire built codebase
to detect any evidence of incomplete, temporary, or placeholder code. This skill
does not fix issues — it finds them and delegates every finding back to
bob-the-builder through build-management until the codebase is production-clean.

Every finding is non-negotiable at the BLOCKER level. Placeholder code does not ship.

## Core Principle

> "If it is not production-ready, it does not ship. Every placeholder, TODO,
> and mock is a broken promise to the user. Temporary code has a permanent
> tendency to remain."

---

## Scan Methodology

### Step 1: Static Pattern Scan

Scan the entire codebase for comment markers and keywords that indicate incomplete work:

**Primary markers** (case-insensitive search):

| Pattern | Severity | Rationale |
|---------|----------|-----------|
| `TODO` | BLOCKER | Explicitly marks unfinished work |
| `FIXME` | BLOCKER | Explicitly marks known defects |
| `HACK` | BLOCKER | Explicitly marks shortcuts that need proper solutions |
| `XXX` | BLOCKER | Marks dangerous or problematic code |
| `PLACEHOLDER` | BLOCKER | Explicitly temporary content |
| `STUB` | BLOCKER | Empty or minimal implementation stand-in |
| `MOCK` (in production paths) | BLOCKER | Mock data or behavior in non-test code |
| `TEMP` / `TEMPORARY` | WARNING | Potentially intentional but verify |
| `DUMMY` | WARNING | Potentially test-only but verify |
| `FAKE` | WARNING | Potentially test-only but verify |
| `SAMPLE` | WARNING | May be intentional example data |
| `NOT IMPLEMENTED` | BLOCKER | Explicitly incomplete |
| `COMING SOON` | BLOCKER | Feature promised but not built |

**Exclusions**: Patterns in test files, documentation, and comments explaining WHY something was NOT done (as opposed to markers for work yet to be done) may be acceptable. Verify context before classifying.

### Step 2: Structural Completeness Scan

Compare the implemented codebase against the original design specification:

1. **Module inventory**: List every module specified in the design — verify each has a corresponding directory with implementation files
2. **Feature inventory**: List every feature or user story — verify each has corresponding code paths
3. **API endpoint inventory**: List every endpoint in the API contract — verify each has a route handler with complete logic
4. **Database entity inventory**: List every entity in the data model — verify each has model, migration, and repository

**Output format:**

| Specified Item | Expected Location | Found | Status |
|---------------|------------------|-------|--------|
| User registration | src/user/routes.ts | Yes | Complete |
| Password reset | src/auth/routes.ts | No | MISSING |
| Order history | src/order/routes.ts | Partial | Stub only |

### Step 3: Behavioral Completeness Scan

Examine code for patterns that indicate unfinished behavior:

| Pattern | Detection Method | Severity |
|---------|-----------------|----------|
| Functions returning hardcoded values | Find `return "..."` / `return 0` / `return null` where computed values expected | BLOCKER |
| Empty function bodies | Find functions with no logic (just `pass`, `{}`, `return`) | BLOCKER |
| `throw new Error("Not implemented")` | Literal search for "not implemented" in throw/raise statements | BLOCKER |
| Console debugging statements | Find `console.log`, `print()`, `fmt.Println` used for debugging (not structured logging) | WARNING |
| Commented-out code blocks | Find multi-line comments containing code syntax | WARNING |
| Empty catch/except blocks | Find `catch` or `except` with empty body or just `pass` | BLOCKER |
| Unreachable code after early returns | Find code after unconditional return/throw statements | WARNING |

### Step 4: Data Completeness Scan

Scan for placeholder data that should not appear in production:

| Pattern | Example | Severity |
|---------|---------|----------|
| Lorem ipsum text | `"Lorem ipsum dolor sit amet"` | BLOCKER |
| Example domains | `example.com`, `test.com`, `foo.bar` in non-test code | BLOCKER |
| Example emails | `test@test.com`, `user@example.com` in production config | BLOCKER |
| Sequential placeholder IDs | `12345`, `99999`, `000-000-0000` as hardcoded values | WARNING |
| Placeholder names | `"John Doe"`, `"Jane Smith"`, `"Acme Corp"` in production data | WARNING |
| Hardcoded localhost | `127.0.0.1`, `localhost` in production configuration | BLOCKER |
| Placeholder URLs | `https://api.example.com` in production config | BLOCKER |

### Step 5: Configuration Completeness Scan

Verify configuration is production-ready:

| Check | Method | Severity |
|-------|--------|----------|
| `.env.example` exists | File search | BLOCKER if missing |
| All required env vars documented | Compare code references to .env.example | BLOCKER if gaps |
| No hardcoded URLs in source | Search for `http://` and `https://` outside config files | WARNING |
| Debug mode disabled | Search for `DEBUG=true`, `NODE_ENV=development` in committed configs | BLOCKER |
| Default secrets replaced | Search for `secret`, `password`, `changeme`, `default` in config values | BLOCKER |

### Step 6: Documentation Completeness Scan

Verify documentation is not placeholder:

| Pattern | Detection | Severity |
|---------|-----------|----------|
| `[INSERT HERE]`, `[TBD]`, `[TODO]` | Literal search in .md files | BLOCKER |
| Empty README sections | Find H2/H3 headers followed immediately by another header or end-of-file | WARNING |
| Template text unmodified | Compare against known template phrases | WARNING |
| Missing API documentation | Endpoints without JSDoc/docstring | INFO |

---

## Severity Classification

| Level | Definition | Required Action |
|-------|-----------|----------------|
| **BLOCKER** | Scaffold, placeholder, or incomplete code in production paths. This code would reach end users or affect production behavior. | MUST be resolved. Delegate to bob-the-builder. Blocks CLEAN verdict. |
| **WARNING** | Scaffold code in non-critical paths, or patterns that may be intentional but require verification. | SHOULD be resolved. Delegate to bob-the-builder with context. Blocks CLEAN verdict. |
| **INFO** | Style-only issues or minor documentation gaps that do not affect production behavior. | MAY be resolved. Does not block CLEAN verdict. Document and report. |

---

## Delegation Protocol

### Finding Packaging

Package all findings for routing to bob-the-builder through build-management:

```markdown
## COMPLETENESS FINDINGS PACKAGE

### Scan Summary
- BLOCKERs: [count]
- WARNINGs: [count]
- INFOs: [count]
- Verdict: [FINDINGS | CLEAN]

### BLOCKER Findings
| ID | Category | File:Line | Pattern Found | Required Action |
|----|----------|-----------|--------------|----------------|
| CCC-001 | Static Pattern | src/api/handler.ts:15 | `// TODO: implement error handling` | Implement complete error handling |
| CCC-002 | Behavioral | src/order/service.ts:42 | `return []` (hardcoded empty array) | Implement real order fetching logic |
| CCC-003 | Data | src/config/api.ts:8 | `https://api.example.com` | Replace with environment variable |

### WARNING Findings
| ID | Category | File:Line | Pattern Found | Recommended Action |
|----|----------|-----------|--------------|-------------------|
| CCC-010 | Behavioral | src/utils/logger.ts:5 | `console.log(...)` | Replace with structured logging |

### INFO Findings
| ID | Category | File:Line | Pattern Found | Suggestion |
|----|----------|-----------|--------------|-----------|
| CCC-020 | Documentation | README.md:45 | Empty "Deployment" section | Add deployment instructions |
```

### Re-Scan Cycle

After bob-the-builder addresses findings:

1. Receive the updated codebase from build-management
2. Re-run the FULL 6-step scan (not just the previously found items — new issues may have been introduced during fixes)
3. Issue updated verdict
4. **Maximum 2 full scan cycles** — if BLOCKERs persist after 2 cycles, escalate to user through build-management

---

## Acceptance Criteria

### CLEAN Verdict Requirements

A CLEAN verdict requires ALL of the following:

- **Zero BLOCKERs**: No scaffold, placeholder, or incomplete code in any production path
- **Zero WARNINGs**: All warnings resolved or explicitly justified
- **INFOs acceptable**: Informational items may remain with documentation
- **Structural completeness**: All modules, features, and endpoints from the design specification have corresponding implementations
- **Configuration completeness**: All required environment variables documented, no hardcoded secrets or URLs

### FINDINGS Verdict

Issued when any BLOCKER or WARNING exists. Includes the complete findings package for delegation to bob-the-builder.

---

## Output Format

Structure the completeness scan report as follows:

```markdown
## Completeness Scan Report

### Scan Metadata
- **Scan cycle**: [1 | 2]
- **Codebase scope**: [total files scanned]
- **Design spec reference**: [document name]
- **Verdict**: [CLEAN | FINDINGS]

### Scan Results
| Category | BLOCKERs | WARNINGs | INFOs |
|----------|----------|----------|-------|
| Static Pattern | [N] | [N] | [N] |
| Structural Completeness | [N] | [N] | [N] |
| Behavioral Completeness | [N] | [N] | [N] |
| Data Completeness | [N] | [N] | [N] |
| Configuration Completeness | [N] | [N] | [N] |
| Documentation Completeness | [N] | [N] | [N] |
| **Total** | **[N]** | **[N]** | **[N]** |

### Feature Completeness Matrix
| Feature | Specified | Implemented | Status |
|---------|----------|------------|--------|
| [Feature 1] | Yes | Yes | Complete |
| [Feature 2] | Yes | Partial | BLOCKER — stub implementation |
| [Feature 3] | Yes | No | BLOCKER — missing entirely |

### Detailed Findings
[Full findings package as specified in Delegation Protocol]

### Verdict Justification
[Explain why CLEAN or FINDINGS, with specific evidence]
```

---

## Additional Resources

### Reference Files

For detailed detection patterns and completeness checklists:
- **`references/scaffold-detection.md`** — Exhaustive pattern catalog organized by category (comment markers, code patterns, data patterns, structural patterns, configuration patterns) with language-specific regex patterns, false positive guidance, and detection confidence levels
- **`references/completeness-checklist.md`** — Feature completeness matrix template, module completeness checklist, API endpoint completeness verification, database migration completeness, configuration completeness, documentation completeness, CLEAN verdict criteria, and re-scan protocol procedures
