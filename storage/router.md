
# Storage Router

The storage router defines how content is directed to the appropriate backend for persistence.  
It ensures that routing decisions are deterministic, integrity‑aware, and fault‑tolerant, balancing performance and resilience across local, distributed, and permanent storage systems.

---

## Objectives

- Provide deterministic routing logic for content persistence  
- Balance availability, latency, and redundancy across backends  
- Enforce integrity verification before routing decisions  
- Enable fallback routing under failures or censorship conditions  

---

## Routing Logic

### 1. Integrity‑First Routing
- Content must pass signature and hash validation before routing  
- Unverified content is rejected and logged  

### 2. Backend Selection Criteria
- **Local** → Fast persistence for testing and small‑scale deployments  
- **IPFS** → Distributed replication for censorship‑resistant availability  
- **Arweave** → Permanent archiving for immutable, long‑term storage  

### 3. Decision Factors
- **Availability** — Backend health and uptime status  
- **Latency** — Measured response time from health checks  
- **Redundancy** — Replica count and geographic distribution  
- **Priority** — Content classification (critical vs. non‑critical)  

### 4. Fallback Routing
- Automatic rerouting to healthy backends when primary fails  
- Degraded mode: minimal payload persisted locally until distributed storage recovers  

---

## Routing Workflow

1. **Input Validation** → Verify content integrity  
2. **Backend Health Check** → Query availability and latency metrics  
3. **Decision Engine** → Select backend based on criteria  
4. **Persistence** → Store content in chosen backend(s)  
5. **Replication Update** → Record replica status in schema  
6. **Audit Logging** → Log routing decision and outcome  

---

## Integration

- **overview.md** — Describes storage models available for routing  
- **schema.md** — Provides metadata and replication records used in routing decisions  
- **redundancy.md** — Ensures replicas are maintained across selected backends  
- **health-checks.md** — Supplies backend health metrics for routing logic  
- **backends/** — Contains backend‑specific documentation (local, IPFS, Arweave)  

---

## Summary

The storage router enforces integrity‑first routing and deterministic backend selection.  
By balancing availability, latency, redundancy, and priority, it ensures reliable persistence across local, distributed, and permanent storage systems, with automatic fallback under adverse conditions.
