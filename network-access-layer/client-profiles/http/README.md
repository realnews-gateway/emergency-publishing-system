# HTTP Entrypoints

HTTP entrypoints provide covert access to the VPN Access Layer using HTTP/1.1 and HTTP/2 traffic patterns.  
They are designed to blend into normal web traffic, support domain‑fronting techniques, and offer a fallback path when TLS or QUIC are blocked.

This subsystem includes XHTTP, HTTP/2 domain‑fronted access, and CDN‑compatible HTTP flows.

---

## Purpose

HTTP entrypoints enable:

- Covert access using HTTP/1.1 or HTTP/2  
- Domain‑fronted access through major CDNs  
- Compatibility with restrictive firewalls that block TLS/QUIC  
- Traffic patterns that resemble normal web browsing  
- Seamless fallback when higher‑performance transports fail  
- Integration with camouflage and fallback layers  

HTTP is often the last protocol standing in heavily censored environments.

---

## HTTP Entrypoint Types

### **1. XHTTP (Extended HTTP Transport)**
- Encapsulates encrypted payloads inside HTTP requests  
- Mimics browser request patterns  
- Supports header randomization and padding  
- Works well in environments with strict TLS inspection  

### **2. HTTP/2 Domain‑Fronted Access**
- Uses CDN‑hosted domains as the visible endpoint  
- Real traffic is routed through CDN to the hidden backend  
- Extremely stealthy but requires compatible CDN configuration  

### **3. HTTP-over-CDN**
- HTTP traffic tunneled through Cloudflare, Fastly, or Akamai  
- Appears identical to normal CDN‑served web content  
- Useful when TLS/QUIC are throttled or fingerprinted  

---

## Key Features

- **Browser‑like request patterns**  
  HTTP entrypoints must mimic:
  - Chrome/Safari request headers  
  - Accept‑Language patterns  
  - User‑Agent strings  
  - Cookie behavior (optional)  

- **Header randomization**  
  Prevents static fingerprints by rotating:
  - header order  
  - casing  
  - optional fields  

- **Payload encapsulation**  
  Encrypted data is hidden inside:
  - POST bodies  
  - chunked transfer encoding  
  - HTTP/2 streams  

- **Domain‑fronting compatibility**  
  Supports:
  - SNI → front domain  
  - Host header → hidden backend  

- **Fallback integration**  
  HTTP is often used as:
  - TLS → QUIC → HTTP → CDN  
  - QUIC → TLS → HTTP → CDN  

---

## Integration

HTTP entrypoints integrate with:

- **camouflage/**  
  - HTTP header randomization  
  - Browser‑grade request patterns  
  - HTTP/2 fingerprint shaping  

- **session-init/**  
  - HTTP‑based negotiation  
  - Key exchange encapsulated in HTTP requests  

- **fallback/**  
  - HTTP as fallback when TLS/QUIC fail  
  - Region‑specific HTTP strategies  

- **client-profiles/**  
  - iOS/macOS use HTTP as fallback  
  - Android/Linux use HTTP as last resort  

---

## Security Considerations

HTTP entrypoints must:

- Avoid static header fingerprints  
- Prevent replayable request patterns  
- Normalize timing to resemble real browsing  
- Resist active probing by rejecting malformed requests  
- Minimize metadata leakage in headers  

---

## Summary

HTTP entrypoints provide a stealthy and resilient fallback path into the VPN Access Layer.  
By mimicking real browser traffic, supporting domain‑fronting, and encapsulating encrypted payloads inside HTTP flows, they remain effective even in environments where TLS and QUIC are heavily restricted.
