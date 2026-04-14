---
owner: "@Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Security Module — Transport Security

## Overview

The transport security layer protects all data in motion across the Emergency Channel system.  
It ensures confidentiality, integrity, authenticity, and resistance to interception or manipulation, even in adversarial network environments.

Transport security covers:
- Module‑to‑module communication  
- Internal API calls and RPCs  
- Storage synchronization and replication  
- Optional secure publishing channels

Design assumption: external networks may be monitored, filtered, or actively hostile; internal networks are treated as lower‑trust and must still be protected.

---

## Core Principles

Transport security is built on the following principles:

- **Encrypt everything**  
  No plaintext communication between modules or across trust boundaries.

- **Authenticate every connection**  
  Mutual authentication prevents impersonation and unauthorized access.

- **Minimize metadata leakage**  
  Transport headers and observable patterns should reveal as little as possible.

- **Fail closed**  
  Connections without valid security guarantees are rejected.

- **Defense in depth**  
  Combine TLS, token validation, mTLS, and application‑level checks for layered protection.

---

## Protocols and Baselines

- **TLS**: Require TLS 1.3 for all internal and external channels where TLS is applicable.  
- **Cipher suites**: Use AEAD suites (AES‑GCM, ChaCha20‑Poly1305) and prefer forward secrecy.  
- **Certificate validation**: Enforce strict validation of CN/SAN, expected audience, and certificate chains.  
- **No insecure fallbacks**: Disable TLS 1.2 and earlier for sensitive channels; disallow weak ciphers and renegotiation.

Certificates and keys must be scoped per service/environment and rotated according to `key-management.md`.

---

## Mutual Authentication and Identity

- **mTLS**: Use mutual TLS for high‑sensitivity channels and administrative endpoints.  
- **Token validation**: Validate OIDC/JWT tokens at the application layer; check audience, issuer, expiry, and revocation where supported.  
- **Service identity**: Map certificates or tokens to service identities and enforce RBAC based on those identities.  
- **Short lifetimes**: Prefer short‑lived credentials and session tokens to reduce exposure.

---

## Replay, Injection, and Integrity Protections

- **Replay protection**: Use nonces, sequence numbers, or short token lifetimes to prevent replay attacks.  
- **Message integrity**: Use AEAD or explicit integrity tags to detect tampering.  
- **Strict parsing**: Validate and canonicalize protocol inputs to avoid injection and parsing ambiguities.  
- **Request/response pairing**: Enforce strict correlation between requests and responses for stateful protocols.

---

## Post‑Quantum Readiness

- **Hybrid key exchange**: Evaluate hybrid classical + PQC key exchange (e.g., X25519 + Kyber) for long‑lived confidentiality where appropriate.  
- **Review process**: Any PQC adoption requires a security review and compatibility testing.  
- **Fallback strategy**: Maintain clear migration and rollback plans for hybrid or PQC deployments.

---

## Channel Hardening by Transport Type

### HTTP(S)
- Strict TLS enforcement and HSTS where applicable.  
- Minimize headers and avoid custom headers that leak topology.  
- Disallow insecure redirects and open proxies.  
- Validate request sizes and timeouts at the edge.

### RPC / Internal APIs
- Use mTLS or short‑lived tokens for authentication.  
- Enforce schema validation and strict versioning.  
- Isolate RPC endpoints by role and environment.

### IPC / Local Links
- Use ephemeral session keys and OS‑level protections (file permissions, socket ACLs).  
- Limit access to local sockets to authorized processes only.

### Opportunistic or P2P Links
- Use opportunistic encryption when full trust is not possible.  
- Require identity verification before elevating privileges.  
- Apply additional integrity checks and replay protections.

---

## Failure Handling and Resilience

- **Fail closed**: Do not fall back to insecure channels on failure.  
- **Retries**: Implement exponential backoff and jitter for retries.  
- **Channel switching**: Support safe channel switching only when the alternative meets security baselines.  
- **Graceful degradation**: Reduce functionality rather than exposing sensitive data when transport guarantees cannot be met.  
- **Observability**: Log transport failures and security events to internal audit sinks (use non‑reversible identifiers).

---

## Metadata Minimization in Transport

- Avoid exposing internal hostnames, module names, or topology in headers or URLs.  
- Do not include long‑lived identifiers in query strings.  
- Normalize request patterns and sizes where feasible to reduce fingerprinting.  
- Treat transport metadata as sensitive and apply the rules in `metadata.md`.

---

## Operational Considerations

- **Certificate lifecycle**: Automate issuance, rotation, and revocation via KMS/CA tooling.  
- **Monitoring**: Alert on certificate expiry, unusual handshake failures, or spikes in connection errors.  
- **Testing**: Include transport security tests in CI (cipher suite checks, TLS version enforcement, mTLS validation).  
- **Incident response**: On suspected transport compromise, follow `incident-response.md` and rotate affected credentials immediately.

---

## Summary

The transport security subsystem provides:

- Strong, authenticated encryption for all data in motion  
- Mutual authentication and identity mapping for services  
- Replay, injection, and integrity protections  
- Post‑quantum readiness planning where appropriate  
- Hardened channel configurations for diverse environments  
- Fail‑closed behavior and resilient failure handling

These controls ensure that the Emergency Channel maintains confidentiality, integrity, and availability even across hostile or unreliable networks.
