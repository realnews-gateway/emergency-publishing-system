> **Note:** This repository was previously named `realnews-free-publish-core`.  
> It has been renamed to reflect the system’s expanded scope as a full censorship‑resistant publishing architecture.

# Emergency Publishing System

The Emergency Publishing System is a censorship‑resistant publishing architecture designed for high‑risk and adversarial environments. It provides secure ingestion, sanitization, encrypted storage, and multi‑path distribution of critical information, ensuring that public‑interest content remains accessible even under severe network interference. All subsystems converge into a unified and resilient core: the **Emergency Channel**.

This repository contains the complete architecture, documentation, roadmap, and module structure for review and development.

---

## Repository Structure

### Core System Layers

- **network-access-layer/**  
  Access Layer — censorship‑resistant entry layer providing multi‑protocol transport paths.

- **emergency-channel/**  
  Processing Layer — responsible for sanitization, metadata minimization, routing logic, and preparing content for storage and distribution.

- **storage/**  
  Storage Layer — encrypted, tamper‑resistant, region‑aware storage for processed content.

- **distribution/**  
  Distribution Layer — multi‑path, multi‑region delivery system supporting fallback and covert distribution modes.

---

### Supporting Directories

- **architecture/**  
  High‑level system design, threat model, data flow, and deployment strategies.

- **modules/**  
  Optional ingestion and processing modules that integrate with the Emergency Channel:  
  - news‑aggregation  
  - anonymous‑bbs  

- **docs/**  
  Supplemental documentation, conceptual notes, and supporting materials.

- **roadmap/**  
  Structured development plan:  
  - milestones.md  
  - deliverables.md  
  - timeline.md  
  - README.md (overview)

---

### Root Files

- README.md  
- LICENSE  
- SECURITY.md  
- CONTRIBUTING.md  
- CODEOFCONDUCT.md  
- .gitignore  

---

## Core Concepts

### Emergency Channel

The Emergency Channel is the system’s central processing and distribution core, responsible for:

- Content sanitization  
- Metadata minimization  
- **Encrypted, tamper‑resistant storage preparation**  
- **Multi‑path distribution scheduling**  
- Region‑aware routing  
- Transport selection and fallback logic  
- Covert and emergency delivery modes  

All ingestion paths—news aggregation or pseudonymous posts—flow into this subsystem, where raw content is transformed into publishable, censorship‑resilient output.

---

## Network Access Layer

The **network-access-layer** provides the system’s censorship‑resistant transport foundation.  
It replaces legacy VPN‑based models with a modern, multi‑protocol architecture designed for adversarial environments.

### Six‑Protocol Transport Stack

- REALITY — certificate camouflage  
- uTLS — Chrome/Firefox fingerprint mimicry  
- XTLS‑Vision — dynamic padding and statistical DPI evasion  
- XHTTP — HTTP/3‑like behavioral camouflage (Stream and Packet modes)  
- VLESS — universal carrier layer  
- TUIC v5 — high‑performance UDP transport  

### Three Operational Layers

- **Performance Layer (TCP)**  
  - VLESS + REALITY + uTLS + XTLS‑Vision  
  - VLESS + REALITY + uTLS + XHTTP (Stream)

- **High‑Performance UDP Layer**  
  - TUIC v5 (optimized for low‑latency and mobile switching)

- **Emergency Layer (extreme censorship)**  
  - XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise  
  - TUIC v5 + Cloudflare Spectrum  

Transport selection adapts dynamically to censorship intensity and network conditions.

---

## Documentation Overview

Key documents are located in **architecture/**:

- system-overview.md  
- data-flow.md  
- protocol-integration.md  
- security-design.md  
- deployment-models.md  

Additional materials are in **docs/**.

---

## Roadmap

The **roadmap/** directory provides a structured development plan:

- milestones.md  
- deliverables.md  
- timeline.md  

These outline system evolution, review phases, and deployment readiness.

---

## Development Guidelines

Contributions follow the principles defined in:

- CONTRIBUTING.md  
- SECURITY.md  
- CODEOFCONDUCT.md  

The system emphasizes:

- Security‑first design  
- Metadata minimization  
- Modular architecture  
- Region‑aware behavior  
- Clear and stable documentation  

---

## Summary

The Emergency Publishing System is a resilient, censorship‑resistant publishing platform built around the Emergency Channel. It provides:

- Secure ingestion  
- Content sanitization  
- **Encrypted storage**  
- **Multi‑path distribution**  
- Region‑aware routing  
- A modern censorship‑resistant transport foundation (network‑access-layer)  

Its modular architecture and transport‑layer resilience support long‑term operation and deployment in high‑risk environments.
