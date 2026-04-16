# Session Initialization

The Session Initialization subsystem defines how clients and servers establish a secure, covert, and censorship‑resistant communication session.  
It provides the negotiation, key exchange, authentication, and bootstrap mechanisms used across all entrypoints (TLS, QUIC, HTTP, CDN).

Session initialization is designed to resist active probing, fingerprinting, replay attacks, traffic analysis, and metadata leakage.

---

## Purpose

Session initialization enables:

- Secure and covert session establishment  
- Negotiation of transport, camouflage, and fallback profiles  
- Key exchange resistant to replay and probing  
- Authentication without exposing identifiers  
- Integration with TLS, QUIC, and HTTP entrypoints  
- Region‑aware session bootstrap strategies  
- Timing camouflage and replay protection  

This subsystem ensures that every session begins in a stealthy and secure manner.

---

## Core Components

### **1. Transport Negotiation**
Determines which entrypoint and transport to use:

- TLS-first (macOS/iOS)  
- QUIC-first (Android/Linux, tuic v5 optimized)  
- Hybrid strategy (Windows: TLS-first with optional QUIC)  
- HTTP fallback  
- CDN-backed negotiation  

Negotiation must avoid detectable patterns and mimic real-world browser/app behavior.

---

### **2. Key Exchange**
Provides secure session keys using:

- ECDH (x25519)  
- Hybrid post-quantum options (optional)  
- One-time ephemeral keys  
- Replay-resistant handshake tokens  
- Forward secrecy with key rotation  

Key exchange must be indistinguishable from normal TLS/QUIC behavior.

---

### **3. Authentication**
Authentication must avoid exposing:

- User identifiers  
- Static tokens  
- Replayable credentials  

Supported methods:

- Ephemeral tokens  
- Time-limited session tickets  
- Domain-fronted authentication flows  
- Region-aware authentication strategies  

---

### **4. Bootstrap Flow**
Defines how a client begins communication:

- Initial probe  
- Transport selection  
- Camouflage profile selection  
- Key exchange  
- Session confirmation  

Bootstrap must mimic real browser or app behavior, including timing and packet size camouflage.

---

### **5. Error Normalization**
Errors must resemble real-world behavior:

- TLS alerts  
- QUIC close frames  
- HTTP error pages  
- CDN-style responses  

This prevents active probing from detecting anomalies.

---

### **6. Replay Protection (Extension)**
Replay attempts are rejected using:

- Nonces  
- Ephemeral keys  
- One-time tokens  
- Timestamp-based validation  

Replayed handshakes produce only normalized TLS/QUIC errors.

---

### **7. Timing Camouflage (Extension)**
Session-init traffic must blend into real-world timing:

- Browser-like handshake delays  
- Mobile jitter profiles  
- CDN-like response pacing  
- Randomized retry intervals  

---

## Platform Defaults

Different platforms require different negotiation strategies:

- **macOS/iOS**  
  Safari-style TLS fingerprints; TLS-first negotiation for maximum compatibility.

- **Android/Linux**  
  QUIC-first negotiation (tuic v5); optimized for mobile networks and modern transports.

- **Windows**  
  Hybrid strategy:  
  - TLS-first for reliability and compatibility with Chrome/Edge fingerprints  
  - QUIC enabled when available through application-level implementations  
  This ensures Windows clients blend into common desktop traffic patterns.

---

## Integration

Session initialization integrates with:

- **camouflage/**  
  TLS fingerprint matching, SNI randomization, handshake obfuscation.

- **entrypoints/**  
  Transport negotiation, entry protocol selection.

- **fallback/**  
  Region-specific fallback chains, transport downgrade logic.

- **client-profiles/**  
  Platform-specific negotiation behavior.

- **security/**  
  Replay protection, timing camouflage, probing resistance.

---

## Security Considerations

Session initialization must:

- Avoid static handshake patterns  
- Prevent replay attacks  
- Normalize timing and packet sizes  
- Resist active probing  
- Avoid exposing backend infrastructure  
- Support region-aware negotiation strategies  
- Provide indistinguishability across platforms  

---

## Summary

The Session Initialization subsystem provides the secure and covert foundation for all communication.  
By combining transport negotiation, key exchange, authentication, replay protection, and camouflage-aware bootstrap flows, it ensures that every session begins in a way that is indistinguishable from legitimate internet traffic and resilient to censorship.  
Extensions for **timing camouflage** and **region-aware replay protection** further strengthen resilience in diverse environments.
