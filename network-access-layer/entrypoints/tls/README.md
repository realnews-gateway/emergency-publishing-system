# TLS Entrypoints

TLS entrypoints provide covert access to the VPN Access Layer using encrypted HTTPS‑like traffic.  
They are designed to blend into legitimate TLS flows, resist DPI fingerprinting, and survive active probing.

This subsystem includes Trojan‑style TLS, XTLS variants, and Reality‑style real‑site mimicry.

---

## Purpose

TLS entrypoints enable:

- Covert access using HTTPS‑like traffic  
- Real‑site mimicry with genuine certificates  
- Browser‑grade TLS fingerprint shaping  
- Resistance to TLS‑based DPI and active probing  
- Compatibility with CDN‑fronted or domain‑fronted flows  
- Seamless integration with camouflage and session-init layers  

TLS is the most widely accepted and least suspicious transport on the modern internet, making it a critical entrypoint type.

---

## TLS Entrypoint Types

### **1. Trojan‑style TLS**
- Standard HTTPS handshake  
- Encrypted payload after handshake  
- Mimics normal web traffic  
- Easy to deploy and widely compatible  

### **2. XTLS / XTLS‑Vision**
- Optimized for performance  
- Reduces overhead for encrypted streams  
- Can mimic browser TLS behavior  

### **3. Reality‑style TLS (Real‑site mimicry)**
- Uses genuine certificates from real domains  
- Matches real websites’ TLS fingerprints  
- Extremely difficult for DPI to distinguish  
- Ideal for high‑risk regions  

---

## Key Features

- **Genuine TLS camouflage**  
  Entrypoints must match real browser fingerprints (Chrome, Safari, Firefox).

- **SNI flexibility**  
  Supports:
  - static SNI  
  - rotating SNI  
  - domain‑fronted SNI  

- **Certificate strategies**  
  - Real certificates (preferred)  
  - ACME‑issued certificates  
  - CDN‑compatible certificates  

- **Active probing resistance**  
  - No static signatures  
  - No distinguishable error messages  
  - Rejects unauthorized handshakes gracefully  

- **Traffic normalization**  
  Packet sizes and timing resemble real HTTPS traffic.

---

## Integration

TLS entrypoints integrate with:

- **camouflage/**  
  - TLS fingerprint shaping  
  - SNI randomization  
  - Real‑site mimicry  
  - Handshake obfuscation  

- **session-init/**  
  - TLS‑first negotiation  
  - Key exchange  
  - Bootstrap flow  

- **fallback/**  
  - TLS → QUIC → HTTP → CDN fallback chains  
  - Region‑specific TLS strategies  

- **client-profiles/**  
  - macOS/iOS prefer TLS-first  
  - Windows/Linux hybrid strategies  

---

## Security Considerations

TLS entrypoints must:

- Avoid static TLS fingerprints  
- Use indistinguishable handshake behavior  
- Prevent metadata leakage  
- Support replay‑safe handshake flows  
- Maintain compatibility with region‑specific DPI patterns  

---

## Summary

TLS entrypoints provide the most universal and stealthy access path into the VPN Access Layer.  
By combining real‑site mimicry, browser‑grade TLS fingerprints, and active probing resistance, they offer a resilient and highly covert transport suitable for hostile network environments.
