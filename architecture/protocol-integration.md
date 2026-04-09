# Protocol Integration

This document explains how the Emergency Channel in **Empus** integrates with multiple network transports.  
The goal is to ensure message delivery under censorship, packet inspection, throttling, or regional blocking.  
The Emergency Channel selects, adapts, and switches transports dynamically based on network conditions.

Empus supports a flexible transport layer rather than a fixed set of protocols.  
Each transport is treated as a pluggable module with its own capabilities and constraints.

---

## Transport Integration Model

The Emergency Channel interacts with transports through a unified interface:

    send(message, context)
    receive(context)
    probe(network_state)
    fallback(previous_transport)

Each transport implements these operations according to its own characteristics.  
The Emergency Channel does not depend on protocol‑specific logic.

---

## Transport Categories

### 1. Standard Transports
Used when censorship pressure is low.

- HTTPS with domain fronting  
- CDN‑backed HTTPS  
- TLS over common ports  

Characteristics:
- High throughput  
- Low suspicion  
- Suitable for bulk content  

---

### 2. Obfuscated Transports
Used when DPI or active probing is present.

- Traffic mimicry (browser‑like patterns)  
- Randomized padding and timing  
- Encrypted payloads disguised as benign traffic  

Characteristics:
- Medium throughput  
- High stealth  
- Resistant to protocol fingerprinting  

---

### 3. Covert Transports
Used when standard and obfuscated transports are blocked.

- DNS tunneling  
- Fragmented payloads hidden in allowed traffic  
- Region‑specific covert channels  

Characteristics:
- Low throughput  
- High survivability  
- Effective under heavy censorship  

---

### 4. Offline & Delay‑Tolerant Transports
Used when connectivity is intermittent.

- Bundled message packages  
- Opportunistic synchronization  
- Region‑aware caching  

Characteristics:
- Very high resilience  
- Works in unstable networks  
- Suitable for emergency content  

---

## Transport Selection Logic

The Emergency Channel selects transports based on:

- Region  
- Censorship intensity  
- Network latency and packet loss  
- Recent failures  
- User access layer (e.g., vpn‑access‑layer)  

Selection process:

    1. Probe available transports
    2. Score each transport based on conditions
    3. Select highest‑scoring transport
    4. On failure, fallback to next option
    5. Persist successful patterns for future sessions

This ensures adaptive, context‑aware delivery.

---

## Fallback Chain

Fallback is essential for censorship resistance.  
A typical chain:

    HTTPS → CDN‑backed HTTPS → Obfuscated TLS → DNS tunneling → Covert regional transport → Offline sync

Fallback is triggered by:

- Connection failure  
- DPI resets  
- Suspicious latency spikes  
- Region‑specific blocking events  

Fallback decisions are stateless and reversible.

---

## Integration with Emergency Channel

The Emergency Channel uses transports for:

- Routing messages  
- Delivering content to users  
- Synchronizing storage across regions  
- Distributing emergency alerts  

The Channel does not assume transport reliability. Instead, it:

- Normalizes messages before sending  
- Splits content into small chunks  
- Adds redundancy for unstable networks  
- Applies region‑aware routing rules  

This allows Empus to operate even when transports are unreliable or heavily monitored.

---

## Summary

Empus integrates multiple transports through a unified interface.  
The Emergency Channel dynamically selects and switches transports based on censorship conditions, ensuring message delivery under hostile environments.  
Standard, obfuscated, covert, and offline transports work together to provide a resilient communication backbone.
