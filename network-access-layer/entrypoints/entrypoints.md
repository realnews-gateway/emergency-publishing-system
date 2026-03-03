# Entrypoints Specification

The Entrypoints subsystem defines all inbound access paths into the VPN Access Layer.  
Each entrypoint represents a transport mechanism designed to bypass censorship, blend into legitimate traffic, and provide a stable foundation for session initialization.

Entrypoints are the first component a user interacts with when connecting to the system.

---

## Goals

Entrypoints must:

- Provide multiple covert access paths  
- Mimic legitimate TLS/QUIC/HTTP/CDN traffic  
- Integrate seamlessly with camouflage and session-init layers  
- Support region-aware fallback strategies  
- Maintain resilience under DPI, throttling, and active probing  
- Offer consistent behavior across client platforms  

---

## Entrypoint Types

The system supports four major entrypoint categories:

### **1. TLS Entrypoints**
- Trojan  
- XTLS  
- Reality-style TLS mimicry  
- Browser-grade TLS fingerprints  
- Real-site certificate camouflage  

Used when TLS-based DPI is dominant or QUIC is heavily throttled.

---

### **2. QUIC Entrypoints**
- Hysteria2  
- QUIC obfuscation  
- Traffic normalization to resemble video streaming or gaming traffic  

Used for high-performance access or when TLS is aggressively filtered.

---

### **3. HTTP Entrypoints**
- XHTTP  
- HTTP/2 domain-fronted access  
- CDN-compatible HTTP flows  

Used when TLS/QUIC are blocked or require fallback.

---

### **4. CDN Entrypoints**
- Cloudflare  
- Fastly  
- Akamai  

Used for maximum stealth and global reach.

---

## Integration Requirements

Entrypoints must integrate with:

- **camouflage/**  
  TLS fingerprint shaping, SNI rotation, handshake obfuscation.

- **session-init/**  
  Transport negotiation and secure key exchange.

- **fallback/**  
  Automatic switching when an entrypoint is blocked.

- **client-profiles/**  
  Platform-specific entrypoint preferences.

---

## Security Considerations

Entrypoints must:

- Avoid static signatures  
- Use indistinguishable error behavior  
- Support active probing resistance  
- Minimize metadata leakage  
- Maintain compatibility with region-specific blocking patterns  

---

## Summary

Entrypoints provide the system’s first line of defense against censorship.  
By offering TLS, QUIC, HTTP, and CDN-backed access paths, they ensure users can always reach the system—even in the most hostile network environments.
