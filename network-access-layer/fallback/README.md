# Fallback Subsystem

The Fallback subsystem defines how clients recover from blocked, throttled, or degraded network conditions.  
It provides region‑aware fallback chains, transport downgrade logic, and error normalization to ensure that the system remains reachable even under aggressive censorship.

Fallback is a core component of censorship resistance and must operate covertly, adaptively, and without exposing detectable patterns.

---

## Purpose

Fallback enables:

- Recovery from blocked transports (QUIC, TLS, HTTP/2)  
- Region‑specific fallback strategies  
- Seamless downgrade from QUIC → TLS → HTTP/2 → CDN  
- Covert error handling and retry logic  
- Avoidance of detectable retry patterns  
- Integration with negotiation, camouflage, and bootstrap flows  

Fallback ensures that clients can always reach the server, even in hostile network environments.

---

## Design Principles

### **1. Region‑Aware Behavior**
Different regions require different fallback strategies:

- QUIC-blocking regions  
- TLS fingerprint filtering regions  
- SNI-blocking regions  
- CDN-only regions  
- High-risk active probing regions  

Fallback chains are defined per region.

---

### **2. Progressive Downgrade**
Fallback follows a structured downgrade path:

1. QUIC-first (if supported)  
2. TLS (Chrome/Safari fingerprints)  
3. HTTP/2 (browser-like)  
4. CDN-backed domain-fronted mode  
5. Final minimal HTTP-only mode  

Each step must mimic real-world application behavior.

---

### **3. Covert Retry Logic**
Retry behavior must avoid:

- Fixed intervals  
- Identifiable timing patterns  
- Burst retries  
- Static backoff sequences  

Retry timing is randomized within realistic browser/app ranges.

---

### **4. Error Normalization**
All fallback-triggering errors must resemble:

- TLS alerts  
- QUIC close frames  
- HTTP error pages  
- CDN gateway responses  

This prevents censors from fingerprinting fallback behavior.

---

## Fallback Chain Structure

A fallback chain consists of:

- **Primary transport**  
- **Secondary transport**  
- **Tertiary transport**  
- **CDN fallback**  
- **Last-resort HTTP-only mode**  

Each step includes:

- Transport  
- Camouflage profile  
- Retry timing  
- Error normalization rules  
- Region-specific overrides  

Fallback chains are defined in `chains.md`.

---

## Region Profiles

Region profiles define:

- Which transports are likely blocked  
- Which TLS fingerprints are safe  
- Whether QUIC is throttled  
- Whether SNI filtering is active  
- Whether CDN is required  

Profiles are defined in `region-profiles.md`.

---

## Integration

Fallback integrates with:

- **session-init/**  
  Bootstrap failures trigger fallback transitions.

- **camouflage/**  
  Each fallback step selects a new camouflage profile.

- **entrypoints/**  
  Fallback determines which entrypoint to use next.

- **client-profiles/**  
  Platform-specific fallback behavior is applied.

---

## Security Considerations

Fallback must:

- Avoid static retry patterns  
- Normalize all error responses  
- Prevent fallback loops  
- Avoid exposing backend infrastructure  
- Adapt to regional censorship conditions  
- Blend into legitimate browser/app behavior  

---

## Summary

The Fallback subsystem provides adaptive, covert, and region-aware recovery mechanisms that ensure continuous connectivity under censorship.  
By combining progressive downgrade, error normalization, and region-specific strategies, it enables clients to remain reachable even in the most restrictive environments.
