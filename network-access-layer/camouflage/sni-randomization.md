# SNI Randomization

SNI (Server Name Indication) randomization is a critical camouflage technique that prevents censors from blocking VPN traffic based on domain names observed during the TLS handshake.  
By rotating, obfuscating, or fronting SNI values, the system avoids static domain fingerprints and reduces the effectiveness of SNI-based filtering.

SNI randomization is essential in regions where SNI filtering is widely deployed.

---

## Purpose

SNI randomization enables:

- Avoidance of domain-based blocking  
- Resistance to SNI fingerprinting  
- Compatibility with CDN-backed entrypoints  
- Dynamic domain rotation to evade blacklists  
- Integration with real-site mimicry  
- Protection against active probing  

This ensures that TLS traffic cannot be easily classified or blocked based on SNI alone.

---

## SNI Randomization Techniques

### **1. Static SNI (Baseline Mimicry)**
Used when mimicking a specific real website:

- SNI matches the real domain  
- Certificate chain matches the domain  
- Traffic patterns align with the real site  

This is the simplest form of mimicry.

---

### **2. Rotating SNI (Dynamic Mimicry)**
SNI is selected from a pool of legitimate domains:

- News websites  
- E-commerce platforms  
- CDN-hosted assets  
- Popular global services  

Rotation strategies:

- Per-connection rotation  
- Per-session rotation  
- Time-based rotation  
- Region-aware rotation  

This prevents static SNI fingerprints.

---

### **3. Domain-Fronted SNI**
Used with CDN-backed entrypoints:

- SNI → front domain (e.g., `cdn.example.com`)  
- Host header → hidden backend  
- TLS terminates at CDN edge nodes  

This technique is extremely stealthy and widely used in high-risk regions.

---

### **4. Decoy SNI**
SNI is set to a harmless domain while real traffic is tunneled inside:

- Appears as normal HTTPS traffic  
- Backend routing is hidden  
- Works well with HTTP/2 and CDN flows  

---

## Domain Pool Management

A robust SNI randomization system requires:

- A large pool of legitimate domains  
- Region-specific domain lists  
- Automatic removal of blocked domains  
- Periodic domain refresh  
- CDN-compatible domain selection  

Domains must be:

- High-value  
- Frequently accessed  
- Difficult to block without collateral damage  

---

## Integration

SNI randomization integrates with:

- **real-site-mimicry.md**  
  Ensures SNI matches real domains when mimicking.

- **tls-fingerprint.md**  
  Ensures SNI behavior matches browser expectations.

- **handshake-obfuscation.md**  
  Protects SNI behavior from active probing.

- **entrypoints/tls/**  
  TLS entrypoints rely heavily on SNI camouflage.

- **entrypoints/cdn/**  
  Domain-fronted SNI is essential for CDN-backed flows.

- **fallback/**  
  Region-specific fallback may switch SNI strategies.

---

## Security Considerations

SNI randomization must:

- Avoid static SNI values  
- Prevent predictable rotation patterns  
- Use domain pools that are difficult to block  
- Avoid exposing backend infrastructure  
- Resist active probing by rejecting malformed SNI values  

---

## Summary

SNI randomization prevents censors from blocking VPN traffic based on domain names observed during the TLS handshake.  
By rotating SNI values, using domain-fronted flows, and aligning with real-site mimicry, the system becomes highly resistant to SNI-based filtering and domain blacklisting.
