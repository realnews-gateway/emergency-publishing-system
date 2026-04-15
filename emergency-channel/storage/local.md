# Emergency Channel — Local Storage

## Purpose
Local Storage provides a low‑latency, lightweight temporary storage tier for the Emergency Channel.  
This file contains channel‑specific implementation notes, operational constraints, and quick recovery guidance. For global storage policies (retention, encryption, backups, cost models, and governance), see the repository root /storage/README.md.

---

## Applicable Scenarios (channel scoped)
- Short‑term buffering: Accept sanitized content and hold it until asynchronous replication to distributed backends completes.  
- Low‑latency reads and writes: Serve pipeline components that require fast access for validation and processing.  
- Constrained or offline deployments: Provide temporary persistence when remote backends are unreachable.  
- Development and testing: Default storage for local CI and developer workflows.

---

## Directory Structure (recommended)
``` 
/local-storage/
  /content/
  /metadata/
  /cache/
  /tmp/
```
- content — committed local content files (short retention).  
- metadata — lightweight index (local ID, CID/TXID, state, timestamps, replication status).  
- cache — hot cache for frequently accessed items.  
- tmp — temporary files used for atomic writes and staging.

---

## Write and Read Paths (channel implementation notes)
- Write path: write to tmp → generate metadata → atomically move to content → return local reference or ID to upstream.  
- Asynchronous replication: background workers replicate content to IPFS or Arweave and update metadata with replication state.  
- Read path: prefer cache → content → fallback to distributed backends if local copy is missing per policy.

---

## Cleanup and Retention (simplified for channel)
- Short‑term retention: default retention window (for example, 7 days) or until successful replication to distributed backends.  
- Capacity thresholds: trigger cleanup when disk usage exceeds configured thresholds (evict cache first, then oldest content).  
- Retention exceptions: extend retention only for items required for immediate debugging or pending replication; long‑term archival is handled by global policies.

---

## Failure Modes and Recovery (quick runbook)
- Write failures: retry writes to tmp; on repeated failure, record failure metadata and surface to monitoring; do not block main pipeline.  
- Replication failures: mark items as pending in metadata; background retries with exponential backoff; escalate to oncall if unresolved past threshold.  
- Disk pressure: enter emergency cleanup mode — throttle new writes, purge non‑essential cache, remove oldest content, and notify operators.  
- Consistency: use atomic rename semantics to avoid exposing partial writes.

---

## Security and Compliance (channel notes)
- Avoid storing raw sensitive content in logs or metadata; prefer references and hashes.  
- Access control: restrict local storage access to pipeline services and authorized operators.  
- Encryption: if running in untrusted environments, enable disk or directory encryption per global policy.

---

## Monitoring and Alerts (channel checklist)
- Monitor disk usage, write error rate, replication lag, and cleanup frequency.  
- Alert thresholds: disk usage at 80% and 90%; replication lag beyond configured SLA; sudden spike in write errors.  
- Retain recent local access logs (for example, 72 hours) for troubleshooting, ensuring logs are sanitized.

---

## Summary
Local Storage in the Emergency Channel is a short‑lived, high‑performance buffer and cache layer. Keep the implementation lightweight, resilient, and tightly integrated with asynchronous replication to distributed backends. Defer long‑term retention, encryption policy, and cost considerations to the global /storage/README.md.
```
