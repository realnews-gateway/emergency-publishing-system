# Real-Site Mimicry

Real-site mimicry is a core camouflage technique that makes VPN traffic indistinguishable from legitimate HTTPS traffic to real, high‑value websites.  
This technique significantly increases censorship resistance by aligning TLS behavior, certificate chains, and traffic patterns with real-world services.

Real-site mimicry is essential for operating in high-risk regions where TLS fingerprinting, SNI filtering, and active probing are common.

---

## Purpose

Real-site mimicry enables:

- TLS handshakes identical to real websites  
- Certificate chains that match legitimate domains  
- Traffic patterns resembling normal browsing  
- Resistance to JA3/JA4 fingerprinting  
- Protection against active probing  
- Compatibility with CDN-backed entrypoints  

This technique makes it extremely difficult for censors to distinguish VPN traffic from real HTTPS traffic.

---

## Components of Real-Site Mimicry

### **1. Certificate Alignment**
Entrypoints must use certificates that resemble real websites:

- Genuine certificates (preferred)  
- ACME-issued certificates  
- CDN-compatible certificates  
- Matching SAN (Subject Alternative Name) structure  
- Matching certificate validity periods  

### **2. TLS Fingerprint Matching**
TLS handshake parameters must match real browsers:

- Cipher suite ordering  
- ALPN preferences  
- Supported groups  
- Signature algorithms  
- Extension ordering  

This ensures compatibility with Chrome, Safari, and Firefox fingerprints.

### **3. Traffic Pattern Simulation**
Traffic must resemble real browsing:

- Packet size distribution  
- Timing jitter  
- Request/response patterns  
- Idle-time behavior  

This prevents traffic classifiers from identifying VPN flows.

### **4. SNI Behavior**
SNI must match the real domain being mimicked:

- Static SNI for stable mimicry  
- Rotating SNI for dynamic mimicry  
- Domain-fronted SNI for CDN-backed flows  

### **5. Error Behavior**
Error responses must match real websites:

- TLS alert codes  
- HTTP error pages  
- Connection reset timing  

This prevents active probing from detecting anomalies.

---

## Integration

Real-site mimicry integrates with:

- **tls-fingerprint.md**  
  Ensures TLS handshake matches real browser fingerprints.

- **sni-randomization.md**  
  Provides domain rotation strategies.

- **handshake-obfuscation.md**  
  Protects against active probing.

- **entrypoints/**  
  TLS and CDN entrypoints rely heavily on mimicry.

- **fallback/**  
  Region-specific fallback may switch between mimicry profiles.

---

## Security Considerations

Real-site mimicry must:

- Avoid static mimicry profiles  
- Rotate domains to prevent fingerprinting  
- Use indistinguishable error behavior  
- Prevent replayable handshake patterns  
- Avoid exposing backend infrastructure  

---

## Summary

Real-site mimicry provides one of the strongest forms of censorship resistance by making VPN traffic indistinguishable from legitimate HTTPS traffic.  
By aligning certificates, TLS fingerprints, SNI behavior, and traffic patterns with real websites, it becomes extremely difficult for censors to detect or block the system without causing massive collateral damage.
