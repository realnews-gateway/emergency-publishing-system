
# SNI Randomization

SNI (Server Name Indication) randomization is a camouflage technique that prevents censors from blocking Empus traffic based on domain names observed during the TLS handshake.  
By rotating, obfuscating, or fronting SNI values, the system avoids static domain fingerprints and reduces the effectiveness of SNI-based filtering.  
This is essential in regions where SNI filtering is widely deployed.

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

## Techniques

### 1. Static SNI (Baseline Mimicry)
- SNI matches the real domain being mimicked  
- Certificate chain matches the domain  
- Traffic patterns align with the real site  

### 2. Rotating SNI (Dynamic Mimicry)
- SNI selected from a pool of legitimate domains (news, e-commerce, CDN assets, global services)  
- Rotation strategies: per-connection, per-session, time-based, region-aware  
- Prevents static fingerprints and domain blacklisting  

### 3. Domain-Fronted SNI
- SNI points to a front domain (e.g., `cdn.example.com`)  
- Host header points to hidden backend  
- TLS terminates at CDN edge nodes  
- Extremely stealthy, widely used in high-risk regions  

### 4. Decoy SNI
- SNI set to a harmless domain while real traffic is tunneled inside  
- Appears as normal HTTPS traffic  
- Backend routing hidden  
- Works well with HTTP/2 and CDN flows  

---

## Domain Pool Management

A robust system requires:
- Large pool of legitimate domains  
- Region-specific lists  
- Automatic removal of blocked domains  
- Periodic refresh  
- CDN-compatible selection  

Domains must be high-value, frequently accessed, and difficult to block without collateral damage.

---

## Integration

SNI randomization integrates with:
- **real-site-mimicry.md** — ensures SNI matches real domains when mimicking  
- **tls-fingerprint.md** — ensures SNI behavior matches browser expectations  
- **handshake-obfuscation.md** — protects SNI behavior from active probing  
- **entrypoints/tls/** — TLS entrypoints rely heavily on SNI camouflage  
- **entrypoints/cdn/** — domain-fronted SNI is essential for CDN-backed flows  
- **fallback/** — region-specific fallback may switch SNI strategies  

---

## Security Considerations

SNI randomization must:
- Avoid static values  
- Prevent predictable rotation patterns  
- Use domain pools that are difficult to block  
- Reject malformed or suspicious SNI values  
- Avoid exposing backend infrastructure  

---

## Summary

SNI randomization prevents censors from blocking Empus traffic based on domain names observed during the TLS handshake.  
By rotating values, using domain-fronted flows, and aligning with real-site mimicry, the system becomes highly resistant to SNI-based filtering and domain blacklisting.
