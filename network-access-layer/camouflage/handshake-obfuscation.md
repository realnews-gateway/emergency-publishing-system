# Handshake Obfuscation

Handshake obfuscation protects the system from active probing, protocol fingerprinting, and handshake-based DPI detection.  
By modifying, delaying, padding, or restructuring handshake messages, the system prevents censors from identifying VPN protocols through their initial connection patterns.

Handshake obfuscation is essential in regions where active probing is widely deployed.

---

## Purpose

Handshake obfuscation enables:

- Resistance to active probing  
- Avoidance of handshake-based protocol detection  
- Protection against replay attacks  
- Compatibility with TLS, QUIC, and HTTP entrypoints  
- Integration with real-site mimicry and TLS fingerprint camouflage  
- Indistinguishable error behavior  

This ensures that handshake traffic cannot be used to identify or block the system.

---

## Threat Model

Active probing systems attempt to:

- Initiate fake connections  
- Replay captured handshakes  
- Send malformed handshake messages  
- Measure timing and packet size patterns  
- Detect protocol-specific responses  

Handshake obfuscation prevents these attacks.

---

## Handshake Obfuscation Techniques

### **1. Delayed Handshake Initiation**
Introduce randomized delays before responding:

- Prevents timing-based fingerprinting  
- Mimics real server load behavior  
- Avoids deterministic response patterns  

Delays must be small enough to avoid user impact.

---

### **2. Padding and Fragmentation**
Handshake messages are padded or fragmented:

- Prevents size-based fingerprinting  
- Mimics real TLS/QUIC fragmentation  
- Obscures protocol-specific message boundaries  

Padding must match real browser behavior.

---

### **3. Conditional Handshake Acceptance**
The server only proceeds with handshake if:

- Client fingerprint matches expected profile  
- SNI is valid  
- ALPN is acceptable  
- Timing behavior is consistent with real clients  

Invalid clients receive normal-looking errors.

---

### **4. Error Behavior Normalization**
Error responses must match real servers:

- TLS alert codes  
- HTTP error pages  
- QUIC connection close frames  
- Timing and packet size of error responses  

This prevents active probing from detecting anomalies.

---

### **5. Replay Protection**
Replay attacks are mitigated by:

- Nonces  
- Session tokens  
- One-time handshake parameters  
- Rejecting repeated ClientHello messages  

Replay protection prevents fingerprint harvesting.

---

### **6. Protocol Confusion Layer**
Optional layer that:

- Wraps handshake in an additional obfuscation layer  
- Mimics other protocols (e.g., WebSocket, HTTP/2)  
- Forces DPI to misclassify traffic  

Useful in high-risk regions.

---

## Integration

Handshake obfuscation integrates with:

- **tls-fingerprint.md**  
  Ensures handshake matches real browser behavior.

- **real-site-mimicry.md**  
  Ensures handshake aligns with real websites.

- **sni-randomization.md**  
  Ensures SNI behavior is consistent with mimicry.

- **entrypoints/**  
  TLS, QUIC, and HTTP entrypoints rely on handshake obfuscation.

- **fallback/**  
  Region-specific fallback may switch obfuscation profiles.

---

## Security Considerations

Handshake obfuscation must:

- Avoid static obfuscation patterns  
- Randomize timing and padding  
- Prevent replayable handshake flows  
- Reject malformed or suspicious handshakes  
- Avoid exposing backend infrastructure  

---

## Summary

Handshake obfuscation protects the system from active probing and handshake-based DPI detection.  
By delaying, padding, fragmenting, and normalizing handshake behavior—and by rejecting suspicious clients—the system becomes highly resistant to protocol fingerprinting and active probing attacks.
