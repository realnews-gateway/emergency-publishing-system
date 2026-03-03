# Region Profiles

Region profiles define censorship characteristics, transport availability, TLS/QUIC/SNI filtering behavior, and recommended fallback chains for different network environments.  
These profiles allow the client to adapt its fallback strategy to local conditions while maintaining stealth and reliability.

Region profiles are used by the fallback subsystem, negotiation subsystem, and bootstrap flow.

---

## Purpose

Region profiles provide:

- Transport availability predictions  
- TLS fingerprint safety levels  
- QUIC throttling/blocking indicators  
- SNI filtering behavior  
- CDN accessibility  
- Recommended fallback chains  
- Risk classification (low, moderate, high, active-probing)  

Profiles are updated as censorship conditions evolve.

---

## Region Classification Model

Regions are classified into four categories:

1. **Open Regions**  
2. **Moderate Censorship Regions**  
3. **High Censorship Regions**  
4. **Active Probing Regions**

Each category maps to a specific fallback chain.

---

## Region Profile Fields

Each region profile includes:

- risk_level  
- quic_status  
- tls_fingerprint_safety  
- sni_filtering  
- cdn_access  
- recommended_chain  
- notes  

---

## Example Region Profiles

### **1. Open Region Profile**

    risk_level: open
    quic_status: allowed
    tls_fingerprint_safety: chrome
    sni_filtering: none
    cdn_access: available
    recommended_chain: global-default
    notes: Normal browser behavior is safe.

Characteristics:

- QUIC widely available  
- No TLS fingerprint filtering  
- No SNI filtering  
- CDN optional  

---

### **2. QUIC‑Blocking Region Profile**

    risk_level: moderate
    quic_status: blocked
    tls_fingerprint_safety: chrome or safari
    sni_filtering: partial
    cdn_access: available
    recommended_chain: quic-blocking
    notes: QUIC is throttled or silently dropped.

Characteristics:

- QUIC packets dropped or delayed  
- TLS allowed but fingerprint-sensitive  
- SNI filtering may occur intermittently  

---

### **3. TLS Fingerprint Filtering Region Profile**

    risk_level: high
    quic_status: throttled
    tls_fingerprint_safety: rotating
    sni_filtering: partial
    cdn_access: available
    recommended_chain: tls-fingerprint-rotation
    notes: Chrome fingerprints may be blocked; rotation required.

Characteristics:

- DPI blocks specific TLS fingerprints  
- QUIC may be degraded  
- SNI filtering inconsistent  

---

### **4. SNI‑Blocking Region Profile**

    risk_level: high
    quic_status: throttled
    tls_fingerprint_safety: safari or ech-enabled
    sni_filtering: strict
    cdn_access: required
    recommended_chain: sni-blocking
    notes: ECH or CDN domain-fronting required.

Characteristics:

- Strict SNI filtering  
- QUIC often throttled  
- CDN domain-fronting required  

---

### **5. Active Probing Region Profile**

    risk_level: active-probing
    quic_status: blocked
    tls_fingerprint_safety: safari (low entropy)
    sni_filtering: strict
    cdn_access: degraded
    recommended_chain: active-probing
    notes: Challenge/response authentication required.

Characteristics:

- Aggressive DPI  
- Active probing of suspected endpoints  
- QUIC unsafe  
- TLS must use low-entropy fingerprints  
- Authentication must use challenge/response  

---

## Region Detection

Region detection uses:

- Transport success/failure patterns  
- TLS handshake acceptance/rejection  
- QUIC packet loss patterns  
- SNI filtering behavior  
- CDN accessibility tests  
- Timing and RTT anomalies  

Detection is passive and avoids sending identifiable probes.

---

## Integration

Region profiles integrate with:

- fallback/chains.md  
- session-init/bootstrap-flow.md  
- camouflage/  
- entrypoints/  
- client-profiles/  

---

## Summary

Region profiles define censorship characteristics and guide the fallback subsystem in selecting the safest and most effective downgrade strategy.  
By modeling QUIC/TLS/SNI/CDN behavior and mapping regions to fallback chains, the system adapts dynamically to local censorship conditions while maintaining stealth and reliability.
