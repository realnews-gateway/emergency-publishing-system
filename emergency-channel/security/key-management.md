---
owner: "@Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Key Management

## Purpose
Define key lifecycle, storage, rotation, access, and audit rules for all cryptographic keys used by the Emergency Channel. This document provides actionable requirements for developers, operators, and incident responders to ensure keys remain confidential, available, and auditable.

## Scope
Applies to all symmetric and asymmetric keys used for:
- **Encryption** (DEKs for data at rest)
- **Key wrapping** (KEKs)
- **Signing and MACs** (Ed25519, HMAC)
- **TLS/private keys** for transport
- **Ephemeral/session keys** used for short‑lived channels

Covers keys created by services, keys managed in KMS/HSM, and any local caches or derived keys.

---

## Key Types and Intended Use
- **Data Encryption Keys (DEK)** — encrypt stored payloads; scoped per‑tenant or per‑channel where feasible.
- **Key Encryption Keys (KEK)** — wrap DEKs; stored in KMS/HSM.
- **Signing Keys** — Ed25519 or approved alternatives for authenticity and non‑repudiation.
- **HMAC Keys** — for message authentication where symmetric MACs are used.
- **TLS Private Keys** — for transport security; rotate and protect via KMS/CA.
- **Ephemeral Keys** — short‑lived keys derived per session or RPC.

Each key must have a documented **purpose**, **owner**, **scope**, and **usage constraints**.

---

## Key Storage and Approved KMS/HSM Usage
- **Primary storage**: Store all long‑lived private keys and KEKs in an approved KMS or HSM. Do not store plaintext private keys in source control, container images, or configuration files.
- **Local caches**: If a service caches unwrapped keys for performance, encrypt the cache with a KEK and limit TTL to **1 hour** by default. Cache access must be audited.
- **Secrets manager**: Use the organization’s approved secrets manager for short‑lived tokens and non‑KMS secrets.
- **Non‑reversible identifiers**: Use hashed key identifiers (e.g., `sha256(key_id)`) in logs and telemetry to avoid leaking raw key IDs.

---

## Access Control and Authorization
- **Principle of least privilege**: Grant key access only to service principals or human roles that require it.
- **Role separation**: Distinguish roles for key creation, approval, and use (e.g., key-admin, key-approver, key-user).
- **Short‑lived credentials**: Prefer ephemeral credentials (OIDC tokens, short TTL service identities) for requesting unwrapped keys.
- **Approval workflows**: Changes to KEKs or signing root keys require multi‑party approval (security-owner + ops-owner).
- **Break‑glass**: Implement a documented break‑glass process for emergency key access; every break‑glass event must be time‑limited and fully audited.

---

## Key Generation and Entropy
- **Secure RNG**: Use platform CSPRNGs or HSM RNGs for key generation. Do not implement custom RNGs.
- **Algorithm parameters**: Generate keys with approved parameters (see `cryptography.md`). New parameter sets require security review.
- **Generation location**: Prefer generation inside KMS/HSM. If generated outside, import securely and mark origin in metadata.

---

## Rotation, Expiry, and Grace Periods
- **Rotation cadence (recommended defaults)**:
  - Signing keys: rotate every **90 days**.
  - DEKs: rotate on rekey events or every **180 days**.
  - KEKs/HSM root keys: follow provider best practices; document windows.
- **Automated rotation**: Use KMS APIs to automate rotation where possible.
- **Dual‑key verification**: Support dual‑key verification during rollouts with a configurable grace period (default **7 days**) to allow rolling updates.
- **Expiry metadata**: All keys must include expiry and creation timestamps in KMS metadata.

---

## Key Usage Constraints
- **Separation of duties**: Never use the same key for signing and encryption.
- **Scoped keys**: Prefer per‑tenant, per‑channel, or per‑environment keys to limit blast radius.
- **Enforce usage in KMS**: Set key policies to restrict operations (e.g., `encrypt` vs `sign`) and allowed callers.
- **Algorithm enforcement**: Only use algorithms and parameters approved in `cryptography.md`.

---

## Compromise, Revocation, and Emergency Rotation
- **Detection**: Monitor for anomalous KMS access patterns, signature verification failures, and unexpected key exports.
- **Immediate actions on suspected compromise**:
  1. Revoke the compromised key in KMS (or mark as compromised).
  2. Rotate affected keys and any dependent DEKs.
  3. Re‑encrypt persisted data if DEKs are affected.
  4. Trigger incident response playbook and notify `@Empus/security`.
- **Key‑revocation log**: Maintain a revocation log with timestamp, actor role, key_id_hash, and justification.
- **Recovery**: Document recovery steps and validate via staging before re‑enabling production flows.

---

## Auditing and Logging
- **Audit all KMS operations**: create, import, rotate, access (unwrap), revoke, and policy changes.
- **Audit fields**: actor role, operation, key_id_hash (non‑reversible), timestamp, request context, and justification for privileged actions.
- **Retention**: Retain audit logs per `audit-and-logging.md` retention rules and ensure logs are tamper‑resistant.
- **Alerting**: Create alerts for unusual patterns (e.g., mass key unwraps, repeated failures, policy changes).

---

## Testing, Validation, and CI
- **Unit tests**: Validate key usage constraints, signature verification, and error handling.
- **Integration tests**: Exercise rotation workflows, dual‑key verification, and KMS policy enforcement in staging.
- **Chaos tests**: Simulate KMS outages and key revocation to validate graceful degradation and recovery.
- **CI checks**: Ensure code that references keys uses approved key identifiers and does not embed secrets. Include secret scanning in PR pipelines.

---

## Operational Playbooks
Maintain playbooks for:
- **Key compromise**: detection, revocation, rotation, and re‑encryption steps.
- **KMS outage**: fallback modes, degraded operation, and recovery.
- **Key rotation**: step‑by‑step rotation with verification and rollback instructions.
- **Key import/export**: secure import/export and verification procedures.

Playbooks must be versioned and exercised periodically.

---

## Key Metadata and Documentation
Every key must include metadata in KMS:
- **owner** (team or role)
- **purpose** (DEK, KEK, signing, TLS)
- **scope** (service, tenant, environment)
- **creation_time** and **expiry_time**
- **rotation_policy** and **last_rotated**
- **origin** (generated-in-kms / imported)

Document key mappings and dependencies in architecture diagrams and module design docs.

---

## Compliance and Third‑Party Dependencies
- **Third‑party KMS**: Evaluate provider SLAs, export controls, and compliance posture before adoption.
- **Supply‑chain**: Track libraries and tools used for key handling; apply supply‑chain risk assessments.
- **Legal**: Follow jurisdictional rules for key export, escrow, and lawful access.

---

## References
- `cryptography.md`  
- `audit-and-logging.md`  
- `incident-response.md`  
- `runtime.md`

---

## Summary
Key management is foundational to the Emergency Channel's security posture. Enforce KMS/HSM usage, strict access controls, automated rotation, robust auditing, and practiced playbooks to minimize risk and enable rapid, auditable response to incidents.
