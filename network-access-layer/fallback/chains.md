# Fallback Chains

Fallback chains define the ordered sequence of transports, camouflage profiles, and retry strategies used when a client encounters blocked, throttled, or degraded network conditions.  
Each chain is region‑aware, platform‑aware, and designed to mimic legitimate browser/application behavior.

Fallback chains ensure that clients remain reachable even under aggressive censorship.

---

## Structure of a Fallback Chain

A fallback chain consists of multiple ordered stages:

1. **Primary Transport**  
2. **Secondary Transport**  
3. **Tertiary Transport**  
4. **CDN Fallback**  
5. **Minimal HTTP‑Only Mode**  

Each stage defines:

- Transport  
- Camouflage profile  
- Retry timing  
- Error normalization rules  
- Region-specific overrides  
- Platform-specific overrides  

Fallback chains must avoid static patterns and remain indistinguishable from normal application retry behavior.

---

## Default Global Fallback Chain

This chain is used in regions without known aggressive censorship.

### **1. QUIC (Primary)**
- Transport: QUIC (tuic v5)  
- Camouflage: Chrome QUIC fingerprint  
- Retry: randomized 200–600 ms  
- Error normalization: QUIC close frames  

### **2. TLS (Secondary)**
- Transport: TLS 1.3  
- Camouflage: Chrome/Safari fingerprints, Reality-style mimicry  
- Retry: randomized 300–900 ms  
- Error normalization: TLS alerts  

### **3. HTTP/2 (Tertiary)**
- Transport: HTTP/2 over TLS  
- Camouflage: browser-like pseudo-headers  
- Retry: randomized 500–1200 ms  
- Error normalization: HTTP error pages  

### **4. CDN Fallback**
- Transport: HTTPS via CDN  
- Camouflage: domain-fronted SNI  
- Retry: randomized 800–1500 ms  
- Error normalization: CDN gateway responses  

### **5. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  
- Camouflage: generic browser headers  
- Retry: randomized 1–3 seconds  
- Error normalization: generic HTTP failures  

---

## QUIC‑Blocking Region Chain

Used in regions where QUIC is throttled or blocked.

### **1. TLS (Primary)**
- Transport: TLS 1.3  
- Camouflage: Chrome/Safari fingerprints  
- Retry: 200–600 ms  

### **2. HTTP/2 (Secondary)**
- Transport: HTTP/2  
- Camouflage: browser-like  
- Retry: 300–900 ms  

### **3. CDN Fallback**
- Transport: HTTPS via CDN  
- Camouflage: domain-fronted  
- Retry: 600–1200 ms  

### **4. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  
- Retry: 1–3 seconds  

---

## TLS Fingerprint Filtering Region Chain

Used in regions where specific TLS fingerprints are blocked.

### **1. QUIC (Primary)**
- Transport: QUIC (tuic v5)  
- Camouflage: Chrome QUIC  

### **2. TLS (Alternate Fingerprint)**
- Transport: TLS 1.3  
- Camouflage: Firefox or Edge fingerprints  

### **3. HTTP/2**
- Transport: HTTP/2  
- Camouflage: browser-like  

### **4. CDN Fallback**
- Transport: HTTPS via CDN  

### **5. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  

---

## SNI‑Blocking Region Chain

Used in regions where SNI filtering is active.

### **1. QUIC (Primary)**
- Transport: QUIC (tuic v5)  
- Camouflage: Encrypted ClientHello (ECH) if available  

### **2. TLS with ECH**
- Transport: TLS 1.3  
- Camouflage: ECH-enabled fingerprints  

### **3. CDN Domain‑Fronting**
- Transport: HTTPS via CDN  
- Camouflage: fronted SNI  

### **4. HTTP/2 (Fronted)**
- Transport: HTTP/2  
- Camouflage: CDN pseudo-headers  

### **5. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  

---

## High‑Risk Active Probing Region Chain

Used in regions with aggressive active probing.

### **1. TLS (Primary)**
- Transport: TLS 1.3  
- Camouflage: Safari fingerprint (low entropy)  
- Authentication: challenge/response required  

### **2. HTTP/2**
- Transport: HTTP/2  
- Camouflage: browser-like  

### **3. CDN Fallback**
- Transport: HTTPS via CDN  
- Authentication: challenge/response  

### **4. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  

QUIC is avoided because it is easily probed.

---

## Mobile Priority Chain (Extension)

Optimized for unstable mobile networks with high latency and jitter.

### **1. QUIC (tuic v5 Primary)**
- Transport: QUIC tuic v5  
- Camouflage: Chrome QUIC fingerprint  
- Retry: 300–900 ms (wider range for jitter)  

### **2. TLS (Secondary)**
- Transport: TLS 1.3 Reality-style  
- Camouflage: mobile browser fingerprints  
- Retry: 500–1200 ms  

### **3. CDN Fallback**
- Transport: HTTPS via CDN  
- Retry: 1–2 seconds  

### **4. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  

---

## Enterprise Chain (Extension)

Optimized for corporate networks with DPI and proxy filtering.

### **1. TLS Reality (Primary)**
- Transport: TLS 1.3 with real-site mimicry  
- Camouflage: enterprise-grade fingerprints  
- Retry: 200–600 ms  

### **2. HTTP/2 (Secondary)**
- Transport: HTTP/2  
- Camouflage: API-like traffic  

### **3. CDN Fallback**
- Transport: HTTPS via CDN  
- Retry: 800–1500 ms  

### **4. Minimal HTTP‑Only Mode**
- Transport: HTTP/1.1  

---

## Retry Timing Rules

Retry timing must:

- Be randomized  
- Avoid fixed intervals  
- Avoid exponential backoff patterns  
- Match real browser/app behavior  

Examples:

- QUIC retries: 200–600 ms (mobile: 300–900 ms)  
- TLS retries: 300–900 ms  
- HTTP/2 retries: 500–1200 ms  
- CDN retries: 800–1500 ms  
- Minimal HTTP retries: 1–3 seconds  

---

## Error Normalization Rules

Each fallback stage must normalize errors to match real-world behavior:

- QUIC → QUIC close frames  
- TLS → TLS alerts  
- HTTP/2 → HTTP error pages  
- CDN → CDN gateway responses  
- HTTP/1.1 → generic HTTP failures  

No custom error messages are allowed.

---

## Summary

Fallback chains define the ordered, region-aware downgrade paths used to maintain connectivity under censorship.  
By combining transport transitions, camouflage changes, randomized retry timing, and error normalization, fallback chains ensure that clients remain reachable while blending into legitimate traffic patterns.  
Extensions such as **Mobile Priority Chain** and **Enterprise Chain** provide specialized strategies for mobile and corporate environments, further strengthening resilience.
