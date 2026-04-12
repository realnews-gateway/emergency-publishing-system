# Publisher Module — Delivery Adapters

## Overview

Delivery adapters are the **execution layer** of the Publisher module.  
Channels define *where* content should be delivered, adapters define *how* delivery is executed.  
Each adapter implements a unified interface, allowing the Publisher pipeline to transmit content through diverse mechanisms without being tied to any single protocol.

Adapters are **modular, isolated, and replaceable**.  
Deployments may enable, disable, or override adapters based on operational needs.

---

## Adapter Responsibilities

Each delivery adapter is responsible for:

- **Payload transmission**  
  Sending formatted content to external endpoints.

- **Protocol handling**  
  Managing HTTP, email, IPFS, P2P, or other transports.

- **Error reporting**  
  Returning structured errors to the fallback subsystem.

- **Health signaling**  
  Providing status information for monitoring.

- **Security enforcement**  
  Enforcing TLS, signing, or encryption requirements.

Adapters do *not* modify content; they only deliver it.

---

## Standard Adapter Types

### 1. HTTP Adapter
**Use cases:** HTML publishing, JSON APIs, RSS feeds  
**Features:** TLS enforcement, redirect handling, retries, GET/POST/PUT support

### 2. Email Adapter
**Use cases:** Digest emails, alert notifications  
**Features:** SMTP/API delivery, optional signing/encryption, batch or single‑message modes

### 3. IPFS Adapter
**Use cases:** Decentralized publishing, mirror‑resistant replication  
**Features:** Pinning, gateway fallback, optional local node integration

### 4. P2P Adapter
**Use cases:** Opportunistic delivery, unstable networks  
**Features:** Peer discovery, store‑and‑forward, delay‑tolerant operation

### 5. Offline Bundle Adapter
**Use cases:** Air‑gapped environments, high‑risk distribution  
**Features:** Portable bundles (USB, SD, QR), no network dependency

### 6. Micro‑Feed Adapter
**Use cases:** Extreme censorship, minimal text‑only feeds  
**Features:** Very small payloads, resilient to filtering, fallback micro‑channels

---

## Adapter Interface

All adapters implement a common interface:

- `prepare(payload)` → validate and prepare  
- `deliver(payload)` → send to target endpoint  
- `status()` → return health information  
- `capabilities()` → declare supported features (retries, encryption, etc.)

This interface ensures new adapters can be added without modifying the Publisher pipeline.

---

## Extending the Adapter System

To add a new adapter, deployments must provide:

1. Delivery implementation  
2. Configuration schema  
3. Optional health‑check logic  
4. Optional fallback rules  

Adapters can be registered dynamically, enabling evolution over time.

---

## Summary

The adapter subsystem provides:

- A unified interface for all delivery mechanisms  
- Modular, replaceable components  
- Strong separation between formatting and delivery  
- Built‑in support for decentralized and offline channels  
- Extensibility for future protocols and environments  

Adapters are the **execution backbone** of the Publisher module.
