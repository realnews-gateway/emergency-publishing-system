---
owner: "Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-14"
---

# Access Control

## Purpose
Define access control models, role definitions, service-to-service authentication, and operational procedures to enforce least privilege across the Emergency Channel. This document provides concrete requirements for developers, operators, and auditors to ensure that access to data, keys, and control planes is limited, auditable, and recoverable.

## Scope
Applies to:
- Human users (operators, developers, auditors)
- Service principals and service accounts
- CI/CD pipelines and automation
- Administrative consoles, KMS, and secrets managers
- All environments: **dev**, **staging**, **production**

## Principles
- **Least privilege**: Grant only the permissions required to perform a task.
- **Role-based access**: Prefer RBAC with well-defined roles and scopes.
- **Separation of duties**: Critical actions require multi-person approval.
- **Just-in-time access**: Use time-limited elevation rather than permanent privileges.
- **Auditability**: All privilege changes and privileged actions must be logged.
- **Fail-safe defaults**: Deny by default; allow only explicitly permitted actions.

---

## Roles and Responsibilities

### Example Roles
- **Empus/security** — policy approver and incident authority.
- **service-admin** — deploy and manage service runtime; limited to specific services.
- **service-operator** — perform operational tasks (restart, scale) with scoped permissions.
- **developer** — push code to non-production; limited production access via PRs and approvals.
- **auditor** — read-only access to audit logs and compliance reports.
- **oncall** — respond to incidents with limited escalation privileges.
- **key-admin** — manage KMS policies and key lifecycle (requires multi-party approval).

### Role Metadata
Each role must have:
- **owner** (team or alias)
- **allowed actions** (explicit list)
- **resource scope** (service, environment, tenant)
- **approval requirements** (single approver, multi-approver)
- **review cadence**

---

## Role Based Access Control (RBAC)

### Model
- Use **RBAC** as the primary model for human and machine identities.
- Define roles at the organization, team, and service level.
- Map roles to minimal permission sets; avoid broad roles like "admin" without scope.

### Scoping
- Scope permissions by **environment** (prod/stage/dev), **service**, and **resource type**.
- Use resource tags and labels to enforce policy scoping in IAM systems.

### Policy Management
- Store RBAC policies as code where supported (**policy-as-code**).
- Require PRs for policy changes with automated policy linting and mandatory reviewers from **Empus/security**.
- Maintain a changelog of policy updates with rationale and risk assessment.

---

## Service to Service Authentication

### Identity
- Use provider-managed service identities (**OIDC**, cloud service identities) or short-lived certificates.
- Avoid long-lived static credentials for services.

### Mutual Authentication
- Prefer **mTLS** for high-sensitivity channels; validate CN/SAN and expected audience.
- Validate tokens at the application layer: issuer, audience, expiry, and revocation status.

### Token Best Practices
- Use **short TTLs** for tokens and rotate frequently.
- Use audience and scope claims to limit token use.
- Implement token revocation and introspection where supported.

### Service Account Hygiene
- One service account per service and environment.
- Rotate service account credentials regularly.
- Revoke accounts unused for a configurable period.

---

## Privilege Elevation and Just-in-Time Access

- Implement **just-in-time (JIT)** elevation for human operators requiring temporary privileges.
- Require explicit justification and automatic expiry for elevated sessions.
- Log the reason, approver, and duration for every elevation event.
- Use **multi-factor authentication (MFA)** for elevation approval.

---

## Break Glass and Emergency Access

- Define a documented **break-glass** process for emergency access with:
  - Time-limited credentials
  - Mandatory justification and post-facto review
  - Immediate notification to **Empus/security**
  - Full audit trail of actions taken during break-glass
- Break-glass events must trigger a post-incident review and remediation plan.

---

## Secrets and Credential Management

- Store secrets in an approved secrets manager or KMS; **never** commit secrets to source control.
- Use environment-scoped secrets and avoid sharing secrets across services.
- Enforce secret rotation policies and automated secret scanning in CI.
- Limit secret access to the minimal set of principals and log all secret retrievals.

---

## Access Reviews and Certification

- Conduct **quarterly** access reviews for privileged roles and service accounts.
- Automate review reports and require owners to certify access lists.
- Remove or reduce privileges for accounts that fail certification or are inactive.

---

## Approval Workflows and Change Control

- Policy changes that widen privileges require:
  - A PR with risk assessment and test plan
  - Approval from **Empus/security** and the affected service owner
  - Automated CI checks passing (policy lint, least-privilege heuristics)
- Emergency policy changes may be applied with post-facto review and must be documented.

---

## Auditing and Monitoring

- Audit **all** access control changes and privileged operations.
- Audit records must include: actor, role, action, resource, justification, and timestamp.
- Forward audit logs to an access-controlled audit sink; retain per `audit-and-logging.md`.
- Alert on anomalous access patterns: mass privilege grants, repeated failed elevation attempts, unusual token usage.

---

## Enforcement and Runtime Checks

- Enforce RBAC at the API gateway, service mesh, or application layer as appropriate.
- Implement runtime authorization checks for sensitive operations (e.g., key unwrap, data export).
- **Fail closed** on authorization failures; do not silently degrade to permissive behavior.

---

## CI and Automation Controls

- CI systems must use **ephemeral credentials** and least-privilege service accounts.
- PRs that modify access control or IAM policies must trigger:
  - Policy-as-code linting
  - Secret scanning
  - Approval from **Empus/security**
- Prevent merges that introduce wildcard or overly broad permissions.

---

## Testing and Validation

- Unit tests for authorization logic and policy enforcement.
- Integration tests that validate role mappings and token validation.
- Adversarial tests simulating privilege escalation attempts.
- Include access-control checks in pre-production gates.

---

## Operational Playbooks

Maintain playbooks for:
- Privilege escalation investigation and rollback
- Compromised service account response
- Emergency access and break-glass handling
- Automated remediation for misconfigured policies

Playbooks must reference `incident-response.md` and `audit-and-logging.md`.

---

## Governance

- Define role owners and maintain an authoritative role registry.
- Review access-control policies quarterly or after incidents.
- Document mapping between roles and responsibilities in architecture docs.
- Changes to core access-control patterns require approval from **Empus/security**.

---

## Examples and Templates

### Example role definition (YAML)
```yaml
role: service-admin
owner: "team-ops"
scope:
  services: ["sanitizer", "router"]
  environments: ["staging", "prod"]
permissions:
  - deploy
  - restart
  - view-logs
approval: "single-approver"
review-cadence: "90d"
```

### Example approval flow
1. Developer opens PR to change IAM policy.
2. CI runs policy lint and secret scan.
3. PR requests review from **Empus/security** and service owner.
4. After approvals, merge triggers staged rollout and audit entry.

---

## References
- `key-management.md`
- `audit-and-logging.md`
- `incident-response.md`
- `runtime.md`

---

## Summary
Access control enforces least privilege across people, services, and automation. Use RBAC, short-lived credentials, JIT elevation, strict auditing, and automated CI checks to reduce risk and ensure that access changes are deliberate, reviewed, and reversible.
