§
# Modules Directory

The Modules directory contains all supporting runtime subsystems of Empus.  
Each module represents a distinct functional layer, designed to be independently testable, deployable, and extensible.

These modules provide ingestion, access, and content‑generation capabilities that feed into the core system: the **Emergency Channel**.

---

## Contents

This directory includes the following modules:

- **news-aggregation/**  
  Aggregates external news sources, normalizes content, removes duplicates, and classifies topics.  
  Includes source registry, region‑aware fetching, parsing, deduplication, and classification.

- **anonymous-bbs/**  
  Provides pseudonymous posting with account‑based access control.  
  Users authenticate with lightweight accounts, but all public identity is system‑generated (name + avatar).  
  Supports metadata stripping, moderation, content sanitization, and censorship‑resistant discussion.

(Additional modules may be added as the system evolves.)

---

## Design Principles

All modules follow these architectural principles:

- **Modularity**  
  Each module is self‑contained and independently testable.

- **Extensibility**  
  Modules can evolve without breaking system‑wide compatibility.

- **Separation of concerns**  
  Transport, aggregation, and user‑generated content are clearly separated.

- **Censorship resistance**  
  All modules are designed to operate under hostile network conditions.

- **Stealth and resilience**  
  Protocols and workflows mimic legitimate traffic and recover from failure.

---

## Integration

Modules interact with the core system via shared interfaces and pipelines:

- **Transport and fallback logic** are provided by `network-access-layer/`  
- **News ingestion and processing** are handled by `news-aggregation/`  
- **User‑generated content** is handled by `anonymous-bbs/`  
- **Emergency routing and distribution** are handled by the root‑level `emergency-channel/`  
- **Region‑aware behavior** is supported across all modules

---

## Summary

The Modules directory defines the supporting runtime architecture of Empus.  
Each submodule implements a critical capability—transport, aggregation, or pseudonymous communication—and together they feed content into the core **Emergency Channel**.
