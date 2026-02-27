# Architecture Overview

This document provides a high‑level overview of the Emergency Publishing System architecture. It aligns with the repository’s structure and the three‑layer transport model used across all modules. The system is designed to deliver critical information reliably under censorship, network instability, and adversarial interference.

---

## System Identity

The Emergency Publishing System is a censorship‑resistant publishing architecture designed for high‑risk environments. It provides secure ingestion, processing, and distribution of critical information under hostile network conditions. All modules integrate into a unified, resilient core subsystem: the Emergency Channel.

The architecture emphasizes multi‑protocol transport, region‑aware routing, redundant storage, behavioral camouflage, and minimal metadata exposure.

---

## Protocol Families

The system integrates six protocols, each providing distinct evasion and performance characteristics:

- REALITY — certificate camouflage  
- uTLS — Chrome/Firefox fingerprint mimicry  
- XTLS‑Vision — dynamic padding and statistical DPI evasion  
- XHTTP — HTTP/3‑like behavioral camouflage (Stream and Packet modes)  
- VLESS — universal carrier layer  
- TUIC v5 — high‑performance UDP transport  

These protocols operate as layered modes within the three‑layer transport architecture.

---

## Three‑Layer Transport Architecture

### Performance Layer (TCP)
Mode 1: VLESS + REALITY + uTLS + XTLS‑Vision  
Mode 2: VLESS + REALITY + uTLS + XHTTP (Stream)  
(Mode 2 uses TCP transport with HTTP/3‑like behavioral camouflage.)

### High‑Performance UDP Layer
TUIC v5 (optimized for 5G switching and low‑latency scenarios)

### Emergency Layer (extreme environments)
XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise  
TUIC v5 + Cloudflare Spectrum

These definitions must remain consistent across all architecture documents.

---

## Routing Architecture

Routing is performed by the Emergency Channel and includes region‑aware routing rules, transport scoring, stateless routing decisions, and adaptive behavior based on network conditions.

### Layer‑Level Fallback

```
[1] Performance Layer (TCP)
      ↓ if TCP is blocked or heavily degraded
[2] High‑Performance UDP Layer (TUIC v5)
      ↓ if UDP is also blocked or unstable
[3] Emergency Layer (extreme environments)
```

This ensures clean separation of transport families and maximum survivability under censorship.

---

## Storage Architecture

The storage layer ensures continuity during outages through:

- Local caching nodes  
- Delay‑tolerant bundles  
- Region‑aware retention  
- Redundant chunk distribution  
- Metadata minimization  

---

## Deployment Architecture

Deployment models include:

- Standard Deployment  
- Resilient Deployment  
- High‑Risk Deployment  
- Offline & Hybrid Deployment  

Each model enables or disables specific transport layers based on regional conditions.

---

## Summary

The Emergency Publishing System integrates six protocols, three transport layers, region‑aware routing, and redundant storage into a unified architecture. The Emergency Channel coordinates all components to ensure reliable content delivery across diverse and adversarial environments. The three‑layer transport model and layer‑level fallback logic form the foundation of the system’s resilience.
