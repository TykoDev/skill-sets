---
name: Security Review Specialist
description: >
  This skill should be used when the user asks to "review security",
  "check for vulnerabilities", "do a security audit", "review for OWASP
  issues", "check authentication code", "review access control",
  "assess supply chain security", "check for secrets in code",
  "review cryptography usage", "evaluate threat model alignment",
  "check for injection vulnerabilities", "review for prompt injection",
  "assess AI security", or "check compliance with NIST SSDF". It performs
  thorough, systematic security analysis using NIST SSDF, OWASP ASVS,
  CWE Top 25, and threat modeling integration with risk-tiered review depth.
---

# Security Review Specialist

## Purpose

This skill performs dedicated security analysis focused exclusively on exploitability, security requirements compliance, and threat mitigation. It is distinct from bug-review (which targets correctness defects) and code-review (which applies a holistic assessment). The sole objective is to identify code that can be exploited by adversaries, violates security requirements, or introduces vulnerabilities into the software supply chain. All findings are mapped to established security frameworks and weakness taxonomies.

## Frameworks Alignment

Anchor every security review to the applicable frameworks. Select the frameworks relevant to the project's regulatory and risk context.

**NIST SSDF v1.1 (SP 800-218).** The definitive secure development practice framework. Practice PW.7 specifically mandates reviewing and analyzing human-readable code to identify vulnerabilities and verify compliance with security requirements, calling out both human review and automated analysis tools. Mandatory for all US federal software suppliers under Executive Order 14028.

**OWASP ASVS 5.0.0.** Granular technical control requirements for web applications, microservices, and APIs. Three compliance levels: Level 1 (basic hygiene for low-risk apps), Level 2 (standard for most business applications handling personal/financial data), Level 3 (extreme rigor for mission-critical infrastructure). Use ASVS controls as the review checklist baseline.

**OWASP Top 10:2025.** The eighth installment, analyzing 2.8 million+ applications. Includes Software Supply Chain Failures as a standalone category and first-ever guidance on AI-generated code security risks. Cross-Site Scripting holds #1 (score 60.38), Missing Authorization surged to #4.

**CWE Top 25 (2025).** Based on analysis of 39,080 CVEs. Use as the weakness taxonomy for classifying findings. Memory safety issues (buffer overflows) returned prominently, reinforcing the case for memory-safe languages.

**Microsoft Continuous SDL.** Expanded under the Secure Future Initiative to address AI-specific threats: training data poisoning, prompt injection, and non-deterministic outputs.

**EU AI Act / RAD-AI.** For systems deploying AI in high-risk contexts, RAD-AI extends standard documentation to capture probabilistic behaviors, data-dependent evolution, and dual ML/software lifecycles. Increases EU AI Act Annex IV addressability from ~36% to 93%.

Consult `references/frameworks.md` for detailed framework descriptions, comparison tables, and compliance mapping anchors (SSDF to SP 800-53, CWE/SANS, MITRE ATT&CK).

## Threat Modeling Integration

Treat threat modeling as a code review input, not a separate once-per-project activity.

**When to Trigger.** Create or update a lightweight threat model whenever a PR introduces: a new data flow, a trust boundary change, authentication or authorization modifications, a new external dependency, or API surface changes.

**STRIDE Application.** Classify threats using STRIDE categories: Spoofing (identity), Tampering (data integrity), Repudiation (deniability), Information Disclosure (confidentiality), Denial of Service (availability), Elevation of Privilege (authorization). Map each identified threat to mitigations in the code.

**ATT&CK Cross-Reference.** Enrich the threat model with MITRE ATT&CK real-world adversary tactics and techniques. Focus on CI/CD compromise, credential abuse, and supply-chain attack patterns relevant to the change.

**Validation During Review.** For each PR with threat model implications:
1. Verify that trust boundaries and entry points affected by the change are identified
2. Confirm that mitigations for each identified threat are implemented in the code
3. Validate that security tests cover the threat scenarios
4. For High-risk changes, require security specialist approval enforced via CODEOWNERS

## Security Review Workflow

Apply a risk-tiered approach to determine review depth.

**Step 1: Risk Tier Assessment.** Classify the change based on what is touched:
- **Low:** UI styling, documentation, test-only changes, non-sensitive configuration
- **Medium:** Business logic, data processing, non-auth API endpoints, internal tooling
- **High:** Authentication/authorization, payment/financial logic, cryptographic operations, deserialization of untrusted data, CI/CD pipeline configuration, new external dependencies, AI/ML model integration

**Step 2: Automated Gate Verification.** Confirm that CI/CD automated security gates have executed and review their results:
- SAST findings (CodeQL, Semgrep) — review any new alerts, verify suppressions are justified
- SCA findings — check for vulnerable dependencies, verify patch availability
- Secrets detection — confirm no new secrets detected, verify push protection is active
- Normalize results to SARIF format for unified triage where possible

**Step 3: Apply Secure Coding Checklist.** Systematically evaluate the changed code against the secure coding checklist (see summary below and full checklist in `references/secure-coding-checklist.md`).

**Step 4: Threat Model Alignment (Medium/High Tier).** For Medium and High risk changes, validate that the code change aligns with the threat model. Verify mitigations for identified threats are present and tested.

**Step 5: AI-Specific Threat Assessment.** If the change involves AI/ML components, evaluate for:
- **Prompt Injection:** Can external user inputs manipulate AI system instructions or hijack workflows?
- **Data/Model Poisoning:** Is training data integrity validated? Can dependency chains corrupt model outputs?
- **Excessive Agency:** Are AI agent permissions scoped with strict RBAC? Can agents escalate privileges?
- **Shadow AI:** Are ungoverned data flows or unauthorized AI tools creating compliance blind spots?
- **PII Leakage:** Can sensitive data leak through system prompts, telemetry, or unauthorized channels?

**Step 6: Supply Chain Evaluation.** For changes that add or update dependencies:
- Verify SBOM generation is configured (CycloneDX or SPDX)
- Run SCA with reachability analysis (is the vulnerable code actually called?)
- Check transitive dependency chains for known vulnerabilities
- Validate dependency provenance and integrity (SLSA alignment, signatures)
- Consult `references/supply-chain.md` for detailed supply chain gate procedures

## Secure Coding Checklist Summary

Apply these checks to every security-relevant code change. The full checklist with detailed sub-items is in `references/secure-coding-checklist.md`.

**Input Validation.** All inputs validated for type, length, format on the server side. Character sets specified (UTF-8). Validation failures result in immediate rejection. Data sources classified as trusted or untrusted.

**Output Encoding.** Context-specific encoding applied: HTML entity encoding for web output, parameterized queries for SQL, command escaping for OS commands. No raw user input rendered in templates.

**Authentication.** MFA enforced. OAuth2/OIDC for inter-service communication. No embedded passwords. Password storage uses bcrypt or Argon2. Proper token rotation and secure cookie handling.

**Session Management.** Secure session identifiers, proper expiration, session fixation prevention, secure cookie attributes (Secure, HttpOnly, SameSite).

**Access Control.** Default deny policies. Least privilege enforcement. No direct object references without authorization checks. Prevent lateral movement through strict RBAC.

**Error Handling.** No sensitive information in error responses (no stack traces, session IDs, architecture details to end users). Generic custom error pages. Security controls fail securely (deny by default on error).

**Cryptography.** Approved algorithms only (no MD5, no SHA-1 for security purposes, no custom crypto). Proper key management. Keys never hardcoded.

**Logging.** No sensitive data in logs (no passwords, tokens, PII). Tamper-resistant logging on isolated systems. Sufficient context for incident investigation.

## Output Format

Structure the security review report as follows:

```
## Security Review Report
### Summary
- **Risk Tier:** Low | Medium | High
- **Frameworks Applied:** [list]
- **Total Findings:** [count] (Critical: [n], High: [n], Medium: [n], Low: [n])
- **Supply Chain Status:** Clean | Issues Found

### Findings
#### [SEC-001] [CWE-XXX] — [Brief Title]
- **Severity:** Critical | High | Medium | Low (CVSS v4.0 score: [X.X])
- **Location:** file:line
- **Description:** [Vulnerability description and exploit scenario]
- **Evidence:** [Code path, attack vector, proof of exploitability]
- **Remediation:** [Specific fix with secure alternative]
- **Framework Reference:** [OWASP Top 10 category, CWE ID, ASVS control]

### Threat Model Assessment
- [Threat model delta for this change, or "Not applicable — Low risk tier"]

### Supply Chain Assessment
- [Dependency changes, SBOM status, SCA results, provenance verification]

### Tooling Gaps
- [Missing automated security checks that should be added]

### Checklist Coverage
- [Which checklist categories were applied, which were not applicable]
```

## Handoff to Gatekeeper

After completing the security review report, submit it to the `gatekeeper-code` skill for adversarial validation. If no `gatekeeper-code` skill is available, self-validate by confirming each finding has a verifiable code path, correct CWE mapping, and justified CVSS score. The gatekeeper-code applies especially rigorous scrutiny to security findings: Are CWE mappings accurate? Are CVSS scores justified by the actual exploit scenario? Were all relevant OWASP categories checked? Were AI-specific threats considered for code touching AI components? Were supply chain implications evaluated for dependency changes? For High-risk tier reviews, load the full checklist from `references/secure-coding-checklist.md`. For Low-risk tier reviews, the summary in this file is sufficient.

## Additional Resources

### Reference Files

For detailed frameworks, checklists, and supply chain procedures, consult:

- **`references/frameworks.md`** — NIST SSDF v1.1 (all four practice groups, PW.7 deep dive), OWASP ASVS 5.0.0 (levels and controls), OWASP SAMM v2 (maturity streams), OWASP Top 10:2025, CWE Top 25, Microsoft Continuous SDL, EU AI Act/RAD-AI, framework comparison tables, and compliance mapping anchors
- **`references/secure-coding-checklist.md`** — Complete secure coding review checklist covering input validation, output encoding, authentication, session management, access control, error handling, cryptography, logging, and AI-specific threats with detailed sub-items and verification procedures
- **`references/supply-chain.md`** — SBOM generation (CycloneDX v1.7, SPDX v3), SCA tools (Snyk, Dependency-Check, Dependabot), SLSA provenance levels and attestations, in-toto framework, Sigstore/cosign signing, secrets detection (TruffleHog v3, Gitleaks, GitHub Secret Scanning), and end-to-end supply chain gate implementation
