
# Storage Overview

The storage overview describes the models and strategies used to persist content across different backends.  
It explains local, distributed, and permanent storage approaches, redundancy mechanisms, and routing logic that ensure durability and resilience.

---

## Objectives

- Provide a clear description of supported storage models  
- Ensure redundancy and fault tolerance across nodes  
- Enable routing between multiple backends based on availability and latency  
- Support long‑term persistence and auditability  

---

## Storage Models

### 1. Local Storage
- Simple persistence on local nodes  
- Fast read/write operations  
- Suitable for testing, prototyping, and small‑scale deployments  

### 2. Distributed Storage (IPFS)
- Peer‑to‑peer content addressing  
- Decentralized replication across multiple nodes  
- Resilient against single‑point failures  
- Supports censorship‑resistant distribution  

### 3. Permanent Storage (Arweave)
- Immutable, permanent archiving of content  
- Guarantees long‑term availability and historical integrity  
- Ideal for environments requiring censorship resistance and verifiable history  

---

## Redundancy and Routing

- Replicas maintained across local, distributed, and permanent backends  
- Sharding strategies for scalability and load balancing  
- Router logic selects optimal backend based on health checks and latency  
- Cross‑node fault tolerance ensures continuity under failures  

---

## Integration

- **schema.md** — Defines data structures, metadata, and version control  
- **redundancy.md** — Describes replication and sharding strategies  
- **router.md** — Specifies backend selection logic  
- **health-checks.md** — Provides monitoring of availability and error rates  
- **backends/** — Contains detailed documentation for each backend (local, IPFS, Arweave)  

---

## Summary

The storage overview defines the models and strategies for persisting content reliably.  
By combining local, distributed, and permanent storage with redundancy and routing, the system ensures durability, scalability, and censorship‑resistant availability.
