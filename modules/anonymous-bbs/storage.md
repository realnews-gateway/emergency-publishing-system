
# Storage Layer (Anonymous BBS)

The storage layer ensures durable, censorship‑resistant persistence of pseudonymous messages.  
It uses append‑only logs and region‑aware replication, while enforcing **mandatory removal of PII** before any data is stored.

---

## Objectives

- Immutable append‑only logs  
- Region‑aware replication (full, partial, delayed, multi‑path)  
- Integrity protection with hash chains and signatures  
- Efficient retrieval for distribution  
- **No storage of real identity, network metadata, or PII**  

---

## Storage Model

```
log:
  - message_001
  - message_002
  - ...
```

- **Immutable**: entries cannot be modified or deleted  
- **Ordered**: messages stored in gateway acceptance order  
- **Compact**: only public message fields, PII removed  

---

## Message Entry Format

```
log_entry:
  id: Public message ID
  pseudonym:
    name: Generated display name
    avatar: Generated avatar
  body: Cleaned text (PII removed)
  timestamp: Coarse-grained or jittered
  thread_id: Topic identifier
  reply_to: Optional parent ID
```

---

## Replication

- **Full**: all messages, low‑risk regions  
- **Partial**: selected threads, medium‑risk  
- **Delayed**: local first, replicate later, high‑risk  
- **Multi‑path**: multiple transports to resist blocking  

---

## Integrity

- Hash chains for tamper evidence  
- Optional Merkle trees for verification  
- Node signatures for authenticity  

---

## Retrieval

- Thread‑based queries  
- Time windows  
- Pagination  
- No full‑text search, no user queries  

---

## Integration

- **gateway.md** → supplies normalized, PII‑free messages  
- **distribution.md** → retrieves for delivery  
- **network‑access‑layer/** → censorship‑resistant transport  
- **region‑config/** → replication policies  

---

## Summary

The anonymous BBS storage layer provides durable, censorship‑resistant persistence of pseudonymous messages.  
By enforcing append‑only logs, region‑aware replication, and **mandatory PII removal**, it guarantees privacy, integrity, and availability even under hostile conditions.
