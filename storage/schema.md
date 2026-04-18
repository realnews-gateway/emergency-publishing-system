
# Storage Schema

The storage schema defines the data structures, metadata, and versioning rules that govern how content is persisted across backends.  
It ensures consistency, auditability, and compatibility between local, distributed, and permanent storage systems.

---

## Objectives

- Provide a unified schema for all storage backends  
- Ensure deterministic defaults and safe‑by‑default settings  
- Support explicit validation and versioned changes  
- Preserve copyright and author attribution in metadata  
- Enable replication tracking and auditability  

---

## Core Data Structures

### 1. Content Object
```
{
  "id": "unique identifier",
  "hash": "sha256 hash of content",
  "signature": "publisher signature",
  "payload": "normalized content body",
  "metadata": { ... }
}
```

### 2. Metadata
- **source** — Original publisher or feed  
- **timestamp** — UTC time of ingestion  
- **classification** — Tags or categories assigned by modules  
- **author** — Original author attribution  
- **version** — Schema version for compatibility  

### 3. Replication Record
```
{
  "object_id": "content id",
  "backends": ["local", "ipfs", "arweave"],
  "replica_count": 3,
  "last_synced": "ISO8601 timestamp"
}
```

---

## Validation Rules

- **Deterministic Defaults** — All fields must have explicit defaults  
- **Safe‑by‑Default** — No implicit trust; integrity must be verified  
- **Explicit Validation** — Schema validation enforced before persistence  
- **Versioned Changes** — Backward compatibility maintained via schema versioning  

---

## Integration

- **overview.md** — Describes storage models that use this schema  
- **redundancy.md** — Tracks replication and sharding using schema records  
- **router.md** — Uses schema metadata to select optimal backend  
- **health-checks.md** — Validates schema compliance during monitoring  

---

## Summary

The storage schema provides a unified, validated structure for persisting content across backends.  
By enforcing deterministic defaults, explicit validation, and versioned changes, it ensures consistency, auditability, and long‑term reliability while preserving copyright and author attribution.
