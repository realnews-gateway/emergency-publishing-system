# Transport and Camouflage Negotiation

The negotiation phase determines how a client and server agree on the transport protocol, camouflage profile, and fallback strategy before establishing a full session.  
This phase is designed to be covert, adaptive, and resistant to fingerprinting or active probing.

Negotiation is the first step of session initialization and directly influences the security and stealth of the entire connection.

---

## Purpose

Transport and camouflage negotiation enables:

- Selection of the optimal entrypoint (TLS, QUIC, HTTP, CDN)  
- Adaptation to regional censorship conditions  
- Alignment with platform-specific client profiles  
- Dynamic selection of camouflage techniques  
- Seamless fallback when primary transports fail  
- Covert negotiation indistinguishable from normal traffic  

This ensures that each session begins with the most resilient and stealthy configuration.

---

## Negotiation Flow Overview

The negotiation process consists of four stages:

1. **Initial Probe**  
   - Client performs a lightweight, browser-like probe  
   - Server responds with mimicry-consistent behavior  
   - No identifiable metadata is exchanged  

2. **Transport Capability Exchange**  
   - Client advertises supported transports  
   - Server advertises available entrypoints  
   - Exchange is embedded inside normal-looking traffic  

3. **Camouflage Profile Selection**  
   - TLS fingerprint profile (Chrome/Safari/Firefox)  
   - SNI strategy (static, rotating, domain-fronted)  
   - Handshake obfuscation level  
   - Traffic normalization mode  

4. **Session Confirmation**  
   - Both sides commit to selected parameters  
   - Key exchange begins  
   - Full session is established  

---

## Transport Negotiation

Transport selection is influenced by:

### **1. Platform Defaults**
- Android/Linux → QUIC-first  
- macOS/iOS → TLS-first  
- Windows → Hybrid strategy  

### **2. Region-Aware Logic**
Transport selection adapts to:

- QUIC throttling  
- TLS fingerprint blocking  
- SNI filtering  
- CDN interference  
- HTTP-only environments  

### **3. Entrypoint Availability**
Server may expose:

- TLS entrypoints  
- QUIC entrypoints  
- HTTP/2 entrypoints  
- CDN-backed entrypoints  

Client selects the best available option.

---

## Camouflage Negotiation

Camouflage profiles include:

### **1. TLS Fingerprint Profile**
- Chrome-style  
- Safari-style  
- Firefox-style  
- Custom enterprise-style (optional)  

### **2. SNI Strategy**
- Static SNI  
- Rotating SNI  
- Domain-fronted SNI  
- Decoy SNI  

### **3. Handshake Obfuscation Level**
- Minimal  
- Standard  
- High-risk region mode  

### **4. Traffic Normalization Mode**
- Browser-like  
- CDN-like  
- Video-stream-like  
- Gaming-like  

---

## Covert Negotiation Techniques

Negotiation must be indistinguishable from normal traffic:

### **1. Embedded Negotiation**
Negotiation data is hidden inside:

- TLS extensions  
- QUIC transport parameters  
- HTTP/2 pseudo-headers  
- CDN-compatible metadata  

### **2. Timing Camouflage**
Negotiation messages follow:

- Browser-like timing  
- CDN-like request patterns  
- Realistic idle intervals  

### **3. Error Normalization**
Invalid negotiation attempts receive:

- Realistic TLS alerts  
- HTTP error pages  
- QUIC close frames  

This prevents active probing detection.

---

## Integration

Negotiation integrates with:

- **key-exchange.md**  
  Begins immediately after negotiation completes.

- **authentication.md**  
  Authentication tokens may be exchanged during negotiation.

- **camouflage/**  
  Negotiated camouflage profiles determine TLS/SNI/handshake behavior.

- **fallback/**  
  Negotiation determines fallback chains.

- **client-profiles/**  
  Platform-specific negotiation behavior is defined here.

---

## Security Considerations

Negotiation must:

- Avoid static negotiation patterns  
- Prevent replayable negotiation messages  
- Normalize timing and packet sizes  
- Resist active probing  
- Avoid exposing backend infrastructure  
- Support region-aware negotiation strategies  

---

## Summary

Transport and camouflage negotiation determines how each session begins, ensuring that the client and server agree on the most secure and stealthy configuration.  
By embedding negotiation inside normal-looking traffic and adapting to regional conditions, the system achieves strong resistance to censorship and active probing.
