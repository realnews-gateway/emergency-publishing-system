# CDN Entrypoints

CDN entrypoints provide the most stealthy and globally resilient access path into the VPN Access Layer.  
By routing traffic through major Content Delivery Networks (CDNs) such as Cloudflare, Fastly, and Akamai, these entrypoints make VPN traffic indistinguishable from normal CDN‑served web content.

CDN entrypoints are essential for high‑risk regions where direct TLS or QUIC traffic is aggressively blocked or fingerprinted.

---

## Purpose

CDN entrypoints enable:

- Maximum stealth through CDN‑hosted domains  
- Global reach with distributed edge networks  
- Domain‑fronted access paths  
- Traffic patterns identical to real CDN‑served content  
- Resistance to IP blocking and regional filtering  
- Seamless fallback when TLS/QUIC/HTTP are blocked  

CDN entrypoints are the strongest option when operating under state‑level censorship.

---

## CDN Entrypoint Types

### **1. Cloudflare‑Backed Entrypoints**
- Uses Cloudflare edge nodes as the visible endpoint  
- Supports domain‑fronting via SNI/Host separation  
- Strong TLS camouflage due to Cloudflare’s fingerprint diversity  
- Ideal for regions with aggressive TLS DPI  

### **2. Fastly‑Backed Entrypoints**
- High‑performance CDN with stable global coverage  
- Supports HTTP/2 and TLS‑based domain‑fronting  
- Traffic resembles real media and API traffic  

### **3. Akamai‑Backed Entrypoints**
- Extremely large global footprint  
- Hard to block without collateral damage  
- Suitable for enterprise‑grade mimicry  

---

## Key Features

- **Domain‑fronting compatibility**  
  CDN entrypoints support:
  - SNI → front domain  
  - Host header → hidden backend  
  - CDN‑routed TLS termination  

- **Indistinguishable traffic patterns**  
  CDN entrypoints must mimic:
  - static asset delivery  
  - API calls  
  - video streaming metadata  
  - browser‑initiated HTTPS traffic  

- **IP rotation and global distribution**  
  Blocking a single CDN IP is ineffective due to:
  - dynamic edge routing  
  - large IP pools  
  - region‑specific edge nodes  

- **Active probing resistance**  
  CDN entrypoints must:
  - reject unauthorized requests like a normal CDN  
  - avoid custom error messages  
  - maintain indistinguishable handshake behavior  

- **Fallback integration**  
  CDN is typically the final fallback:
  - TLS → QUIC → HTTP → CDN  
  - QUIC → TLS → HTTP → CDN  

---

## Integration

CDN entrypoints integrate with:

- **camouflage/**  
  - TLS fingerprint shaping to match CDN behavior  
  - SNI/Host separation for domain‑fronting  
  - Real‑site mimicry using CDN‑compatible certificates  

- **session-init/**  
  - CDN‑compatible negotiation flows  
  - Key exchange encapsulated in CDN‑routed traffic  

- **fallback/**  
  - CDN as the ultimate fallback path  
  - Region‑specific CDN strategies  

- **client-profiles/**  
  - iOS/macOS rely heavily on CDN fallback  
  - Android/Linux use CDN as last resort  

---

## Security Considerations

CDN entrypoints must:

- Avoid exposing backend IPs  
- Prevent replayable CDN request patterns  
- Use CDN‑compatible TLS fingerprints  
- Normalize timing to match real CDN traffic  
- Resist active probing by behaving like a normal CDN endpoint  

---

## Summary

CDN entrypoints provide the highest level of stealth and resilience in the VPN Access Layer.  
By leveraging Cloudflare, Fastly, and Akamai as the visible access points, they make VPN traffic indistinguishable from normal CDN‑served content—offering unmatched protection in hostile network environments.
