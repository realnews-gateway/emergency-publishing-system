# Protocols Overview

This document provides a high‑level overview of the transport architecture
used by the Empus Emergency Publishing System. The system relies on six
widely deployed transport protocols and combines them into three
operational layers designed to withstand diverse censorship and network
conditions.

Empus does not modify or redefine these protocols.  
Instead, it composes them into layered configurations optimized for
performance, resilience, and extreme censorship resistance.

---

## Six‑Protocol Transport Stack

Empus uses six existing protocols as its transport foundation:

- **REALITY — certificate camouflage**  
  Mimics legitimate TLS certificates to evade SNI‑based and
  certificate‑based filtering.

- **uTLS — Chrome/Firefox fingerprint mimicry**  
  Reproduces real browser TLS fingerprints to blend into normal encrypted
  traffic.

- **XTLS‑Vision — dynamic padding and statistical DPI evasion**  
  Uses adaptive padding and traffic shaping to resist statistical,
  flow‑based, and timing‑based inspection.

- **XHTTP — HTTP/3‑like behavioral camouflage**  
  Emulates HTTP/3‑style behavior with two modes:  
  - **Stream Mode** — long‑lived, multiplexed flows  
  - **Packet Mode** — discrete packet‑like behavior for extreme
    censorship environments

- **VLESS — universal carrier layer**  
  A flexible, metadata‑minimal carrier used as the base layer for most
  TCP‑based configurations.

- **TUIC v5 — high‑performance UDP transport**  
  Optimized for low latency, mobile switching, and unstable networks.

These protocols serve as building blocks for the system’s operational
layers.

---

## Three Operational Layers

Empus organizes protocol usage into three operational layers. Each layer
corresponds to a different censorship environment and performance
requirement.

### 1. Performance Layer (TCP)

Optimized for high throughput and stable networks.  
Uses VLESS as the carrier and applies multiple camouflage layers:

- **VLESS + REALITY + uTLS + XTLS‑Vision**  
- **VLESS + REALITY + uTLS + XHTTP (Stream)**

Characteristics:

- Strong TLS/HTTP camouflage  
- High throughput  
- Stable long‑lived connections  
- Resistant to active probing and basic DPI

---

### 2. High‑Performance UDP Layer

Focused on low latency and mobile network resilience:

- **TUIC v5** (optimized for low‑latency and mobile switching)

Characteristics:

- Fast failover  
- Path migration  
- High performance on unstable or mobile networks  
- Efficient for real‑time or bursty workloads

---

### 3. Emergency Layer (Extreme Censorship)

Designed for severe blocking, active interference, and partial blackouts:

- **VLESS + XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise**  
- **TUIC v5 + Cloudflare Spectrum**

Characteristics:

- Maximum indistinguishability from high‑value web traffic  
- Deep integration with CDN infrastructures  
- Survives aggressive throttling, probing, and protocol‑level blocking  
- Suitable for high‑risk or blackout scenarios

---

## Summary

The Empus transport architecture is defined by:

- A **Six‑Protocol Transport Stack** providing the building blocks  
- A **Three‑Layer Operational Model** adapting to censorship conditions  
- Layered combinations that prioritize camouflage, resilience, and
  performance

Detailed descriptions of the six protocols and the three operational
layers are provided in the accompanying documents:

- `source-protocols.md`  
- `operational-layers.md`
