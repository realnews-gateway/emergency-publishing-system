# Error Normalization

Error normalization ensures that all fallback-triggering failures resemble legitimate TLS, QUIC, HTTP, or CDN errors.  
This prevents censors from fingerprinting the system based on failure patterns, retry behavior, or protocol-level anomalies.

Error normalization is essential for stealth and censorship resistance.

---

## Purpose

Error normalization provides:

- Covert handling of transport failures  
- Realistic browser/app-like error patterns  
- Prevention of detectable fallback signatures  
- Consistent behavior across regions and transports  
- Integration with retry logic and fallback chains  

All errors must appear indistinguishable from real-world failures.

---

## Principles

### **1. No Custom Error Messages**
The system must never emit:

- Custom error codes  
- Custom error strings  
- Protocol anomalies  
- Distinctive timing patterns  

All errors must map to real-world equivalents.

---

### **2. Transport-Specific Normalization**
Each transport layer has its own error model:

- QUIC → close frames  
- TLS → alerts  
- HTTP/2 → HTTP error pages  
- CDN → gateway responses  
- HTTP/1.1 → generic browser failures  

Fallback transitions must use the correct error model.

---

### **3. Timing Normalization**
Error timing must:

- Match real browser/app behavior  
- Include randomized jitter  
- Avoid fixed intervals  
- Avoid exponential backoff patterns  

Timing randomness prevents fingerprinting.

---

## Normalized Error Types

### **1. QUIC Error Normalization**

QUIC failures must resemble:

- QUIC close frames  
- Connection timeout  
- Path validation failure  
- Version negotiation failure  

Example normalized errors:

    quic_close: CONNECTION_TIMEOUT
    quic_close: INTERNAL_ERROR
    quic_close: IDLE_TIMEOUT

No custom QUIC error codes are allowed.

---

### **2. TLS Error Normalization**

TLS failures must resemble:

- TLS alerts  
- Handshake failure  
- Certificate rejection  
- Version mismatch  

Example normalized alerts:

    tls_alert: handshake_failure
    tls_alert: protocol_version
    tls_alert: internal_error

TLS alerts must match real browser behavior.

---

### **3. HTTP/2 Error Normalization**

HTTP/2 failures must resemble:

- HTTP 4xx/5xx pages  
- Stream reset  
- Connection close  
- Timeout  

Example normalized responses:

    http2_error: 502 Bad Gateway
    http2_error: 503 Service Unavailable
    http2_error: stream_reset

These errors must be indistinguishable from real server failures.

---

### **4. CDN Error Normalization**

CDN failures must resemble:

- Gateway timeout  
- Origin unreachable  
- 5xx CDN-generated pages  

Example normalized responses:

    cdn_error: 504 Gateway Timeout
    cdn_error: 502 Bad Gateway
    cdn_error: origin_unreachable

CDN fallback must never reveal backend infrastructure.

---

### **5. Minimal HTTP‑Only Mode Errors**

In last-resort mode, errors must resemble:

- Browser connection timeout  
- DNS failure  
- Generic HTTP 4xx/5xx  

Example normalized responses:

    http_error: 408 Request Timeout
    http_error: 500 Internal Server Error
    http_error: dns_failure

These errors must appear generic and unremarkable.

---

## Retry Behavior Normalization

Retry behavior must:

- Use randomized intervals  
- Avoid fixed patterns  
- Avoid exponential backoff  
- Match real browser/app retry timing  

Examples:

    retry_interval_quic: 200–600 ms
    retry_interval_tls: 300–900 ms
    retry_interval_http2: 500–1200 ms
    retry_interval_cdn: 800–1500 ms
    retry_interval_http: 1–3 seconds

Randomness prevents timing-based fingerprinting.

---

## Integration

Error normalization integrates with:

- fallback/chains.md  
  Determines which normalized errors trigger fallback transitions.

- session-init/bootstrap-flow.md  
  Bootstrap failures must use normalized errors.

- camouflage/  
  Ensures error patterns match selected TLS/QUIC fingerprints.

- entrypoints/  
  Each entrypoint type has its own error model.

- region-profiles.md  
  Region-specific censorship patterns influence error normalization.

---

## Security Considerations

Error normalization must:

- Prevent fallback loops  
- Avoid revealing backend infrastructure  
- Avoid exposing transport capabilities  
- Resist active probing  
- Blend into legitimate traffic patterns  

Errors must never reveal internal state.

---

## Summary

Error normalization ensures that all failures resemble legitimate TLS, QUIC, HTTP, or CDN errors.  
By standardizing error behavior across transports and regions, the system prevents censors from fingerprinting fallback transitions and maintains strong censorship resistance.
