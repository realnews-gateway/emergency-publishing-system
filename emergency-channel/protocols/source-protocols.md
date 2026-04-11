# Source Protocols

This document describes the six transport protocols used by the Empus
Emergency Publishing System. These protocols are not modified or
redefined by Empus; instead, they serve as building blocks for the
system’s operational layers.

Each protocol contributes a distinct form of camouflage, resilience, or
transport efficiency. Their combinations are documented in
`operational-layers.md`.

---

## 1. REALITY — Certificate Camouflage

REALITY provides TLS certificate camouflage by presenting server
certificates that mimic legitimate, high‑value websites. This enables:

- Evasion of SNI‑based filtering  
- Resistance to certificate fingerprinting  
- Indistinguishability from normal HTTPS traffic  

REALITY is used as a front‑layer camouflage mechanism in TCP‑based
configurations.

---

## 2. uTLS — Browser Fingerprint Mimicry

uTLS reproduces real TLS ClientHello fingerprints from major browsers
such as Chrome and Firefox. This allows Empus traffic to blend into
normal encrypted traffic flows.

Key properties:

- Mimics widely deployed browser fingerprints  
- Avoids detection by TLS fingerprinting systems  
- Enhances compatibility with CDN‑based camouflage  

uTLS is typically paired with REALITY and VLESS.

---

## 3. XTLS‑Vision — Dynamic Padding & Statistical DPI Evasion

XTLS‑Vision provides adaptive padding and traffic shaping to resist
statistical and flow‑based inspection.

Capabilities include:

- Dynamic padding to obscure packet size patterns  
- Flow‑level obfuscation to resist timing analysis  
- Reduced detectability under statistical DPI  

XTLS‑Vision is used in high‑performance TCP configurations.

---

## 4. XHTTP — HTTP/3‑Like Behavioral Camouflage

XHTTP emulates HTTP/3‑style behavior and supports two modes:

- **Stream Mode** — long‑lived, multiplexed flows resembling normal
  HTTP/3 traffic  
- **Packet Mode** — discrete packet‑like behavior for extreme censorship
  environments  

XHTTP provides:

- Modern web‑traffic camouflage  
- Compatibility with CDN infrastructures  
- Flexible behavior for both normal and hostile networks  

Packet Mode is used in the Emergency Layer.

---

## 5. VLESS — Universal Carrier Layer

VLESS serves as a lightweight, metadata‑minimal carrier protocol. It is
used as the base layer for most TCP‑based configurations.

Characteristics:

- Minimal metadata footprint  
- Flexible routing and transport integration  
- High compatibility with camouflage layers (REALITY, uTLS, XHTTP)  

VLESS is the foundation of the Performance Layer and Emergency Layer.

---

## 6. TUIC v5 — High‑Performance UDP Transport

TUIC v5 is a modern UDP‑based transport optimized for:

- Low latency  
- Mobile network switching  
- Path migration  
- High throughput under unstable conditions  

TUIC v5 is the primary protocol used in the High‑Performance UDP Layer
and also appears in the Emergency Layer when paired with Cloudflare
Spectrum.

---

## Summary

The six source protocols used by Empus are:

- REALITY  
- uTLS  
- XTLS‑Vision  
- XHTTP  
- VLESS  
- TUIC v5  

These protocols form the foundation of the system’s transport
architecture. Their combinations and operational roles are documented in
`operational-layers.md`.
