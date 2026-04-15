# Emergency Channel — IPFS Storage

## Purpose
IPFS (InterPlanetary File System) is used by the Emergency Channel as a distributed, content‑addressed storage tier that complements local and archival backends.  
This file contains channel‑specific integration notes, operational constraints, and quick runbook items. For global storage policies (retention, encryption, backups, cost models, and governance), see the repository root `/storage/README.md`.

---

## Responsibilities

### Distributed Persistence
- Ensure critical content is persisted across multiple IPFS nodes for redundancy.  
- Track pin status and replication for channel‑published CIDs.

### Content Addressing
- Produce and return immutable CIDs for sanitized content.  
- Ensure identical content maps to the same CID and reject tampered payloads.

### Availability and Access
- Provide retrieval via local nodes, private gateways, or trusted public gateways.  
- Support fallback retrieval when local or cloud storage is unavailable.

### Pipeline Integration
- Accept content from the channel pipeline and return CIDs to Distributor and Analytics.  
- Expose verification hooks for downstream retrieval and integrity checks.

---

## Architecture

### Node Interaction
- Prefer local IPFS daemons for performance and control.  
- Use clustered or managed pinning services for redundancy.  
- Allow optional private IPFS networks for sensitive deployments.

### Pinning and Replication
- Automatically pin newly published CIDs to a managed pinset.  
- Replicate pins to multiple nodes asynchronously; track replication state in metadata.  
- Treat unpinned content as ephemeral and subject to GC.

### Gateway Strategy
- Use private or internal gateways for production reads; public gateways only for emergency fallback.  
- Select gateways by latency and reliability; record gateway health metrics.

---

## Failure Handling

### Node Failure
- Failover to alternate local or clustered nodes; mark degraded nodes and requeue replication.  
- Retry operations with exponential backoff; do not block the publishing pipeline.

### Gateway Failure
- Switch to alternative gateways; prefer internal gateways when available.  
- Log gateway errors and surface metrics to monitoring for escalation.

### Replication Failure
- Retain content in local storage until replication succeeds.  
- Retry replication in background; escalate to oncall if replication remains incomplete beyond threshold.

---

## Security and Integrity

### Hash Verification
- Verify CID content hash before publishing; reject mismatches.  
- Recompute and validate hashes during retrieval and replication checks.

### Access Control
- For sensitive content, prefer private IPFS networks or encrypt payloads before adding to IPFS.  
- Gate production reads through authenticated gateways or proxy layers.

### Anti-Tampering
- Rely on immutable CIDs for integrity; include additional metadata signatures where needed for provenance.

---

## Operational Notes (Channel Specific)
- **Pinning policy**: Use a managed pinning service or internal cluster; document pin TTL and re‑pin procedures.  
- **Gateway usage**: Avoid relying on public gateways for routine reads; cache frequently accessed content.  
- **Monitoring**: Track pin counts, replication lag, gateway latency, and node health.  
- **Cost and capacity**: Account for pinning service costs and node storage capacity when publishing at scale.  
- **Local fallback**: Keep a short‑term local cache for recent publishes to speed reads and reduce gateway dependence.

---

## Summary
This document provides concise, Emergency Channel–specific guidance for using IPFS as a distributed storage tier. It focuses on integration points, pinning and replication behavior, failure handling, and lightweight operational guidance. For detailed global policies and operational playbooks, refer to `/storage/README.md`.
