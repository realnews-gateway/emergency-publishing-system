
##**Empus** Network Access Layer

Provides end-to-end foundational transport capabilities.  
This layer handles secure, indistinguishable, and fallback-capable data transport from clients to overseas servers (Singapore and Japan dual-line), and it carries all system communications (publishing, browsing, account registration, session management). It ensures data can reach storage and delivery nodes under strong censorship and interference.

---

## Core Responsibilities

- **Access establishment**: Create client sessions across varied network conditions and complete handshake and validation.  
- **Protocol camouflage**: Shape traffic and handshake behavior to resist fingerprinting and detection.  
- **Session initialization**: Secure key negotiation, token issuance, and replay protection.  
- **Entrypoint and routing management**: Expose, rotate, and route entrypoints according to policy.  
- **Fallback and recovery**: Activate alternate transports and route chains when primary paths are blocked.  
- **Security guarantees**: Integrity checks, replay protection, key handling, and minimal logging exposure.  
- **System-wide communication carrier**: Support publishing uplinks, content browsing downlinks, account registration, and session synchronization.

---

## Six Base Transports

The Network Access Layer is built on the following established transport and obfuscation technologies:

- **REALITY** — certificate camouflage and real-site behavior emulation  
- **uTLS** — browser fingerprint (Chrome/Firefox) mimicry  
- **XTLS‑Vision** — dynamic padding and statistical DPI evasion  
- **XHTTP** — HTTP/3-like behavioral camouflage (Stream and Packet modes)  
- **VLESS** — universal carrier layer  
- **TUIC v5** — high-performance UDP transport optimized for low-latency and mobile switching

---

## Three Operational Layers

Base transports are combined into three fixed operational layers for different adversary and performance profiles:

- **Performance Layer (TCP)**  
  - Combination A: VLESS + REALITY + uTLS + XTLS‑Vision  
  - Combination B: VLESS + REALITY + uTLS + XHTTP (Stream)  
  - Purpose: Provide a balance of stealth and throughput in TCP/flow-controlled scenarios.

- **High-Performance UDP Layer**  
  - Combination: TUIC v5  
  - Purpose: Deliver low-latency connectivity for mobile and real-time use cases.

- **Emergency Layer (Extreme Censorship)**  
  - Combination A: VLESS + XHTTP (Packet) + TLS 1.3 + ECH + Cloudflare Enterprise  
  - Combination B: TUIC v5 + Cloudflare Spectrum  
  - Purpose: Maintain reachability and concealment under aggressive blocking and active interference.

Transport selection adapts dynamically to measured censorship intensity and network conditions.

---

## Client and Server Responsibilities

- **Client**  
  - Perform local content preparation: sanitization, metadata minimization, de-identification, and optional signing/encryption.  
  - Use the Network Access Layer to establish sessions and transmit prepared payloads to overseas servers.  
  - Apply `client-profiles` to match platform characteristics for improved stealth and compatibility.

- **Server**  
  - Deployed overseas (planned: **Singapore and Japan dual-line**) to meet operational reachability requirements.  
  - Responsible for subscription harvesting, storing validated data, and delivering (push/response) content to clients.  
  - Avoid logging raw sensitive payloads at this layer; retain only necessary references or hashes for troubleshooting and consistency checks.

---

## Architecture Overview and Directory Layout

```
network-access-layer/
├── entrypoints/          # Entrypoint routing and protocol negotiation
├── camouflage/           # Protocol mimicry and traffic shaping
├── session-init/         # Session establishment and validation
├── fallback/             # Fallback chains and alternate transports
├── client-profiles/      # Platform-specific behavior profiles
└── security/             # Integrity checks and probing resistance
```

- **entrypoints**: Entrypoint implementations, routing rules, ALPN and negotiation strategies.  
- **camouflage**: Fingerprint emulation, handshake and traffic-shape templates, real-site behavior models.  
- **session-init**: Handshake flows, key negotiation, token issuance, and replay protection.  
- **fallback**: Region-aware fallback chains, alternate transports, and downgrade strategies.  
- **client-profiles**: Platform timing, fingerprint, and compatibility configurations.  
- **security**: Key management helpers, signing/integrity utilities, and probing-resistance notes.

---

## Layer Contracts and Deliverables

This layer defines the following contracts to ensure composability and observability with other system components:

- **Session request schema**: Field definitions (e.g., `session_id`, `client_profile`, `transport`, `metadata`).  
- **Session response and delivery schema**: Success/failure codes and metadata delivered to storage/delivery nodes (e.g., `local_ref`, `cid`, `replication_state`).  
- **Error semantics**: Standardized error codes for handshake failure, invalid signature, fallback activation, and recommended retry/backoff behavior.  
- **Observability**: Required metrics and traces (connection attempts, handshake failure rate, fallback activations, latency distributions).

Concrete JSON schemas and examples are provided in submodule READMEs and interface documentation.

---

## Security and Operational Constraints

- **Keys and credentials**: Private keys must not be committed to repositories; document local signing and rotation procedures.  
- **Replay and integrity**: Handshakes must include replay protection and message integrity checks.  
- **Minimal logging**: Avoid recording raw sensitive payloads; log hashes or references when needed for debugging.  
- **Fail-safe defaults**: Prefer safe termination over silent acceptance on ambiguous failures.  
- **Server deployment**: Planned server deployment in Singapore and Japan (dual-line).  
- **Test coverage**: Include automated smoke tests for probing, handshake failure, and fallback scenarios.

---

## Relationship to the Publishing Stack

The Network Access Layer is Empus’s foundational transport layer. It carries all client↔server communications—publishing uplinks, browsing downlinks, account registration, and session sync—and ensures that prepared client data can reach storage and delivery nodes despite censorship and network interference.

---

## Summary

By combining six base transports and three fixed operational layers with robust session security, protocol camouflage, and fallback mechanisms, the Network Access Layer forms Empus’s transport foundation, enabling reliable, covert, and observable communication across adversarial networks.
