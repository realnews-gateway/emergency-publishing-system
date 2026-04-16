# Camouflage Specification

This subsystem defines the protocol, fingerprinting, mimicry, randomization, and obfuscation strategies used across TLS, QUIC, HTTP, and CDN entrypoints to make Empus traffic blend with legitimate Internet traffic. It provides concrete guidance, templates, and test vectors for implementers and operators.

---

## Purpose

**Goal:** ensure client↔server flows are indistinguishable from benign services while remaining compatible with Empus entrypoints, session initialization, and fallback chains.

**Capabilities enabled**
- Real-site mimicry for domain alignment and certificate behavior  
- TLS and QUIC fingerprint shaping to match mainstream clients  
- SNI rotation and domain selection to avoid static blocking  
- Handshake obfuscation to resist active probing and protocol classifiers  
- Traffic normalization to match packet sizes, timing, and stream behavior  
- Integration points for entrypoints, session-init, and fallback logic

---

## Components

### Real Site Mimicry
- Maintain a small, auditable template library for high-value domains: certificate chains, HTTP/3 behavior, error responses, and timing profiles.  
- Templates include: certificate metadata, ALPN/HTTP behavior, resource timing, and error semantics.  
- Templates are versioned and require a short changelog on modification.

### TLS Fingerprint Camouflage
- Provide selectable fingerprint profiles (Chrome, Firefox, Safari) with safe mapping rules to client profiles.  
- Specify cipher ordering, extension ordering, ALPN sequences, GREASE usage, and key share behavior.  
- Document risk levels for each profile and rotation cadence.

### SNI Randomization
- Define SNI pools, rotation policies, randomness bounds, and region-aware selection rules.  
- Support decoy SNI and domain-fronting patterns where operationally required.  
- Include constraints to avoid predictable or easily correlated SNI sequences.

### Handshake Obfuscation
- Define obfuscation transforms applicable to TLS and QUIC handshakes: delayed initiation, padding, fragmentation, conditional acceptance, and protocol confusion layers.  
- Include replay protection, verification vectors, and deterministic test vectors for CI.  
- Specify safe limits for padding and fragmentation to avoid breaking middleboxes.

### Traffic Normalization
- Specify packet size distributions, inter-packet timing profiles, stream multiplexing behavior, and pacing rules to match target services.  
- Provide profiles for common targets (e.g., mainstream HTTPS, video streaming, lightweight API traffic).  
- Include guidance for adaptive normalization under mobile or lossy networks.

---

## Integration Points

- **Entrypoints**: camouflage artifacts are applied at TLS/QUIC/HTTP/CDN entrypoints to present the chosen profile to the network.  
- **Session Init**: handshake transforms must be validated against session-init to preserve key exchange semantics.  
- **Fallback**: region-aware fallback chains may switch profiles, SNI pools, or transports; camouflage must support seamless profile switching.  
- **Client Profiles**: client-side selection maps user platform and version to an appropriate fingerprint and timing profile.

---

## Testing and Validation

- Provide minimal reproducible examples and smoke tests for local validation and CI.  
- Required tests:
  - Shaped ClientHello acceptance by a test entrypoint  
  - Handshake obfuscation round-trip correctness  
  - Fallback activation and profile switching behavior  
- Tests must be deterministic, fast, and suitable for automated CI on PRs that modify camouflage artifacts.

---

## Security and Operational Constraints

- **No raw payload logging**: only hashes or references for debugging.  
- **Key handling**: private keys used for mimicry must be managed per ops policy and never committed.  
- **Rotation**: rotate high-risk fingerprints, SNI pools, and templates on a defined cadence.  
- **Fail-safe behavior**: prefer safe termination over silent acceptance on ambiguous failures.  
- **Auditability**: each template or fingerprint change must include a short rationale and changelog entry.

---

## File Guidance

Each file in this directory should start with a one-paragraph summary and a short "what to change" section for implementers. Keep templates small, auditable, and versioned.

---

## Summary

Camouflage is a dynamic, auditable subsystem that combines real-site mimicry, fingerprint shaping, SNI strategies, handshake obfuscation, and traffic normalization. It is the primary technical layer that enables Empus to operate under active censorship and protocol classification while remaining compatible with entrypoints and fallback mechanisms.
