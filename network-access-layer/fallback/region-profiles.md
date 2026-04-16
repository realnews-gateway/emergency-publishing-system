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
- Risk classification (open, moderate, high, active-probing, mobile, enterprise)  

Profiles are updated as censorship conditions evolve.

---

## Region Classification Model

Regions are classified into six categories:

1. **Open Regions**  
2. **Moderate Censorship Regions**  
3. **High Censorship Regions**  
4. **Active Probing Regions**  
5. **Mobile Priority Regions**  
6. **Enterprise/Corporate Regions**

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

---

### **2. QUIC‑Blocking Region Profile**

    risk_level: moderate
    quic_status: blocked
    tls_fingerprint_safety: chrome or safari
    sni_filtering: partial
    cdn_access: available
    recommended_chain: quic-blocking
    notes: QUIC is throttled or silently dropped.

---

### **3. TLS Fingerprint Filtering Region Profile**

    risk_level: high
    quic_status: throttled
    tls_fingerprint_safety: rotating
    sni_filtering: partial
    cdn_access: available
    recommended_chain: tls-fingerprint-rotation
    notes: Chrome fingerprints may be blocked; rotation required.

---

### **4. SNI‑Blocking Region Profile**

    risk_level: high
    quic_status: throttled
    tls_fingerprint_safety: safari or ech-enabled
    sni_filtering: strict
    cdn_access: required
    recommended_chain: sni-blocking
    notes: ECH or CDN domain-fronting required.

---

### **5. Active Probing Region Profile**

    risk_level: active-probing
    quic_status: blocked
    tls_fingerprint_safety: safari (low entropy)
    sni_filtering: strict
    cdn_access: degraded
    recommended_chain: active-probing
    notes: Challenge/response authentication required.

---

### **6. Mobile Priority Region Profile**

    risk_level: mobile
    quic_status: allowed but unstable
    tls_fingerprint_safety: mobile-browser fingerprints
    sni_filtering: partial
    cdn_access: available
    recommended_chain: mobile-priority
    notes: Wider retry intervals and jitter handling required.

Characteristics:

- High latency and jitter  
- QUIC tuic v5 preferred  
- Retry timing randomized with wider ranges  

---

### **7. Enterprise/Corporate Region Profile**

    risk_level: enterprise
    quic_status: throttled or proxied
    tls_fingerprint_safety: reality-style mimicry
    sni_filtering: partial
    cdn_access: available
    recommended_chain: enterprise
    notes: DPI and proxy filtering common; TLS Reality preferred.

Characteristics:

- Corporate DPI and proxy interception  
- TLS Reality with genuine certificates  
- HTTP/2 API-like camouflage  

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
Extensions for **Mobile Priority Regions** and **Enterprise Regions** provide specialized strategies for mobile networks and corporate environments, ensuring resilience across diverse conditions.
