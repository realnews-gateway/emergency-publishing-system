# TLS Fingerprint Camouflage

TLS fingerprint camouflage ensures that all TLS-based entrypoints mimic the behavior of real browsers and real websites.  
This prevents censors from identifying VPN traffic using JA3/JA4 fingerprints, TLS extension ordering, cipher suite patterns, or handshake anomalies.

TLS fingerprint camouflage is a critical component of censorship resistance in regions where TLS-based DPI is widely deployed.

---

## Purpose

TLS fingerprint camouflage enables:

- TLS handshakes indistinguishable from Chrome, Safari, and Firefox  
- Resistance to JA3/JA4 fingerprinting  
- Avoidance of TLS-based traffic classification  
- Compatibility with CDN-backed and real-site mimicry flows  
- Protection against active probing  
- Seamless integration with TLS entrypoints  

This ensures that TLS traffic appears identical to legitimate HTTPS traffic.

---

## Components of TLS Fingerprint Camouflage

### **1. Cipher Suite Ordering**
The order of cipher suites must match real browsers:

- Chrome-style ordering  
- Safari-style ordering  
- Firefox-style ordering  

Incorrect ordering is a major fingerprinting vector.

---

### **2. Extension Ordering**
TLS extensions must appear in the same order as real browsers:

- SNI  
- ALPN  
- Supported Groups  
- Signature Algorithms  
- Key Share  
- Padding  
- GREASE values  

Extension ordering is one of the strongest fingerprinting signals.

---

### **3. ALPN Behavior**
ALPN must match real browser preferences:

- `h2`  
- `http/1.1`  

Some browsers prefer `h2`, others send both.

---

### **4. Supported Groups**
Elliptic curve groups must match real browser behavior:

- `x25519`  
- `secp256r1`  
- `secp384r1`  

Incorrect group ordering is easily fingerprinted.

---

### **5. Signature Algorithms**
Signature algorithms must match real browser lists:

- `rsa_pss_rsae_sha256`  
- `ecdsa_secp256r1_sha256`  
- `rsa_pkcs1_sha256`  

Browsers send long lists; VPNs often send short lists, which is detectable.

---

### **6. GREASE Values**
Browsers use GREASE (randomized extension values) to prevent ossification.

VPN traffic **must** include GREASE values to avoid looking synthetic.

---

### **7. Key Share Behavior**
Key share must match browser behavior:

- Chrome sends only `x25519`  
- Firefox sends multiple groups  
- Safari uses platform-specific behavior  

Incorrect key share behavior is a fingerprint.

---

### **8. TLS Version Negotiation**
Browsers negotiate:

- TLS 1.3  
- TLS 1.2 fallback  

VPNs must mimic this behavior.

---

## Integration

TLS fingerprint camouflage integrates with:

- **real-site-mimicry.md**  
  Ensures TLS handshake matches real websites.

- **sni-randomization.md**  
  Ensures SNI behavior matches browser expectations.

- **handshake-obfuscation.md**  
  Protects against active probing.

- **entrypoints/tls/**  
  TLS entrypoints rely heavily on fingerprint camouflage.

- **client-profiles/**  
  macOS/iOS require Safari-style fingerprints.  
  Windows/Linux often use Chrome-style fingerprints.

---

## Security Considerations

TLS fingerprint camouflage must:

- Avoid static fingerprints  
- Rotate fingerprint profiles  
- Match browser updates (Chrome/Safari/Firefox)  
- Prevent replayable handshake patterns  
- Avoid exposing backend infrastructure  

---

## Summary

TLS fingerprint camouflage ensures that VPN TLS traffic is indistinguishable from real browser traffic.  
By matching cipher suite ordering, extension ordering, ALPN behavior, GREASE values, and key share patterns, the system becomes highly resistant to TLS-based DPI, JA3/JA4 fingerprinting, and active probing.
