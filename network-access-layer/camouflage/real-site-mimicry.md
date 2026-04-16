
# Real-Site Mimicry

Real-site mimicry is a core camouflage technique that makes Empus traffic indistinguishable from legitimate HTTPS traffic to high‑value websites.  
By aligning TLS behavior, certificate chains, and traffic patterns with real-world services, it significantly increases censorship resistance in environments where TLS fingerprinting, SNI filtering, and active probing are common.

---

## Purpose

Real-site mimicry enables:

- TLS handshakes identical to real websites  
- Certificate chains that match legitimate domains  
- Traffic patterns resembling normal browsing  
- Resistance to JA3/JA4 fingerprinting  
- Protection against active probing  
- Compatibility with CDN-backed entrypoints  

This technique makes it extremely difficult for censors to distinguish Empus traffic from real HTTPS flows.

---

## Components

### 1. Certificate Alignment
Entrypoints must use certificates that resemble real websites:
- Genuine certificates (preferred)  
- ACME-issued certificates  
- CDN-compatible certificates  
- Matching SAN (Subject Alternative Name) structures  
- Matching certificate validity periods  

### 2. TLS Fingerprint Matching
TLS handshake parameters must match real browsers:
- Cipher suite ordering  
- ALPN preferences  
- Supported groups  
- Signature algorithms  
- Extension ordering  

Ensures compatibility with Chrome, Safari, and Firefox fingerprints.

### 3. Traffic Pattern Simulation
Traffic must resemble real browsing:
- Packet size distribution  
- Timing jitter  
- Request/response patterns  
- Idle-time behavior  

Prevents classifiers from identifying VPN flows.

### 4. SNI Behavior
SNI must match the real domain being mimicked:
- Static SNI for stable mimicry  
- Rotating SNI for dynamic mimicry  
- Domain-fronted SNI for CDN-backed flows  

### 5. Error Behavior
Error responses must match real websites:
- TLS alert codes  
- HTTP error pages  
- Connection reset timing  

Prevents active probing from detecting anomalies.

---

## Integration

Real-site mimicry integrates with:
- **tls-fingerprint.md** — ensures TLS handshake matches real browser fingerprints  
- **sni-randomization.md** — provides domain rotation strategies  
- **handshake-obfuscation.md** — protects against active probing  
- **entrypoints/** — TLS and CDN entrypoints rely heavily on mimicry  
- **fallback/** — region-specific fallback may switch between mimicry profiles  

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

Real-site mimicry provides one of the strongest forms of censorship resistance by making Empus traffic indistinguishable from legitimate HTTPS traffic.  
By aligning certificates, TLS fingerprints, SNI behavior, and traffic patterns with real websites, it becomes extremely difficult for censors to detect or block the system without causing massive collateral damage.
