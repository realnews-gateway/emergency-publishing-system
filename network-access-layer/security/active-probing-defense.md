# Active Probing Defense

This document describes the system’s defenses against active probing, replay attacks, and protocol fingerprinting attempts performed by state-level censorship infrastructures.  
Active probing is one of the most powerful censorship techniques: adversaries send synthetic traffic to suspected endpoints and observe whether the server responds in a way that reveals covert behavior.

The **network-access-layer** is designed to remain silent, indistinguishable, or fully mimic legitimate services under all probing conditions.

---

## Adversary Probing Capabilities

The system assumes adversaries may perform:

### 1. Synthetic Handshake Probing
    - Sending fake TLS ClientHello messages
    - Sending malformed QUIC (tuic v5) Initial packets
    - Attempting to trigger protocol-specific responses

### 2. Replay Attacks
    - Replaying previously captured handshake messages
    - Attempting to reuse tokens or session tickets

### 3. Protocol Deviation Testing
    - Sending invalid ALPN values
    - Sending unsupported cipher suites
    - Manipulating TLS extensions
    - Modifying QUIC transport parameters

### 4. Timing-Based Probing
    - Measuring response latency
    - Detecting fallback transitions
    - Observing retry behavior

### 5. CDN/HTTP Probing
    - Sending HTTP/2 or HTTP/3 requests to endpoints
    - Expecting CDN-like behavior
    - Checking for inconsistencies in headers or status codes

The system must not reveal itself under any of these probing attempts.

---

## Defense Mechanisms

The **network-access-layer** uses multiple layers of defense:

---

### 1. Silent Drop for Unauthorized Traffic

The server does not respond to:

    - Unknown TLS fingerprints
    - Unknown QUIC Initial packets
    - Invalid ALPN values
    - Malformed handshake messages
    - Replay attempts

This prevents adversaries from distinguishing the server from a closed port or a rate-limited CDN node.

---

### 2. Challenge/Response Authentication

Before revealing any covert behavior, the server requires:

    - A cryptographic challenge embedded inside the handshake
    - A valid response derived from client-side secrets
    - A replay-resistant token

If the challenge is missing or invalid:

    - The server behaves like a normal HTTPS/CDN endpoint
    - No covert behavior is exposed

---

### 3. Replay Protection

Replay attempts are rejected using:

    - Nonces
    - Ephemeral keys
    - One-time tokens
    - Timestamp-based validation

Replayed handshakes produce:

    - Normal TLS alerts
    - Normal QUIC close frames
    - No covert-specific errors

---

### 4. Protocol Deviation Handling

When adversaries send malformed or unusual protocol messages:

    - TLS deviations trigger standard browser-like alerts
    - QUIC deviations trigger CONNECTION_TIMEOUT or PROTOCOL_VIOLATION
    - HTTP deviations trigger 400/404/503 responses

All responses match real-world server behavior.

---

### 5. CDN Behavior Mimicry

If the server is probed with HTTP/2 or HTTP/3 requests:

    - It returns CDN-like responses
    - It may return 404, 503, or 504 depending on region
    - It may mimic rate-limited CDN nodes

This prevents adversaries from detecting that the endpoint is not a real CDN.

---

### 6. Timing Camouflage

The server mimics real-world timing patterns:

    - Browser-like TLS handshake timing
    - CDN-like response delays
    - QUIC retry timing
    - Randomized jitter

Timing does not reveal covert behavior.

---

### 7. Region-Aware Probing Defense

Different regions require different strategies:

    - High-risk regions: strict silent-drop policy  
    - Moderate-risk regions: CDN mimicry  
    - Low-risk regions: relaxed behavior with fallback support  
    - Mobile-priority regions: wider jitter ranges, adaptive retry intervals  
    - Enterprise regions: TLS Reality-style mimicry with API-like responses  

This prevents region-specific probing heuristics from succeeding.

---

## Integration with Other Modules

Active probing defense integrates with:

- **session-init/**  
  Challenge/response authentication, replay protection.

- **camouflage/**  
  TLS/QUIC fingerprint mimicry, handshake obfuscation.

- **fallback/**  
  Error normalization and region-aware fallback behavior.

- **entrypoints/**  
  Protocol-specific probing resistance.

- **client-profiles/**  
  Platform-specific timing and fingerprint adjustments.

Active probing defense is embedded across all layers of the system.

---

## Summary

The active probing defense subsystem ensures that endpoints do not reveal themselves under synthetic traffic, replay attacks, or protocol deviation tests.  
By combining silent-drop behavior, challenge/response authentication, replay protection, CDN mimicry, timing camouflage, and region-aware strategies, the **network-access-layer** remains resilient against state-level active probing infrastructures.  
Extensions for **mobile** and **enterprise** environments further strengthen defenses against diverse adversarial conditions.
