---
owner: "Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Security CI Checks

## Purpose
Define the automated CI checks and gating rules that enforce security, privacy, and telemetry constraints for the Emergency Channel codebase. These checks prevent accidental leakage of secrets or high‑cardinality telemetry, ensure safe logging and key usage, and catch risky changes to sanitizers, parsers, and deployment pipelines before merge.

## Scope
Applies to:
- All repository pull requests that modify code, configuration, CI pipelines, or documentation in the Emergency Channel directories.
- Build and release pipelines that produce container images, artifacts, or deployment manifests.
- Any automation that exports logs, telemetry, or schema changes to external systems.

---

## Principles
- **Fail fast**: Detect risky changes early in PRs and block merges until issues are resolved.  
- **Actionable feedback**: Provide clear, prescriptive error messages and remediation steps.  
- **Least privilege**: Checks should enforce minimal exposure of secrets, keys, and high‑cardinality identifiers.  
- **Automated enforcement with human review**: Combine automated gates with required human approvals for high‑risk changes.

---

## Required CI Checks

### 1. Secret Detection
- **What**: Scan diffs and built artifacts for secrets, tokens, private keys, and credentials.  
- **How**: Use multiple detectors (regex, entropy, known secret patterns, and vendor scanners).  
- **Action**: Fail the PR on detection; require secret removal and rotation if leaked in history.  
- **Notes**: Exemptions require documented justification and a temporary mitigation plan.

### 2. High Cardinality Field Detection
- **What**: Identify new log or metric fields added by code or schema changes that introduce high‑cardinality labels (user IDs, IPs, raw submission hashes).  
- **How**: Static analysis of logging calls and schema diffs; compare against allowed field whitelist.  
- **Action**: Block PRs that add prohibited high‑cardinality fields; require design doc or privacy review for borderline cases.

### 3. Logging and Telemetry Policy Check
- **What**: Validate that logging calls follow the audit-and-logging policy (no raw content, tokenization/hashing of identifiers, controlled vocabularies).  
- **How**: Linting rules for logging APIs and schema validators for telemetry exports.  
- **Action**: Fail PRs that log raw submission content, secrets, or internal topology.

### 4. Sanitizer and Parser Safety Tests
- **What**: Run unit tests, fuzz tests, and adversarial input suites against sanitizer and parser changes.  
- **How**: CI executes predefined fuzz corpus and security test harnesses; require coverage thresholds for sanitizer logic.  
- **Action**: Block merges on failing tests; require security review for changes that reduce coverage.

### 5. Dependency and Image Scanning
- **What**: Scan dependencies and container images for known vulnerabilities and supply chain issues.  
- **How**: SBOM generation, SCA tools, image vulnerability scanners, and signature verification.  
- **Action**: Fail PRs that introduce critical or high severity vulnerabilities; require mitigation plan for medium severity.

### 6. Infrastructure as Code and Deployment Checks
- **What**: Lint IaC for insecure configurations (open network ACLs, public S3 buckets, permissive IAM roles).  
- **How**: Static analysis tools for Terraform/CloudFormation/Kubernetes manifests.  
- **Action**: Block PRs with insecure defaults; require remediation or exception with documented risk acceptance.

### 7. Key Management and KMS Operation Safeguards
- **What**: Detect code or config changes that alter KMS usage patterns, key IDs, or export behaviors.  
- **How**: Policy checks in CI that flag changes touching KMS bindings, key rotation scripts, or backup/export logic.  
- **Action**: Require multi‑party approval and security review for changes that affect master keys or key export paths.

### 8. Audit and Query Export Checks
- **What**: Validate that any code enabling log exports or privileged query endpoints includes anonymization and export approvals.  
- **How**: CI checks for export flags, anonymization functions, and presence of approval metadata.  
- **Action**: Block exports without documented approval and automated anonymization.

### 9. Schema Change Review
- **What**: Gate changes to public or internal metadata schemas, routing rules, and event types.  
- **How**: Schema diff tooling that highlights cardinality, new fields, and backward compatibility issues.  
- **Action**: Require schema owner signoff and a migration plan for breaking changes.

### 10. PR Size and Risk Heuristics
- **What**: Flag very large PRs or PRs that touch many subsystems as high risk.  
- **How**: Heuristics based on file count, directories touched, and change types.  
- **Action**: Require additional reviewers and a security checklist completion before merge.

---

## Enforcement and Approval Workflow

- **Blocking gates**: Secret detection, high‑cardinality detection, sanitizer tests, and key management checks are blocking by default.  
- **Human approvals**: Changes that affect KMS, audit exports, or schema must include explicit approvals from designated owners (owner fields in front‑matter).  
- **Just‑in‑time elevation**: For emergency fixes, allow temporary bypass with documented justification, timebound approval, and post‑merge audit.  
- **Audit trail**: Record CI failures, approvals, and bypasses in an auditable log tied to the PR.

---

## Developer Guidance and Remediation Steps

- **When a secret is detected**: Remove the secret, rotate credentials, and follow the secret incident playbook. Use secret manager references instead of inline secrets.  
- **When a high‑cardinality field is flagged**: Replace with hashed or tokenized identifiers; consult audit-and-logging policy for acceptable transformations.  
- **When sanitizer tests fail**: Reproduce locally with the fuzz corpus, add test cases for the failing input, and request a security review for complex fixes.  
- **When dependency issues appear**: Upgrade or patch dependencies; if not possible, document compensating controls and timeline for remediation.

---

## CI Implementation Notes

- **Fail fast in PRs**: Run quick, lightweight checks first (linting, secret scan), then run heavier tests (fuzzing, image scans) in parallel.  
- **Caching and incremental checks**: Use caching to speed repeated runs and only re-scan changed artifacts where possible.  
- **Clear error messages**: CI must return prescriptive remediation steps and links to relevant policies and playbooks.  
- **Visibility**: Surface CI failures in PR status and in team channels for rapid triage.

---

## Metrics and Continuous Improvement

- **Key metrics**: number of blocked PRs by check type, mean time to fix CI failures, number of bypasses and their justification, false positive rate of secret detection.  
- **Review cadence**: Quarterly review of CI rules, thresholds, and false positives with security and developer teams.  
- **Feedback loop**: Maintain a feedback channel for developers to report noisy checks and request rule adjustments.

---

## PR Template Snippets

**Security Checklist**
```
- [ ] No secrets or credentials in diff
- [ ] Logging changes reviewed for PII and cardinality
- [ ] Sanitizer/parser tests added or updated
- [ ] Dependency changes scanned and approved
- [ ] Schema changes approved by owner
```

**Bypass Justification Template**
```
Reason for bypass:
Risk mitigation and timeline:
Approver(s):
Expiration time:
```

---

## References
- `audit-and-logging.md`  
- `key-management.md`  
- `incident-response.md`  
- `security-ci-playbooks.md` (implementation playbooks and CI job definitions)

---

## Summary
Security CI checks are the first line of defense against accidental data exposure, insecure configurations, and supply chain risks. Enforce blocking gates for secrets, high‑cardinality telemetry, sanitizer regressions, and key management changes; provide clear remediation guidance; and maintain an auditable approval process for exceptions to keep the Emergency Channel secure and compliant.
