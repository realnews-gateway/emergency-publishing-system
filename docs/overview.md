# Overview

**Empus** is a censorship‑resistant publishing and distribution system designed to ensure that critical, public‑interest information can be safely transmitted, stored, and accessed under hostile network conditions.  
It provides a secure pathway for news ingestion, pseudonymous submissions, and resilient global distribution, even in regions facing severe censorship or network degradation.

Empus is not a traditional circumvention tool.  
Its focus is **publishing**, not personal access.  
The system ensures that essential information can be ingested, processed, and delivered—regardless of regional restrictions or adversarial interference.

---

## Core Components

Empus consists of four major components:

### 1. Emergency Channel  
The core subsystem responsible for sanitization, metadata minimization, chunking, redundancy, routing, storage coordination, and multi‑transport distribution.  
It unifies all modules and ensures consistent behavior across regions.

### 2. Access Layer  
A region‑aware entry layer that provides covert, censorship‑resistant access paths for clients.  
It integrates modern transports such as REALITY, uTLS, XTLS‑Vision, XHTTP, VLESS, and TUIC v5.

### 3. News Aggregation Module  
A resilient ingestion pipeline for independent journalism, mirror sources, and region‑aware content fetching.  
It normalizes, sanitizes, and standardizes external content before passing it to the Emergency Channel.

### 4. Anonymous BBS  
A lightweight, pseudonymous submission system designed for high‑risk environments.  
It strips metadata, enforces safety rules, and provides a minimal interface for community reporting.

---

## Purpose and Design Philosophy

Empus is designed as foundational infrastructure for information freedom.  
Its goals include:

- Ensuring that suppressed content can be safely published  
- Providing resilient distribution across adversarial networks  
- Maintaining availability during censorship events  
- Minimizing metadata and exposure for users  
- Operating reliably under unstable or intermittent connectivity  

The system emphasizes long‑term survivability, not short‑term circumvention.

---

## How Empus Differs from Traditional VPN/Proxy Tools

Empus is fundamentally different from VPNs, proxies, and “airport”‑style services:

- **Different purpose**  
  VPNs solve “access.” Empus solves “publishing” and “distribution.”

- **Different threat model**  
  Empus is designed for censorship, surveillance, and network disruption—not for general browsing privacy.

- **Different architecture**  
  Empus uses a multi‑layer transport stack with region‑aware routing, redundancy, and chunk‑based distribution.

- **Different guarantees**  
  Empus minimizes metadata, avoids persistent identifiers, and prioritizes survivability over throughput.

Empus is infrastructure for information flow under pressure, not a personal privacy tool.

---

## Summary

Empus provides a unified, censorship‑resistant system for ingesting, processing, storing, and distributing critical information.  
Its architecture is built around the Emergency Channel, supported by resilient ingestion modules and a modern transport stack.  
The system is purpose‑built for environments where traditional communication channels fail, ensuring that essential information continues to reach the world.
