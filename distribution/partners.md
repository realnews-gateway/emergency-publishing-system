
# Distribution Partners

The partners file defines external organizations and technical channels that assist in distributing content.  
Partners extend coverage and resilience but must operate within strict boundaries to prevent trust pollution.  
This document categorizes partner types, specifies metadata, and enforces sandbox rules.

---

## Objectives

- Expand distribution reach through external networks  
- Provide redundancy via mirrors and partner channels  
- Enforce strict sandbox rules to prevent unauthorized modifications  
- Ensure all partner‑distributed content remains verifiable  

---

## Partner Categories

### 1. Technical Partners
- **CDN Providers** (e.g., Cloudflare, Akamai, Fastly)  
- **Community Mirror Nodes** maintained by volunteers or open‑source communities  
- **Archival Platforms** such as Internet Archive for long‑term preservation  

### 2. Media & Information Partners
- Independent media organizations sharing content via API or subscription  
- NGOs and research institutions providing distribution in high‑risk regions  

### 3. Enterprise & Organizational Partners
- Corporate collaboration platforms for cross‑border distribution  
- Academic networks (universities, research institutions)  
- Local community organizations offering language‑specific distribution  

---

## Partner Metadata

Each partner entry includes:

```
id: Unique identifier
type: Category (CDN, media, NGO, enterprise, community)
region: Geographic coverage
reliability: Initial reliability score (0–1)
protocol: Distribution protocol (API, RSS, CDN sync)
sandbox: true/false (must be true to enforce sandbox rules)
```

Example:

```
- id: "cdn_cloudflare"
  type: "CDN"
  region: "global"
  reliability: 0.95
  protocol: "cdn-sync"
  sandbox: true
```

---

## Data Sandbox Rules

Partners operate in a **data sandbox**:

- Can only distribute signed static content  
- Cannot alter delivery order  
- Cannot insert unauthorized content  
- Must perform signature verification before redistribution  
- Any violation results in rejection and audit logging  

---

## Integration

- **pipeline.md** — Partners act as nodes in the distribution pipeline  
- **integrity.md** — Sandbox rules enforce signature verification  
- **fallback.md** — Partners provide alternate routes during failures  
- **monitoring.md** — Partner reliability and compliance are tracked  

---

## Summary

Distribution partners extend reach and resilience but must operate under strict sandbox rules.  
By enforcing signature validation and restricting partners to static signed content, the system prevents trust pollution while ensuring authenticity across all external channels.
