
# IPFS Storage

IPFS (InterPlanetary File System) provides peer‑to‑peer distributed storage using content addressing.  
It enables decentralized replication and censorship‑resistant availability.

---

## Characteristics
- Content identified by cryptographic hash  
- Automatic replication and deduplication  
- Resilient against single‑point failures  
- Supports censorship‑resistant distribution  

---

## Integration
- Works with **redundancy.md** to maintain replicas  
- Router selects IPFS when distributed resilience is required  
- Health checks monitor node availability and synchronization status
