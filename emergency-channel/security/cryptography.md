---
owner: "@Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Security Module — Cryptography

## Overview

The cryptographic layer provides confidentiality, integrity, and authenticity guarantees across the entire Emergency Channel system.  
It defines the primitives, key lifecycles, and verification rules used by all modules, ensuring consistent and secure handling of sensitive data.

Cryptography is applied at multiple stages:  
- During ingestion (hashing, integrity checks)  
- During internal transport (encryption, signing)  
- During storage (at‑rest encryption)  
- During publishing (optional signing or encryption)  

The system is designed to remain secure even under partial compromise.

---

## Cryptographic Primitives

The system uses a combination of modern, well‑vetted primitives. Use the approved suites below and consult `key-management.md` for KMS/HSM usage and key scoping.

### Symmetric Encryption
- **AES-256-GCM** — primary authenticated encryption for at‑rest and bulk payloads.  
- **ChaCha20-Poly1305** — fallback for low‑power or constrained environments.

### Asymmetric Encryption and Key Exchange
- **X25519** — primary key exchange for ephemeral session keys.  
- **RSA-3072** — supported for legacy interoperability only; prefer modern curves.

### Digital Signatures
- **Ed25519** — primary signature algorithm for authenticity and non‑repudiation.  
- **ECDSA P-256** — optional compatibility mode where required.

### Hashing & Integrity
- **SHA-256** — baseline hashing for integrity and HMAC.  
- **BLAKE3** — optional high‑performance hashing where supported.

### Key Derivation
- **HKDF** — primary KDF for deriving subkeys from master secrets.  
- **PBKDF2** — legacy compatibility for slow KDF needs; avoid for new designs.

All algorithm choices must balance security, performance, and long‑term maintainability. New algorithms or parameter changes require review and approval by `@Empus/security`.

---

## Key Management (summary)

Refer to `key-management.md` for full lifecycle rules. Key points:

- **KMS/HSM**: All long‑lived private keys and KEKs must be stored in an approved KMS or HSM.  
- **Separation of keys**: Use distinct keys for encryption, signing, and key wrapping.  
- **Rotation**: Enforce rotation cadences (signing keys, DEKs, KEKs) and support dual‑key verification during rollouts.  
- **Non‑reversible identifiers**: Use hashed key identifiers in logs (e.g., `key_id_hash`) to avoid leaking raw key IDs.

---

## Signing and Verification

- **Signature policy**: All critical artifacts (manifests, configuration blobs, published packages) must be signed.  
- **Verification**: Recipients must verify signatures before accepting or acting on sensitive data. Verification failures are treated as unrecoverable errors unless explicitly documented as recoverable in the receiving module's policy.  
- **Key usage constraints**: Enforce signing‑only and encryption‑only key policies in KMS.

---

## Encryption in Transit

- **Transport security**: All internal channels must use TLS with modern cipher suites and strict certificate validation.  
- **Mutual authentication**: Use mTLS for high‑sensitivity links and validate CN/SAN and expected audience claims.  
- **Replay protection**: Implement sequence numbers, nonces, or token lifetimes to prevent replay attacks.  
- **Post‑quantum readiness**: Evaluate hybrid key exchange (classical + PQC) for long‑lived confidentiality where appropriate; changes require security review.

---

## Encryption at Rest

- **Authenticated encryption**: Use AEAD (e.g., AES‑GCM) for stored payloads.  
- **Per‑module/per‑tenant DEKs**: Where feasible, scope DEKs to limit blast radius.  
- **Integrity tags**: Store and verify integrity tags to detect corruption or tampering.  
- **No plaintext storage**: Never persist raw key material or unencrypted sensitive content in storage or logs.

---

## Ephemeral and Session Keys

- Use ephemeral session keys for short‑lived transports and internal RPCs.  
- Derive session keys via HKDF from ephemeral key exchanges (X25519) and enforce short TTLs.  
- Rotate session keys frequently and avoid long‑lived session reuse.

---

## Randomness and Entropy

- Use platform‑provided cryptographically secure RNGs (e.g., OS CSPRNG or HSM RNG).  
- Avoid custom RNG constructions. Validate entropy sources in CI and during deployment.

---

## Algorithm and Parameter Governance

- Maintain an approved algorithms list and parameter sets in the security repository.  
- Deprecate weak algorithms with a documented migration plan and timeline.  
- Any addition of new algorithms or parameter changes requires a security review and update to `metrics-and-telemetry.md` if telemetry labels or values are affected.

---

## Implementation Guidance

- Prefer well‑maintained, audited cryptographic libraries.  
- Avoid rolling your own crypto primitives.  
- Include unit and integration tests for cryptographic operations, including signature verification, key rotation, and failure modes.  
- Ensure libraries are pinned to known good versions and reviewed for supply‑chain risks.

---

## Operational Considerations

- **Monitoring**: Log KMS operations and signature verification failures to the internal audit sink (use non‑reversible identifiers).  
- **Incident response**: On suspected key compromise, follow `incident-response.md` and `key-management.md` procedures for revocation and rotation.  
- **Performance**: Benchmark cryptographic operations and include them in capacity planning.

---

## Summary

The cryptographic subsystem provides:

- Strong encryption for data in motion and at rest  
- Modern, secure primitives and clear governance  
- Strict key lifecycle and KMS usage rules  
- Robust signing and verification policies  
- Operational guidance for monitoring, rotation, and incident response

These controls ensure the Emergency Channel maintains confidentiality, integrity, and authenticity even under adversarial conditions.
