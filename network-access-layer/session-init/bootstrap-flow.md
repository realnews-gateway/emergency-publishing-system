# Bootstrap Flow

The Bootstrap Flow defines the complete sequence a client follows to establish a secure, covert, and censorship‑resistant session with the server.  
It orchestrates negotiation, key exchange, authentication, replay protection, and camouflage alignment into a unified initialization pipeline.

The bootstrap process is designed to mimic legitimate application behavior, resist active probing, and adapt to regional censorship conditions.

---

## Purpose

The bootstrap flow enables:

- Covert session establishment  
- Transport and camouflage negotiation  
- Secure key exchange  
- Authentication without static identifiers  
- Replay protection and timing camouflage  
- Region‑aware fallback behavior  
- Seamless transition into a fully established session  

This ensures that every connection begins in a stealthy and secure manner.

---

## High-Level Overview

The bootstrap flow consists of six major phases:

1. **Initial Probe**  
2. **Transport Negotiation**  
3. **Camouflage Profile Selection**  
4. **Key Exchange**  
5. **Authentication and Replay Protection**  
6. **Session Confirmation**

Each phase is designed to blend into normal TLS/QUIC/HTTP traffic patterns.

---

## Phase 1: Initial Probe

The client begins with a lightweight, browser‑like probe:

- Mimics Chrome/Safari/Edge request patterns  
- Uses realistic timing and packet sizes  
- Avoids sending identifiable metadata  
- Ensures the server responds with normalized behavior  

If the probe fails, the client triggers region‑specific fallback logic.

---

## Phase 2: Transport Negotiation

Client and server negotiate the transport layer:

- TLS-first (macOS/iOS)  
- QUIC-first (Android/Linux, tuic v5 optimized)  
- Hybrid strategy (Windows)  
- HTTP/2 fallback  
- CDN-backed negotiation  

Negotiation is embedded inside:

- TLS extensions  
- QUIC transport parameters  
- HTTP/2 pseudo-headers  

No custom protocol signatures are exposed.

---

## Phase 3: Camouflage Profile Selection

Client and server agree on:

- TLS fingerprint profile (Chrome/Safari/Firefox)  
- SNI strategy (static, rotating, domain-fronted, ECH when available)  
- Handshake obfuscation level  
- Traffic normalization mode  

Camouflage selection ensures the session blends into legitimate traffic flows.

---

## Phase 4: Key Exchange

Client and server perform:

- Ephemeral x25519 key exchange  
- Optional hybrid post‑quantum KEM  
- Replay-resistant token validation  
- HKDF-based session key derivation  
- Forward secrecy with key rotation  

Key exchange is embedded inside TLS/QUIC handshake structures to avoid detection.

---

## Phase 5: Authentication and Replay Protection

Client authenticates using:

- Ephemeral tokens  
- Session tickets  
- Optional challenge/response  

Server validates:

- Token freshness  
- Ticket integrity  
- Replay protection  
- Camouflage alignment  

Invalid attempts produce normalized TLS/QUIC/CDN errors.

---

## Phase 6: Session Confirmation

Once authentication succeeds:

- Replay protection is enforced  
- Timing camouflage is aligned  
- Transport consistency is validated  
- Session transitions into the fully established state  

---

## Error Normalization

All bootstrap failures must resemble:

- TLS alerts  
- QUIC close frames  
- HTTP error pages  
- CDN-style responses  

This prevents censors from distinguishing bootstrap failures from normal web failures.

---

## Region-Aware Behavior

Bootstrap flow adapts to:

- QUIC throttling  
- TLS fingerprint blocking  
- SNI filtering  
- CDN interference  
- HTTP-only environments  
- Mobile jitter and enterprise proxy conditions  

Fallback chains are defined in the **fallback/** subsystem.

---

## Integration

Bootstrap flow integrates with:

- **negotiation.md**  
  Transport and camouflage negotiation occur early in the bootstrap sequence.

- **key-exchange.md**  
  Key exchange begins after negotiation completes.

- **authentication.md**  
  Authentication and replay protection finalize the bootstrap process.

- **camouflage/**  
  Camouflage profiles influence every phase of bootstrap.

- **fallback/**  
  Bootstrap failures trigger fallback chains.

- **client-profiles/**  
  Platform-specific bootstrap behavior is defined here.

- **security/**  
  Timing camouflage and probing resistance are enforced.

---

## Security Considerations

Bootstrap flow must:

- Avoid static patterns  
- Normalize timing and packet sizes  
- Resist active probing  
- Prevent replayable messages  
- Blend into legitimate TLS/QUIC/HTTP traffic  
- Support region-aware fallback strategies  
- Provide indistinguishability across platforms  

---

## Summary

The Bootstrap Flow orchestrates negotiation, key exchange, authentication, replay protection, and camouflage alignment into a unified, censorship‑resistant session initialization pipeline.  
By mimicking legitimate application behavior and adapting to regional conditions, it ensures that every session begins securely and covertly.  
Extensions for **timing camouflage** and **region-aware replay protection** further strengthen resilience in diverse environments.
