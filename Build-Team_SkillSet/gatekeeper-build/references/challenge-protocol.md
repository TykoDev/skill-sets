# Challenge Protocol Reference

## Challenge Framework

The gatekeeper applies 5 challenge categories across 7 review dimensions. Each challenge targets a specific type of quality failure in the deliverable.

---

## Challenge Category 1: Existence

### Definition

Verify that claimed implementations, findings, or test coverage actually exist in the codebase.

### Trigger Conditions

- The change manifest lists files that should be verified
- Test coverage claims specific percentages
- Security findings reference specific file locations
- Implementation claims to address specific requirements

### Application by Phase

**Phase 1 (bob-the-builder — code):**

| Dimension | Existence Check |
|-----------|----------------|
| Spec Alignment | Verify each requirement in the traceability matrix has corresponding code at the stated location |
| Completeness | Verify every file in the change manifest exists and contains the described functionality |
| Correctness | Verify function signatures match documented APIs |

**Phase 2 (test-builder — tests):**

| Dimension | Existence Check |
|-----------|----------------|
| Testing | Verify each listed test file exists and contains real test functions with assertions |
| Completeness | Verify claimed coverage numbers by checking test scope against code paths |

**Phase 3 (security-builder — audit):**

| Dimension | Existence Check |
|-----------|----------------|
| Security | Verify referenced vulnerable code exists at stated file:line locations |
| Accuracy | Verify the described vulnerability pattern actually matches the code at that location |

### Example Challenge

```
CHALLENGE: Existence
Target: bob-the-builder — Phase 1 Implementation
Finding: Change manifest claims "src/order/order.service.ts" implements order
         total calculation with discount logic.
Question: Does src/order/order.service.ts contain a function that calculates
          order totals with discount application? Verify the function exists
          and contains actual calculation logic (not a placeholder return).
Evidence Required: The function signature and key logic lines from the file.
```

### Resolution Criteria

- **Resolved**: The cited code, test, or finding exists as described
- **Corrected**: The manifest/report is updated to match reality
- **Withdrawn**: The claim is retracted

---

## Challenge Category 2: Accuracy

### Definition

Verify that implementations are logically correct and classifications are properly assigned.

### Trigger Conditions

- Algorithm implementations where correctness can be verified
- Validation logic that claims to enforce specific rules
- Security findings mapped to specific CWE IDs
- Test assertions that claim to verify specific behavior

### Application by Phase

**Phase 1 (bob-the-builder — code):**

| Dimension | Accuracy Check |
|-----------|---------------|
| Correctness | Verify algorithm logic produces correct results for known inputs |
| Code Quality | Verify error handling actually catches the described error conditions |
| Security | Verify input validation actually rejects the described invalid inputs |

**Phase 2 (test-builder — tests):**

| Dimension | Accuracy Check |
|-----------|---------------|
| Testing | Verify test assertions match the expected behavior (not just "does not throw") |
| Correctness | Verify edge case tests actually test the claimed edge case |

**Phase 3 (security-builder — audit):**

| Dimension | Accuracy Check |
|-----------|---------------|
| Security | Verify CWE mapping is correct — does the code pattern match the CWE definition? |
| Accuracy | Verify CVSS scoring components match the actual exploit scenario |

### Example Challenge

```
CHALLENGE: Accuracy
Target: test-builder — Phase 2 Tests
Finding: Test "validateEmail_withInvalidFormat_throwsError" claims to test
         email validation with invalid format.
Question: Does the test actually pass an invalidly formatted email? Does
          the assertion verify the specific error type and message, or just
          that any error was thrown?
Evidence Required: The full test function body showing the input value and
                   assertion specificity.
```

---

## Challenge Category 3: Completeness

### Definition

Verify that the deliverable covers the full scope defined by the design specification and the skill's own methodology.

### Trigger Conditions

- Design specification has more items than the implementation's traceability matrix
- Test suite does not cover all documented error conditions
- Security audit skipped OWASP categories without justification
- Code modules are missing from the implementation

### Application by Phase

**Phase 1 (bob-the-builder — code):**

| Dimension | Completeness Check |
|-----------|-------------------|
| Spec Alignment | Every requirement in the design spec has corresponding implementation |
| Completeness | All API endpoints defined in the contract are implemented |
| Documentation | All public APIs have documentation |

**Phase 2 (test-builder — tests):**

| Dimension | Completeness Check |
|-----------|-------------------|
| Testing | All critical paths have tests; all error conditions documented in code are tested |
| Completeness | Test inventory covers all modules with public APIs |

**Phase 3 (security-builder — audit):**

| Dimension | Completeness Check |
|-----------|-------------------|
| Security | All OWASP Top 10 categories assessed (or explicitly justified as not applicable) |
| Completeness | All input entry points traced through to sinks |

### Example Challenge

```
CHALLENGE: Completeness
Target: bob-the-builder — Phase 1 Implementation
Finding: Design spec defines 8 API endpoints. Change manifest lists implementations
         for 6 endpoints. Two endpoints appear missing.
Question: Where are the implementations for GET /api/orders/:id/history and
          POST /api/orders/:id/refund? Were they intentionally deferred or
          accidentally omitted?
Evidence Required: Either the file locations of the missing implementations
                   or explicit justification for deferral with build-management approval.
```

---

## Challenge Category 4: Proportionality

### Definition

Verify that quality assessments, severity ratings, and effort allocation are calibrated to actual impact.

### Trigger Conditions

- Severity ratings that seem inflated or deflated
- Test coverage that emphasizes trivial code over critical paths
- Security findings with mismatched CVSS scores
- Disproportionate effort on low-risk areas while high-risk areas are under-covered

### Application Examples

| Scenario | Challenge |
|----------|----------|
| Critical severity on a style issue | Deflate: This is Minor at most |
| Low severity on SQL injection in user-facing endpoint | Inflate: This should be Critical |
| 95% test coverage on utility functions, 20% on auth module | Disproportionate: Critical paths under-tested |
| Security finding requiring physical access rated as High | Inflate: Remote exploitability is Low |

### Resolution Criteria

- **Upheld**: The severity is adjusted to match actual impact
- **Defended**: The original rating is justified with additional evidence
- **Withdrawn**: The challenge is retracted (original rating was correct)

---

## Challenge Category 5: Consistency

### Definition

Verify internal coherence within a deliverable and across deliverables within the same build pipeline.

### Trigger Conditions

- Contradictions between the change manifest and actual code
- Tests that reference functions not present in the implementation
- Security remediation claims that conflict with actual code state
- Different parts of the implementation using inconsistent patterns

### Application Examples

| Inconsistency | Challenge |
|---------------|----------|
| Manifest says file created, but file contains only imports | Existence + Consistency: implementation is incomplete |
| Tests pass but test wrong behavior (testing outdated API) | Accuracy + Consistency: tests out of sync with code |
| Security fix claimed but vulnerable pattern still present | Existence + Consistency: remediation incomplete |
| Error handling varies between modules (some throw, some return null) | Code Quality: inconsistent patterns across codebase |

---

## Challenge Rate Guidelines

| Finding Severity | Existence | Accuracy | Completeness | Proportionality | Consistency |
|-----------------|-----------|----------|-------------|----------------|-------------|
| Critical | 100% | 100% | 100% | 100% | 100% |
| Major | 100% | 50% | 100% | 50% | 100% |
| Minor | 30% | 20% | — | 20% | 30% |

---

## Resolution Tracking

Track all challenges and their resolutions:

```markdown
## Challenge Resolution Ledger

| Challenge ID | Category | Target | Finding | Round | Resolution | Status |
|-------------|----------|--------|---------|-------|------------|--------|
| CH-001 | Existence | bob-the-builder | Missing order history endpoint | 1 | Corrected — endpoint added | Resolved |
| CH-002 | Accuracy | test-builder | Test assertion too broad | 1 | Defended — assertion covers behavior | Accepted |
| CH-003 | Completeness | security-builder | A04 not assessed | 1 | Corrected — assessment added | Resolved |
| CH-004 | Proportionality | bob-the-builder | Critical on logging issue | 1 | Upheld — downgraded to Minor | Resolved |
```
