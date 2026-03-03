# Session Initialization

The Session Initialization subsystem defines how clients and servers establish a secure, covert, and censorship‑resistant communication session.  
It provides the negotiation, key exchange, authentication, and bootstrap mechanisms used across all entrypoints (TLS, QUIC, HTTP, CDN).

Session initialization is designed to resist active probing, fingerprinting, replay attacks, and metadata leakage.

---

## Purpose

Session initialization enables:

- Secure and covert session establishment  
- Negotiation of transport, camouflage, and fallback profiles  
- Key exchange resistant to replay and probing  
- Authentication without exposing identifiers  
- Integration with TLS, QUIC, and HTTP entrypoints  
- Region‑aware session bootstrap strategies  

This subsystem ensures that every session begins in a stealthy and secure manner.

---

## Core Components

### **1. Transport Negotiation**
Determines which entrypoint and transport to use:

- TLS-first (macOS/iOS)  
- QUIC-first (Android/Linux)  
- Hybrid strategy (Windows: TLS-first with optional QUIC)  
- HTTP fallback  
- CDN-backed negotiation  

Negotiation must avoid detectable patterns.

---

### **2. Key Exchange**
Provides secure session keys using:

- ECDH (x25519)  
- Hybrid post-quantum options (optional)  
- One-time ephemeral keys  
- Replay-resistant handshake tokens  

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

---

### **4. Bootstrap Flow**
Defines how a client begins communication:

- Initial probe  
- Transport selection  
- Camouflage profile selection  
- Key exchange  
- Session confirmation  

Bootstrap must mimic real browser or app behavior.

---

### **5. Error Normalization**
Errors must resemble real-world behavior:

- TLS alerts  
- QUIC close frames  
- HTTP error pages  
- CDN-style responses  

This prevents active probing from detecting anomalies.

---

## Platform Defaults

Different platforms require different negotiation strategies:

- **macOS/iOS**  
  Safari-style TLS fingerprints; TLS-first negotiation for maximum compatibility.

- **Android/Linux**  
  QUIC-first negotiation; optimized for mobile networks and modern transports.

- **Windows**  
  Hybrid strategy:  
  - TLS-first for reliability and compatibility with Chrome/Edge fingerprints  
  - QUIC enabled when available through application-level implementations  
  This ensures Windows clients blend into common desktop traffic patterns.

---

## Integration

Session initialization integrates with:

- **camouflage/**  
  - TLS fingerprint matching  
  - SNI randomization  
  - Handshake obfuscation  

- **entrypoints/**  
  - Transport negotiation  
  - Entry protocol selection  

- **fallback/**  
  - Region-specific fallback chains  
  - Transport downgrade logic  

- **client-profiles/**  
  - Platform-specific negotiation behavior  

---

## Security Considerations

Session initialization must:

- Avoid static handshake patterns  
- Prevent replay attacks  
- Normalize timing and packet sizes  
- Resist active probing  
- Avoid exposing backend infrastructure  
- Support region-aware negotiation strategies  

---

## Summary

The Session Initialization subsystem provides the secure and covert foundation for all communication.  
By combining transport negotiation, key exchange, authentication, and camouflage-aware bootstrap flows, it ensures that every session begins in a way that is indistinguishable from legitimate internet traffic and resilient to censorship.
