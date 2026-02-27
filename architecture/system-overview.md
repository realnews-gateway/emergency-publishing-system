# System Overview

The Emergency Publishing System is a censorship‑resistant publishing architecture designed for high‑risk environments where network interference, surveillance, and targeted blocking are expected. Its purpose is to ensure that critical information can be ingested, sanitized, stored, and distributed reliably, even when adversaries attempt to suppress or manipulate access.

At the center of the system is the **Emergency Channel**, a unified processing and distribution core that provides secure, metadata‑minimized, multi‑protocol delivery of published content.

This document introduces the system’s high‑level structure, major components, and operational model.

---

## Core Architecture

The system is organized into three major layers that operate independently but integrate through stable, well‑defined contracts.

### 1. Ingestion Layer
Responsible for converting external sources and user‑generated submissions into normalized internal messages.

Modules include:

- **news-aggregation/** — fetches, parses, deduplicates, and classifies external news sources.
- **anonymous-bbs/** — provides pseudonymous posting with strict sanitization and metadata minimization.

All ingestion modules output a unified internal message format consumed by the Emergency Channel.

### 2. Emergency Channel (Core Processing Layer)
The **Emergency Channel** is the backbone of the entire system. It performs:

- Content sanitization and metadata stripping
- Region‑aware routing
- Multi‑protocol transport selection
- Storage and retrieval
- Fallback logic under censorship
- Multi‑hop delivery across heterogeneous networks

All content — regardless of origin — flows into and through the Emergency Channel. It defines the system’s runtime behavior and ensures resilience under adversarial conditions.

### 3. Distribution Layer
Handles outbound delivery using:

- Region‑specific routing profiles
- Multiple independent transport families
- Automatic fallback chains
- Low‑bandwidth and offline formats
- Opportunistic relays and multi‑hop paths

This layer is tightly integrated with the Emergency Channel and does not exist as a standalone module.

---

## Transport Architecture

The system uses multiple independent transport families to ensure resilience, indistinguishability, and regional adaptability.

The transport layer provides:

- Several protocol classes with distinct network signatures
- Fingerprint‑resistant behavior
- Region‑aware selection
- Automatic fallback under blocking
- Go‑native implementations for operational simplicity

The Emergency Channel dynamically selects transports based on region profiles, network conditions, and censorship pressure.

---

## Module Overview

Supporting modules live under `modules/` and integrate with the Emergency Channel through stable contracts.

### vpn-access-layer/
Provides covert entry points using multiple transport families.

Responsibilities include:

- Protocol obfuscation
- Entry‑point discovery
- Region‑aware access routing
- Multi‑protocol fallback

### news-aggregation/
Fetches and normalizes external news sources. Outputs sanitized, deduplicated, classified messages into the Emergency Channel.

### anonymous-bbs/
Provides pseudonymous posting with system‑generated identities. All posts undergo sanitization and metadata removal before entering the Emergency Channel.

---

## High‑Level Data Flow

Content enters the system through one of two ingestion paths:

### News Aggregation Path
Source registry → Fetcher → Parser → Deduplication → Classification  
→ **Emergency Channel** → Region‑aware distribution

### Pseudonymous BBS Path
Account login → Pseudonym generation → Sanitization → Storage  
→ **Emergency Channel** → Multi‑protocol delivery

Both paths converge in the Emergency Channel, which handles all routing, storage, and outbound delivery.

---

## Design Principles

The system is built on the following principles:

### Resilience Under Censorship
Multiple transport families, fallback chains, and region‑aware routing ensure continuity even under active blocking.

### Strict Separation of Concerns
Ingestion, core processing, and distribution are isolated to reduce attack surface and simplify reasoning.

### Metadata Minimization
All modules operate under a “minimum viable metadata” rule. The Emergency Channel enforces sanitization boundaries.

### Modularity and Extensibility
Supporting modules evolve independently without affecting the core system.

### Stealth and Indistinguishability
Traffic patterns mimic legitimate usage to evade detection and fingerprinting.

### Operational Simplicity
Deployable in constrained environments with minimal configuration and low operational overhead.

---

## Summary

The Emergency Publishing System is centered around the **Emergency Channel**, which provides secure, censorship‑resistant processing and distribution of critical information. Ingestion modules feed normalized content into the core, while the transport layer ensures resilient, region‑aware delivery across diverse network conditions.

This overview establishes the conceptual foundation for understanding the system’s architecture and runtime behavior.
