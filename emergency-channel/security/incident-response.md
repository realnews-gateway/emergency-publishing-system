---
owner: "Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Incident Response

## Purpose
Define the incident response process, roles and responsibilities, evidence preservation requirements, communication and disclosure guidelines, and post‑incident remediation for the Emergency Channel. The goal is to detect, contain, recover, and learn from incidents quickly while preserving chain of custody and meeting compliance obligations.

## Scope
Applies to:
- All services and infrastructure in production, pre‑production, and development environments
- Personnel: oncall, service-operator, service-admin, key-admin, Empus/security
- Automation: CI/CD, monitoring and alerting, audit logging systems
- External communications and legal/compliance processes related to incidents

---

## Roles and Responsibilities

- **Incident Commander**  
  Owns overall incident decisions, priorities, resource allocation, and external communication strategy. Typically a senior member designated by Empus/security.

- **Responder**  
  Executes technical containment, evidence collection, recovery actions, and temporary mitigations. Includes service-operator, service-admin, and developers.

- **Forensics Lead**  
  Responsible for evidence preservation, immutable snapshots, chain‑of‑custody, and coordination with legal/compliance.

- **Communications Lead**  
  Manages internal notifications, drafts external statements, and coordinates media or customer communications as needed.

- **Key Admin**  
  Handles KMS and key operations during containment and recovery, following multi‑party approval processes.

- **Postmortem Owner**  
  Coordinates the postmortem, tracks remediation items, and verifies fixes.

---

## Incident Severity and Triggers

- **P0 Critical**: Impacts critical production paths, data exfiltration, key or credential compromise, prolonged outage, or regulatory breach. Triggers immediate full response and high‑priority notifications.  
- **P1 High**: Impacts a critical service or large user base; requires rapid containment and recovery.  
- **P2 Medium**: Localized degradation or configuration issues that are recoverable; handled during business hours.  
- **P3 Low**: Non‑critical issues, monitoring noise, or low‑impact events; handled via normal operations.

Triggers and thresholds should be defined in monitoring rules and documented in oncall runbooks.

---

## High‑Level Response Flow

1. **Detect and Confirm**  
   - On alert, oncall or automation should validate the alert.  
   - Record initial timestamp, alert source, scope of impact, and preliminary evidence (logs, metrics, request IDs).

2. **Triage and Assemble**  
   - Classify severity (P0–P3).  
   - Assemble required roles (Incident Commander, responders, forensics lead, communications lead).  
   - Use controlled communication channels for coordination.

3. **Containment**  
   - Apply short‑term mitigations to stop further damage (isolate instances, revoke temporary credentials, block network paths, disable affected features).  
   - Record every containment action: actor, time, rationale, and rollback plan.

4. **Forensics and Evidence Preservation**  
   - Collect memory snapshots, disk images, packet captures, relevant logs, and configuration snapshots without destroying evidence.  
   - Store evidence in immutable storage or signed snapshots and record chain‑of‑custody metadata.  
   - If keys or credentials are involved, Key Admin must follow multi‑party approval procedures.

5. **Root Cause Analysis and Remediation**  
   - Identify root cause and implement fixes (patches, configuration changes, tightened permissions, credential rotation).  
   - Validate fixes with tests (integration, regression, security scans).  
   - Document all remediation steps and follow change control.

6. **Recovery**  
   - Restore services in phases, prioritizing critical paths.  
   - Monitor for regressions and residual risk.

7. **Communication and Disclosure**  
   - Internal notifications: inform affected teams, leadership, and legal/compliance.  
   - External disclosure: Communications Lead and Legal decide on customer or public statements; approvals and records must be kept.  
   - Avoid sharing unverified details or forensic artifacts externally.

8. **Postmortem**  
   - Produce an initial postmortem draft within 7 days including timeline, impact, root cause, remediation, and open items.  
   - Hold a review and publish a final report within 30 days with an actionable remediation plan and owners.  
   - Track remediation to closure and record verification evidence.

---

## Forensics and Evidence Preservation Guidelines

- **Preserve First**: Collect critical evidence (logs, snapshots, memory) before performing actions that could destroy it.  
- **Immutable Storage**: Store forensic artifacts in access‑controlled immutable or WORM storage.  
- **Signing and Verification**: Sign or checksum snapshots and record verification metadata to ensure integrity.  
- **Least Exposure**: Redact or restrict sensitive fields in forensic data; access requires approval and justification.  
- **Legal Considerations**: Notify legal when incidents may involve litigation or regulatory obligations and follow legal preservation guidance.

---

## Key and Credential Incidents

- **Immediate Rotation**: If a key or long‑lived credential is confirmed compromised, rotate affected keys and revoke old credentials per Key Admin procedures.  
- **Multi‑Party Approval**: Deletion, export, or rotation of master KMS keys requires multi‑party approval and full documentation.  
- **Impact Assessment**: Evaluate how credential compromise affects data decryption, service access, and third‑party integrations; plan remediation accordingly.  
- **Audit**: Record all key operations and forward logs to controlled audit storage.

---

## Communication Templates

**Internal Quick Notification**
```
Severity: P0
Scope: Service X in prod is unavailable
First detected: 2026-04-15T02:12Z
Current status: Isolated affected instances; collecting forensics
Next steps: Rotate affected credentials and evaluate recovery plan
Contact: Incident Commander - Alice (Empus/security)
```

**External Statement Template (legal approval required)**
```
We have detected an incident affecting [service]. Containment measures are in place and an investigation is underway. We are assessing the impact and will provide updates as they become available. We take customer data security seriously and will publish a detailed report after the investigation.
```

---

## Postmortem and Continuous Improvement

- **Postmortem Contents**: timeline, root cause, impact, remediation and mitigations, open risks, action items with owners and due dates.  
- **Metrics**: track MTTR, detection-to-response time, false positive rate, and forensic completeness.  
- **Action Closure**: assign owners and deadlines for remediation; verify fixes and record evidence before closing items.  
- **Knowledge Base**: update playbooks, detection rules, and runbooks with lessons learned.

---

## Drills and Validation

- Conduct regular tabletop and live exercises (at least annually) including scenarios for credential compromise, key misuse, data exfiltration, and service outages.  
- Produce improvement lists from exercises and incorporate them into governance and playbooks.

---

## References
- `access-control.md`  
- `audit-and-logging.md`  
- `key-management.md`  
- `incident-response-playbooks.md`

---

## Summary
Maintain clear severity definitions, rapid assembly and containment procedures, strict evidence preservation and key handling rules, controlled communications, and a closed‑loop postmortem process to ensure incidents are handled quickly, audibly, and in compliance with legal and regulatory requirements.
