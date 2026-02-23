# Emergency Publishing System

The Emergency Publishing System is a censorship-resistant communication architecture designed for high-risk environments. It provides secure ingestion, processing, and distribution of critical information under hostile network conditions. The system integrates multiple modules into a unified, resilient core: the Emergency Channel.

This repository contains the full architecture, roadmap, documentation, and module structure for development and review.

---

## Repository Structure

### Core Directories

- **architecture/**  
  High-level system design, data flow, security model, and deployment strategies.

- **emergency-channel/**  
  Core subsystem responsible for sanitization, routing, storage, and multi-transport distribution.

- **modules/**  
  Independent modules that integrate with the Emergency Channel:
    - **vpn-access-layer** (upgraded to a modern six‑protocol transport stack: REALITY, uTLS, XTLS‑Vision, XHTTP, VLESS, TUIC v5)  
    - news-aggregation  
    - anonymous-bbs  

- **docs/**  
  Additional documentation, conceptual notes, and supporting materials.

- **roadmap/**  
  Structured development plan:
    - milestones.md  
    - deliverables.md  
    - timeline.md  
    - README.md (overview)

- **.github/**  
  GitHub workflows, issue templates, and repository automation.

---

## Core Concepts

### Emergency Channel
The central processing system responsible for:
- Content sanitization  
- Metadata minimization  
- Secure storage  
- Region-aware routing  
- Multi-transport delivery  
- Fallback and covert communication paths  

All content—news ingestion or pseudonymous posts—flows through this subsystem.

### Ingestion Modules
Two primary ingestion paths feed the Emergency Channel:
- News Aggregation Path  
- Anonymous BBS Path  

Both normalize, sanitize, and prepare content for secure distribution.

---

## Transport Layer (Upgraded)

The **vpn-access-layer** has been upgraded to a modern, censorship‑resistant transport stack built on six protocols:

- **REALITY** — certificate camouflage  
- **uTLS** — Chrome/Firefox fingerprint mimicry  
- **XTLS‑Vision** — dynamic padding and statistical DPI evasion  
- **XHTTP** — HTTP/3‑like behavioral camouflage (Stream and Packet modes)  
- **VLESS** — universal carrier layer  
- **TUIC v5** — high‑performance UDP (Rust implementation invoked from Go)

### Three‑Layer Architecture (brief overview)

- **Performance Layer (TCP)**  
  - Mode 1: VLESS + REALITY + uTLS + XTLS‑Vision  
  - Mode 2: VLESS + REALITY + uTLS + XHTTP (Stream)  
  *(Mode 2 uses TCP transport with HTTP/3‑like behavioral camouflage.)*

- **High‑Performance UDP Layer**  
  - TUIC v5 (optimized for 5G switching and low‑latency scenarios)

- **Emergency Layer (extreme environments)**  
  - XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise  
  - TUIC v5 + Cloudflare Spectrum

Transport selection adapts to censorship intensity and network conditions.

---

## Documentation Overview

Key documents are located in the **architecture/** directory:

- system-overview.md  
- data-flow.md  
- protocol-integration.md  
- security-design.md  
- deployment-models.md  

These define the system’s behavior, threat model, and operational strategies.

Additional materials are in **docs/**.

---

## Roadmap

The **roadmap/** directory provides a structured development plan:

- milestones.md  
- deliverables.md  
- timeline.md  

This outlines system evolution, review phases, and deployment readiness.

---

## Development Guidelines

Contributions follow the principles defined in:

- CONTRIBUTING.md  
- SECURITY.md  
- CODEOFCONDUCT.md  

The system emphasizes:
- Security-first design  
- Metadata minimization  
- Modular architecture  
- Region-aware behavior  
- Clear and stable documentation  

---

## License

This project is licensed under the terms described in LICENSE.

---

## Summary

The Emergency Publishing System provides a resilient, censorship-resistant communication platform built around the Emergency Channel. Its modular architecture, structured roadmap, and comprehensive documentation support long-term development, review, and deployment in high-risk environments.

The **vpn-access-layer** has been upgraded to a modern six‑protocol transport stack to ensure reliable delivery even under extreme censorship.
