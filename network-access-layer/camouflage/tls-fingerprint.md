
# TLS Fingerprint Camouflage

TLS fingerprint camouflage ensures that all TLS-based entrypoints mimic the behavior of real browsers and websites.  
This prevents censors from identifying Empus traffic using JA3/JA4 fingerprints, TLS extension ordering, cipher suite patterns, or handshake anomalies.  
It is a critical component of censorship resistance in regions where TLS-based DPI is widely deployed.

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

## Components

### 1. Cipher Suite Ordering
- Must match real browser ordering (Chrome, Safari, Firefox).  
- Incorrect ordering is a major fingerprinting vector.

### 2. Extension Ordering
- Extensions must appear in the same order as real browsers:  
  SNI, ALPN, Supported Groups, Signature Algorithms, Key Share, Padding, GREASE.  
- Extension ordering is one of the strongest fingerprinting signals.

### 3. ALPN Behavior
- Must match browser preferences (`h2`, `http/1.1`).  
- Some browsers prefer `h2`, others send both.

### 4. Supported Groups
- Must match real browser behavior: `x25519`, `secp256r1`, `secp384r1`.  
- Incorrect group ordering is easily fingerprinted.

### 5. Signature Algorithms
- Must match browser lists: `rsa_pss_rsae_sha256`, `ecdsa_secp256r1_sha256`, `rsa_pkcs1_sha256`.  
- Browsers send long lists; short lists are detectable.

### 6. GREASE Values
- Browsers include GREASE (randomized extension values).  
- Empus traffic must include GREASE to avoid looking synthetic.

### 7. Key Share Behavior
- Chrome sends only `x25519`.  
- Firefox sends multiple groups.  
- Safari uses platform-specific behavior.  
- Incorrect key share behavior is a fingerprint.

### 8. TLS Version Negotiation
- Browsers negotiate TLS 1.3 with TLS 1.2 fallback.  
- Empus must mimic this behavior.

---

## Integration

TLS fingerprint camouflage integrates with:
- **real-site-mimicry.md** — ensures TLS handshake matches real websites  
- **sni-randomization.md** — ensures SNI behavior matches browser expectations  
- **handshake-obfuscation.md** — protects against active probing  
- **entrypoints/tls/** — TLS entrypoints rely heavily on fingerprint camouflage  
- **client-profiles/** — platform-specific fingerprints (Safari for macOS/iOS, Chrome for Windows/Linux)  

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

TLS fingerprint camouflage ensures that Empus TLS traffic is indistinguishable from real browser traffic.  
By matching cipher suite ordering, extension ordering, ALPN behavior, GREASE values, and key share patterns, the system becomes highly resistant to TLS-based DPI, JA3/JA4 fingerprinting, and active probing.
