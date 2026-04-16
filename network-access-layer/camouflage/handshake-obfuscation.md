
# Handshake Obfuscation

This layer protects Empus from active probing, protocol fingerprinting, and handshake-based DPI detection.  
By modifying, delaying, padding, or restructuring handshake messages, it prevents censors from identifying protocols through initial connection patterns.  
Handshake obfuscation is critical in regions where active probing is widely deployed.

---

## Purpose

Handshake obfuscation enables:

- Resistance to active probing  
- Avoidance of handshake-based protocol detection  
- Protection against replay attacks  
- Compatibility with TLS, QUIC, and HTTP entrypoints  
- Integration with real-site mimicry and TLS fingerprint camouflage  
- Indistinguishable error behavior  

This ensures that handshake traffic cannot be used to identify or block Empus.

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

## Techniques

### 1. Delayed Handshake Initiation
Introduce randomized delays before responding:
- Prevents timing-based fingerprinting  
- Mimics real server load behavior  
- Avoids deterministic response patterns  

### 2. Padding and Fragmentation
Pad or fragment handshake messages:
- Prevents size-based fingerprinting  
- Mimics real TLS/QUIC fragmentation  
- Obscures protocol-specific boundaries  

### 3. Conditional Handshake Acceptance
Server proceeds only if:
- Client fingerprint matches expected profile  
- SNI is valid  
- ALPN is acceptable  
- Timing behavior is consistent  

Invalid clients receive normal-looking errors.

### 4. Error Behavior Normalization
Error responses must match real servers:
- TLS alert codes  
- HTTP error pages  
- QUIC connection close frames  
- Timing and packet size of error responses  

### 5. Replay Protection
Mitigate replay attacks with:
- Nonces  
- Session tokens  
- One-time handshake parameters  
- Rejecting repeated ClientHello messages  

### 6. Protocol Confusion Layer
Optional layer that:
- Wraps handshake in an additional obfuscation layer  
- Mimics other protocols (WebSocket, HTTP/2)  
- Forces DPI to misclassify traffic  

---

## Integration

Handshake obfuscation integrates with:

- **tls-fingerprint.md** — ensures handshake matches real browser behavior  
- **real-site-mimicry.md** — aligns handshake with real websites  
- **sni-randomization.md** — ensures SNI behavior consistency  
- **entrypoints/** — TLS, QUIC, and HTTP entrypoints rely on obfuscation  
- **fallback/** — region-specific fallback may switch obfuscation profiles  

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

Handshake obfuscation protects Empus from active probing and handshake-based DPI detection.  
By delaying, padding, fragmenting, and normalizing handshake behavior—and by rejecting suspicious clients—the system becomes highly resistant to protocol fingerprinting and active probing attacks.
