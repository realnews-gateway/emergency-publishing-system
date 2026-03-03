# Camouflage Specification

The Camouflage subsystem provides the core techniques that make all VPN traffic indistinguishable from legitimate internet traffic.  
It defines the fingerprinting, mimicry, randomization, and obfuscation strategies used across TLS, QUIC, HTTP, and CDN entrypoints.

Camouflage is the foundation of censorship resistance in the VPN Access Layer.

---

## Purpose

The Camouflage subsystem enables:

- Real-site mimicry for high-value domain alignment  
- TLS fingerprint camouflage to match real browsers  
- SNI randomization to evade domain-based filtering  
- Handshake obfuscation to resist active probing  
- Traffic normalization to resemble legitimate web traffic  
- Integration with entrypoints and fallback strategies  

This subsystem ensures that all traffic blends into the surrounding network environment.

---

## Camouflage Components

The subsystem is composed of four major components:

### **1. Real-Site Mimicry**
Defined in `real-site-mimicry.md`.

Provides:

- Genuine certificate alignment  
- Real website TLS fingerprint matching  
- Traffic pattern simulation  
- Indistinguishable error behavior  

Used heavily by TLS and CDN entrypoints.

---

### **2. TLS Fingerprint Camouflage**
Defined in `tls-fingerprint.md`.

Provides:

- Chrome/Safari/Firefox fingerprint matching  
- Cipher suite ordering  
- Extension ordering  
- ALPN behavior  
- GREASE values  
- Key share behavior  

Prevents JA3/JA4 fingerprinting and TLS-based DPI.

---

### **3. SNI Randomization**
Defined in `sni-randomization.md`.

Provides:

- Static SNI mimicry  
- Rotating SNI pools  
- Domain-fronted SNI for CDN flows  
- Decoy SNI behavior  
- Region-aware domain selection  

Prevents SNI-based blocking and domain blacklisting.

---

### **4. Handshake Obfuscation**
Defined in `handshake-obfuscation.md`.

Provides:

- Delayed handshake initiation  
- Padding and fragmentation  
- Conditional handshake acceptance  
- Error behavior normalization  
- Replay protection  
- Protocol confusion layer  

Protects against active probing and handshake-based detection.

---

## Integration with Other Subsystems

Camouflage integrates with:

### **Entrypoints**
- TLS entrypoints rely on real-site mimicry and TLS fingerprint camouflage.  
- QUIC entrypoints use traffic normalization and handshake obfuscation.  
- HTTP entrypoints use header randomization and domain-fronted SNI.  
- CDN entrypoints rely on domain-fronting and real-site mimicry.

### **Session Initialization**
- Negotiation flows must align with browser-like behavior.  
- Key exchange must avoid detectable patterns.

### **Fallback**
- Region-specific fallback chains may switch camouflage profiles.  
- SNI pools and fingerprints must adapt to local blocking patterns.

### **Client Profiles**
- Platform-specific fingerprints (Chrome, Safari, Firefox).  
- OS-specific TLS/QUIC behavior alignment.

---

## Security Considerations

Camouflage must:

- Avoid static fingerprints or mimicry profiles  
- Rotate domains, fingerprints, and timing patterns  
- Prevent replayable handshake flows  
- Normalize error behavior  
- Resist active probing and DPI classification  
- Avoid exposing backend infrastructure  

Camouflage is a dynamic subsystem and must evolve with browser updates and censorship techniques.

---

## Summary

The Camouflage subsystem provides the core techniques that make VPN traffic indistinguishable from legitimate internet traffic.  
By combining real-site mimicry, TLS fingerprint camouflage, SNI randomization, and handshake obfuscation, the system achieves strong resistance to DPI, active probing, and domain-based filtering across all entrypoints.
