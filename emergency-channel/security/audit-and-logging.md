---
owner: "Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Audit and Logging

## Purpose
Define what to log, retention, anonymization, export rules, and access controls for audit and operational logs produced by the Emergency Channel. This document prescribes minimum logging requirements to support detection, investigation, compliance, and post‑incident forensics while minimizing privacy and correlation risks.

## Scope
Applies to:
- All services in the Emergency Channel pipeline (ingest, sanitizer, core, router, distributor, publisher, storage)
- Supporting infrastructure (CI/CD, orchestration, KMS, secrets manager)
- Human‑facing consoles and administrative tooling
- Telemetry and metric exports derived from logs

## Principles
- **Minimal necessary fidelity**: Log the data required for detection and response, and no more.
- **Protect sensitive content**: Never log raw submission content or secrets.
- **Non‑reversible identifiers**: Use hashed or tokenized identifiers for correlation outside internal stores.
- **Tamper resistance**: Ensure logs are stored and transmitted in a way that supports integrity verification.
- **Access control**: Restrict access to raw logs to authorized roles and require approvals for sensitive queries.
- **Retention and privacy**: Retain logs only as long as necessary for operational and compliance needs.

---

## Log Categories

- **Operational logs**  
  Service lifecycle events, resource usage, scaling, deployments, and health checks.

- **Security logs**  
  Authentication attempts, authorization decisions, key management operations, policy changes, and break‑glass events.

- **Processing logs**  
  Submission lifecycle events (ingest accepted/rejected, sanitizer actions, routing decisions) with sensitive fields removed or tokenized.

- **Audit logs**  
  Administrative actions, configuration changes, RBAC modifications, and privileged operations.

- **Telemetry and metrics**  
  Aggregated counters, histograms, and low‑cardinality labels for dashboards and alerting.

---

## Sensitive Data Handling

- **Never log raw submission content**. Content must be treated as sensitive and only stored in encrypted storage with strict access controls.
- **Identifiers**: Replace user identifiers, IP addresses, and device identifiers with non‑reversible hashes (e.g., `sha256(id)`), or use ephemeral correlation tokens that expire.
- **Secrets**: Never log secrets, private keys, tokens, or unredacted credentials.
- **Contextual fields**: Strip or normalize fields that reveal internal topology, hostnames, or internal IDs before logs leave internal stores.

---

## Log Structure and Minimum Fields

Every log entry should include the following minimal fields where applicable:

- **timestamp**: ISO8601 UTC
- **service**: logical service name (low‑cardinality)
- **environment**: prod/stage/dev
- **event_type**: standardized event name
- **actor_role**: role or service identity (not raw user id)
- **resource**: resource type or logical identifier (tokenized or hashed)
- **result**: success/failure/partial
- **context**: small, low‑cardinality context blob (avoid high‑cardinality values)

Use controlled vocabularies for `service`, `event_type`, `actor_role`, and `result`. Document enumerations centrally.

---

## Retention and Storage

- **High‑resolution operational logs**: retain for **30 days**.
- **Aggregated telemetry and metrics**: retain for **90 days**.
- **Audit logs (privileged actions, key operations, policy changes)**: retain for **1 year** or longer where required by compliance.
- **Forensic snapshots**: preserve per incident response playbooks; move to long‑term, access‑controlled storage if required for legal or regulatory reasons.

Retention windows must be configurable per environment and justified in policy. Implement automated retention enforcement.

---

## Export, Access, and Querying

- **Export rules**: Exports to external systems must be aggregated and anonymized. Do not export raw logs containing hashed identifiers unless the recipient is authorized and the export is approved.
- **Access controls**: Access to raw logs requires RBAC checks and, for sensitive queries, multi‑party approval. Use just‑in‑time elevation for temporary access.
- **Query auditing**: Log all privileged queries and exports, including requester, justification, and query parameters (with sensitive values redacted).
- **Data minimization**: When responding to operational requests, prefer aggregated views or sampled data rather than full raw logs.

---

## Alerting and Detection

- Define alerts for security‑relevant patterns, including:
  - Repeated authentication failures
  - Mass key unwraps or key access spikes
  - Unexpected policy or RBAC changes
  - Sudden increases in rejected submissions or sanitizer errors
  - Large volumes of data exports or log downloads
- Tune alert thresholds to balance noise and detection sensitivity.
- Ensure alerts include sufficient, non‑sensitive context to triage.

---

## Example Log Schema (anonymized)
```json
{
  "timestamp": "2026-04-15T02:12:34Z",
  "service": "sanitizer",
  "environment": "prod",
  "event_type": "key_unwrap",
  "actor_role": "service-operator",
  "resource": "sha256:3f2a...9b1c",
  "result": "success",
  "request_id_hash": "sha256:ab12...ef34",
  "notes": "cache-hit"
}
```

---

## Log Integrity and Tamper Resistance

- **Append‑only storage**: Use append‑only or WORM storage for audit logs where supported.
- **Signed log streams**: Sign log batches or use cryptographic checksums to detect tampering.
- **Immutable snapshots**: For incident investigations, create immutable snapshots of relevant logs and preserve chain‑of‑custody metadata.
- **Access controls**: Restrict write and delete permissions for audit stores to a small set of roles with multi‑party approval for destructive actions.

---

## Anonymization, Aggregation, and Correlation

- **Anonymization**: Apply hashing, tokenization, or redaction to identifiers before logs leave internal stores.
- **Aggregation**: Export aggregated counts, histograms, and percentiles rather than per‑submission records for dashboards and third‑party integrations.
- **Correlation**: Use ephemeral correlation tokens for debugging; expire tokens after a short window and avoid persisting them in exported datasets.
- **Re‑identification**: Re‑identification of hashed identifiers is prohibited without explicit, auditable approval and a documented justification.

---

## Observability vs Privacy Balance

- Keep high‑fidelity logs internal and access‑controlled for incident response.
- Provide operational teams with aggregated, low‑cardinality telemetry for day‑to‑day monitoring.
- Review telemetry exports for cardinality and privacy impact before enabling dashboards or alerts that surface them externally.

---

## Incident Forensics and Preservation

- Preserve raw internal logs for incident response in access‑controlled stores.
- Capture a timeline of relevant events, indicators of compromise, and artifacts (signed manifests, key IDs hashed).
- Document chain of custody for forensic artifacts and restrict access to the incident response team and authorized auditors.
- After containment, perform a scoped extraction of logs for postmortem analysis and retain per retention policy.

---

## CI and Automated Checks

- PRs that change logging, telemetry, or exported artifacts must include a privacy and cardinality review.
- Automated checks should flag:
  - Inclusion of prohibited fields (user_id, ip_address, internal_id) in exported artifacts
  - High‑cardinality labels in metrics or logs
  - New log fields that may leak internal topology or secrets
- Provide pre‑commit hooks and CI feedback to help contributors remediate issues before merge.

---

## Operational Playbooks

Maintain playbooks for:
- Investigating anomalous log access or mass exports
- Responding to suspected log tampering or integrity failures
- Preserving and extracting forensic logs during incidents
- Approving and performing sensitive log exports

Playbooks must reference `incident-response.md`, `key-management.md`, and `access-control.md`.

---

## Governance and Review

- Define owners for log schemas, retention policies, and export rules.
- Review logging policies quarterly or after incidents that reveal gaps.
- Maintain a changelog for schema changes and require security review for additions that increase fidelity or cardinality.

---

## References
- `key-management.md`
- `incident-response.md`
- `metadata.md`
- `access-control.md`

---

## Summary
Audit and logging policies ensure the Emergency Channel can detect, investigate, and recover from incidents while minimizing privacy and correlation risks. Enforce strict controls on what is logged, how logs are stored and accessed, and how telemetry is exported. Practice retention, tamper resistance, and operational playbooks to maintain forensic readiness and compliance.
