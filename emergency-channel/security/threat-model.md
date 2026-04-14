---
owner: "Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Threat Model

## Purpose
Define the threat model for the Emergency Channel: identify assets, threat actors, attack surfaces, potential risks, mitigations, and priorities to continuously reduce risk during design, implementation, and operations, and to guide detection and response.

## Scope
Applies to:
- All Emergency Channel components (ingest, sanitizer, core, router, distributor, publisher, storage)
- Supporting infrastructure (KMS, CI/CD, configuration management, monitoring, network)
- People and processes (developers, operators, oncall, third‑party integrations)
- Boundaries with external systems (APIs, message queues, external storage)

---

## Key Assets
- **User submissions**: raw inputs, including text or media that may contain sensitive information.
- **Decryption and processing keys**: KMS keys and credentials used for decryption or signing.
- **Audit and forensic logs**: raw logs and snapshots used for investigation and compliance.
- **Routing and metadata**: metadata and policy configurations that determine message flow.
- **Services and infrastructure**: runtime instances, container images, configurations, CI/CD pipelines.
- **Third‑party integration credentials**: API keys, webhook secrets, and external service credentials.

---

## Threat Actors
- **External attackers**: internet adversaries, automated scanners, targeted threat actors (APTs).
- **Compromised or malicious insiders**: stolen developer/operator credentials or abused service accounts.
- **Supply chain threats**: tampered libraries, images, CI plugins, or external services.
- **Malicious or abusive legitimate users**: crafted submissions to bypass sanitizer or abuse resources.
- **Human error and misconfiguration**: overly permissive permissions, incorrect routing rules, unsafe defaults.

---

## Attack Surface
- **Network interfaces**: public APIs, admin consoles, and internal service communication.
- **Input processing pipeline**: ingest → sanitizer → core parsing and transformation logic.
- **Key management and crypto flows**: KMS calls, key rotation, export and backup paths.
- **Logging and monitoring exports**: aggregation and export channels to third parties or long‑term stores.
- **CI/CD and image distribution**: build pipelines, image registries, deployment scripts.
- **Configuration and metadata stores**: feature flags, routing tables, and policy configuration read/write interfaces.

---

## Example Threat Scenarios

### 1. Data Exfiltration (External Breach or Insider Abuse)
**Path**: Attacker exploits an unpatched vulnerability or stolen credentials to access processing nodes or storage and export raw submissions or decrypted data.  
**Impact**: Sensitive data exposure, compliance violations, legal and reputational damage.  
**Mitigations**:
- Minimize retention of sensitive data in logs and storage; redact or avoid logging sensitive fields.
- Enforce KMS usage, key rotation, and least privilege access controls.
- Network segmentation and strict internal/external separation; MFA and anomalous login detection.

### 2. Key or Credential Compromise
**Path**: CI/CD secrets leak, developer credential theft, or misuse of KMS administrative operations.  
**Impact**: Data decryption, forged signatures, lateral movement.  
**Mitigations**:
- Use HSM-backed KMS where possible and enforce multi‑party approval for critical key operations; prohibit key export.
- Use short‑lived tokens and secret managers for CI/CD; audit all secret access.
- Require multi‑party approval and just‑in‑time elevation for sensitive operations.

### 3. Input Injection and Sanitizer Bypass
**Path**: Crafted submissions bypass sanitizer logic, causing downstream services to execute unexpected operations or leak data.  
**Impact**: Remote code execution, data tampering, service disruption.  
**Mitigations**:
- Multi‑layer defenses: input validation, content classification, sandboxed processing, and behavioral detection.
- Fuzz testing and security reviews for sanitizer components; require security tests on changes.
- Limit permissions and output channels for processing results to reduce blast radius.

### 4. Log Tampering or Forensic Destruction
**Path**: Attacker or misconfiguration deletes or alters logs, hindering investigation.  
**Impact**: Loss of audit trail, regulatory and legal exposure.  
**Mitigations**:
- Use immutable or signed audit storage (WORM); enforce strict RBAC for log write/delete.
- Audit queries and exports; require approvals for sensitive queries.
- Regularly back up and verify log integrity.

### 5. Supply Chain Compromise (Tampered Images or Dependencies)
**Path**: Malicious dependency or contaminated container image enters CI/CD and is deployed to production.  
**Impact**: Backdoors, data theft, persistent access.  
**Mitigations**:
- Image signing and verification, SBOM generation, dependency auditing, and minimal third‑party usage.
- Integrate dependency and image scanning into CI/CD.
- Restrict build and deploy permissions and use trusted build environments.

---

## Risk Assessment and Prioritization
- **High priority (immediate)**: key/credential compromise, raw data exfiltration, remote code execution vulnerabilities.  
- **Medium priority (short term)**: improper log exports, misconfigurations leading to excessive permissions, sanitizer coverage gaps.  
- **Low priority (ongoing)**: monitoring UX improvements, low‑impact DoS vectors.

Prioritization should consider data sensitivity, user impact, compliance requirements, and exploitability.

---

## Mitigation Controls

### Architecture and Design
- Apply least privilege across services, KMS, storage, and CI/CD.
- Network segmentation, zero trust principles, and strong service‑to‑service authentication and authorization.
- Isolate sensitive processing into controlled environments with limited external visibility.

### Encryption and Key Management
- Encrypt data at rest and in transit; manage master keys via KMS with enforced rotation policies.
- Prohibit plaintext keys or long‑lived credentials in logs or configuration.
- Require multi‑party approval and audit for critical KMS operations.

### Input Handling and Content Safety
- Multi‑stage input validation and content classification; sandbox high‑risk content and escalate to manual review when needed.
- Regular fuzzing and adversarial testing of sanitizer and parsers.
- Restrict outbound channels and rate limits for processed outputs.

### Observability and Detection
- Maintain an auditable chain of custody with immutable logs and query auditing.
- Detect anomalous behavior (unusual key usage, bulk exports, abnormal access patterns).
- Integrate alerts with incident response playbooks and automated containment where appropriate.

### Supply Chain and CI/CD
- Enforce image signing, SBOM generation, dependency scanning, and trusted build environments.
- Minimize third‑party plugins and external dependencies; audit critical dependencies.
- Gate deployments with automated security checks and manual approvals for high‑risk changes.

### People and Process
- Regular security training, periodic permission reviews, and offboarding procedures.
- Multi‑party approvals and just‑in‑time elevation for sensitive operations.
- Regular incident response drills and postmortem-driven improvements.

---

## Assumptions and Limitations
- Assumes KMS provides strong cryptographic guarantees and auditing; if not, compensating controls are required.  
- Assumes monitoring and logging cover critical paths; gaps must be prioritized.  
- Risk assessment is based on current architecture and known dependencies; new features or integrations require re‑evaluation.

---

## Testing and Validation Plan
- Periodic threat model reviews (on architecture changes or quarterly).  
- Penetration testing and fuzzing of critical components at least annually or after major changes.  
- Integrate security gates in CI/CD (SAST, dependency scanning, container scanning).  
- Conduct tabletop and live exercises to validate detection, containment, and forensics.

---

## Outputs and Governance
- Map high‑risk scenarios to measurable mitigation goals and owners.  
- Track remediation items in a backlog and verify completion.  
- Reference the threat model in architecture decisions and PR reviews to ensure risk acceptance criteria are met.

---

## References
- `access-control.md`  
- `audit-and-logging.md`  
- `key-management.md`  
- `incident-response.md`

---

## Summary
By identifying key assets, threat actors, and attack surfaces, and by defining layered mitigations and testing plans, this Threat Model provides a continuous risk management framework for the Emergency Channel to support secure design, detection, and response.
