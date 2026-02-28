# Architecture Overview 

This document provides a high‑level overview of the Emergency Publishing System architecture. It describes the system’s core components, data flow, module boundaries, and deployment considerations. The architecture is designed to ensure reliable content delivery under censorship, network instability, and adversarial interference.

---

## System Identity

The Emergency Publishing System is a censorship‑resistant publishing architecture designed for high‑risk environments. It enables secure ingestion, processing, storage, and distribution of critical information. The system is not a VPN or messaging tool; it is a publishing and distribution platform optimized for adversarial networks.

All modules integrate into a unified core subsystem: the **Emergency Channel**.

---

## High‑Level Architecture

The system is composed of four major layers:

1. **Client Applications**  
   Mobile apps, browser extensions, and CLI tools that submit or retrieve content.

2. **Emergency Channel**  
   The system’s core processing layer, responsible for:
   - Content sanitization  
   - Metadata minimization  
   - Chunking and redundancy  
   - Region‑aware routing  
   - Storage coordination  
   - Multi‑transport distribution  

3. **Transport Architecture**  
   A modern, censorship‑resistant transport stack built on six protocols and organized into three operational layers.

4. **Global Distribution Layer**  
   Provides regional caching, opportunistic synchronization, and delay‑tolerant storage.

---

## Ingestion Architecture

Two ingestion paths feed the Emergency Channel:

- **News Aggregation Path**  
  Collects external news sources, normalizes content, and prepares it for secure distribution.

- **Anonymous BBS Path**  
  Accepts pseudonymous user submissions, sanitizes text, strips metadata, and enforces safety constraints.

Both ingestion paths converge into the Emergency Channel for unified processing.

---

## Storage Architecture

The storage layer ensures continuity during outages through:

- Local caching nodes  
- Delay‑tolerant bundles  
- Region‑aware retention  
- Redundant chunk distribution  
- Metadata minimization  

The Emergency Channel orchestrates storage behavior across regions.

---

## Distribution Architecture

The distribution layer delivers content across diverse network conditions:

- Low‑risk regions use high‑performance transports  
- Medium‑risk regions use camouflaged transports  
- High‑risk regions rely on emergency transports and opportunistic sync  
- Offline environments use bundle‑based distribution  

This ensures that content remains accessible even under severe censorship.

---

## Protocol Families

The system integrates six protocols, each providing distinct evasion and performance characteristics:

- **REALITY** — certificate camouflage  
- **uTLS** — Chrome/Firefox fingerprint mimicry  
- **XTLS‑Vision** — dynamic padding and statistical DPI evasion  
- **XHTTP** — HTTP/3‑like behavioral camouflage (Stream and Packet modes)  
- **VLESS** — universal carrier layer  
- **TUIC v5** — high‑performance UDP transport  

These protocols operate as layered modes within the transport architecture.

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

## Deployment Architecture

Deployment models include:

- Standard Deployment  
- Resilient Deployment  
- High‑Risk Deployment  
- Offline & Hybrid Deployment  

Each model enables or disables specific transport layers based on regional conditions.

---

## Summary

The Emergency Publishing System integrates ingestion, processing, storage, and distribution into a unified architecture centered on the Emergency Channel. The system’s resilience is achieved through multi‑protocol transport, region‑aware routing, redundant storage, and a three‑layer transport model designed for adversarial environments.
