
# Distribution Fallback

The fallback file defines strategies for ensuring reliable delivery when primary distribution channels fail.  
It provides redundancy, retry mechanisms, and degraded modes to guarantee that verified content reaches end‑users even under censorship or hostile network conditions.

---

## Objectives

- Maintain delivery continuity despite failures or censorship  
- Provide multiple redundant paths for distribution  
- Ensure integrity verification remains enforced during fallback  
- Log fallback events for later analysis and auditing  

---

## Fallback Triggers

Fallback is activated when:

- Primary channel failure (unreachable or error response)  
- High latency or repeated timeouts  
- Censorship indicators (connection resets, TLS interference, content filtering)  
- Network instability (packet loss, degraded bandwidth)  
- Health check failures on distribution endpoints  

---

## Fallback Strategies

### 1. Retry with Backoff
- Exponential backoff retries with jitter  
- Prevents overload on unstable networks  

### 2. Channel Switching
- Switch from direct push to feeds or mirrors  
- Automatically selects the healthiest available channel  

### 3. Mirror Failover
- Redirect delivery to CDN or community mirrors  
- Ensures continuity when primary servers are blocked  

### 4. Minimal Payload Mode
- Deliver text‑only micro‑feeds optimized for extreme censorship environments  
- Reduces bandwidth and bypasses filtering heuristics  

### 5. Opportunistic Delivery
- Attempt delivery during temporary network windows  
- Useful in unstable or high‑risk regions  

### 6. Offline Export
- Package content into bundles for manual or offline distribution  
- Enables delivery via physical or peer‑to‑peer networks  

---

## Monitoring and Logging

- Fallback events are logged with trigger type and resolution path  
- Latency and error rates tracked for each fallback attempt  
- Audit trails ensure accountability and performance analysis  

---

## Integration

- **pipeline.md** — Fallback is invoked when primary delivery fails  
- **integrity.md** — Verification enforced even during fallback delivery  
- **partners.md** — Partners may act as alternate channels under sandbox rules  
- **monitoring.md** — Tracks fallback frequency and effectiveness  

---

## Summary

Fallback strategies ensure that distribution remains reliable under adverse conditions.  
By combining retries, channel switching, mirror failover, minimal payload mode, opportunistic delivery, and offline export, the system guarantees that verified content reaches users even when networks are unstable or censored.
