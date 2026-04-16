# QUIC Entrypoints

QUIC entrypoints provide high‑performance, censorship‑resistant access to the **network-access-layer** using UDP‑based encrypted transport.  
They are designed to mimic legitimate QUIC traffic—such as video streaming, gaming, and CDN traffic—while offering strong resistance to throttling and DPI interference.

This subsystem includes **tuic v5‑based QUIC**, obfuscated QUIC flows, and traffic‑normalized QUIC variants.

---

## Purpose

QUIC entrypoints enable:

- High‑performance access with low latency  
- UDP‑based transport that bypasses TCP throttling  
- Traffic patterns resembling real QUIC applications  
- Resistance to TLS‑based DPI  
- Compatibility with mobile and desktop clients  
- Seamless fallback to TLS or HTTP when QUIC is blocked  

QUIC is ideal for regions where TLS is heavily inspected or throttled.

---

## QUIC Entrypoint Types

### **1. tuic v5 QUIC**
- Modern QUIC transport implementation  
- Congestion control optimized for unstable and mobile networks  
- Traffic patterns similar to video streaming and interactive applications  
- Strong DPI evasion due to QUIC’s encrypted headers  
- Improved multiplexing and adaptive performance compared to legacy designs  

### **2. Obfuscated QUIC**
- Adds timing jitter, padding, and packet shaping  
- Mimics gaming or CDN traffic  
- Harder for DPI to classify  

### **3. QUIC-over-CDN (optional)**
- QUIC tunneled through CDN providers  
- Extremely stealthy but requires careful configuration  

---

## Key Features

- **Encrypted headers**  
  QUIC encrypts most metadata, reducing DPI visibility.

- **Traffic normalization**  
  QUIC flows can mimic:
  - YouTube streaming  
  - Cloudflare CDN traffic  
  - Online gaming packets  

- **Congestion control tuning**  
  Supports:
  - BBR  
  - Cubic  
  - tuic v5 adaptive mode  

- **Active probing resistance**  
  QUIC entrypoints must:
  - Reject unauthorized handshakes  
  - Avoid static response patterns  
  - Use indistinguishable error behavior  

- **Fallback compatibility**  
  QUIC → TLS → HTTP → CDN fallback chains are supported.

---

## Integration

QUIC entrypoints integrate with:

- **camouflage/**  
  - QUIC traffic normalization  
  - Packet size/timing obfuscation  
  - QUIC fingerprint shaping  

- **session-init/**  
  - QUIC-first negotiation for Android/Linux  
  - Key exchange over QUIC handshake  

- **fallback/**  
  - QUIC throttling detection  
  - Automatic switch to TLS or HTTP  

- **client-profiles/**  
  - Android/Linux prefer QUIC-first  
  - Windows/macOS use hybrid strategies  

---

## Security Considerations

QUIC entrypoints must:

- Avoid static QUIC fingerprints  
- Randomize packet timing and padding  
- Prevent replay attacks  
- Resist QUIC handshake probing  
- Maintain compatibility with region-specific blocking patterns  

---

## Summary

QUIC entrypoints provide a fast, resilient, and stealthy access path into the **network-access-layer**.  
By combining tuic v5 performance, QUIC traffic normalization, and strong DPI resistance, they offer a powerful alternative to TLS—especially in regions where TLS is throttled or heavily inspected.
