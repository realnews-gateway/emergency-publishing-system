# Emergency Channel — Storage Module

## Overview
The Storage module for the **Emergency Channel** defines how sanitized content is persisted for this channel specifically.  
This directory contains channel‑specific implementation notes, examples, and operational constraints. For global storage policies (retention, encryption, backups, cost models, and governance), see the repository root **/storage/README.md**.

---

## Supported Backends
- **Arweave** — permanent decentralized storage; one‑time fees and permanence tradeoffs. See `arweave.md` for channel integration notes.  
- **IPFS** — content‑addressable storage; requires pinning and gateway considerations. See `ipfs.md`.  
- **Local** — local filesystem or ephemeral object store for constrained/offline environments and testing. See `local.md`.

Each backend has different tradeoffs in durability, availability, cost, and decentralization; choose per deployment context.

---

## Channel Guarantees and Constraints
- **Signature verification**: All content must be signed and verified before persistence.  
- **Immutable storage**: Treat persisted content as immutable; do not modify stored payloads or canonical content addresses.  
- **Minimal retention in channel**: Keep only what is necessary for channel operation; rely on global archival policies for long‑term retention.  
- **No duplicate policy text**: This directory is intentionally lightweight — authoritative policies live at `/storage/README.md`.

---

## Operational Notes (Channel Specific)
- **Cost awareness**: For Arweave writes, estimate one‑time fees before bulk publishing.  
- **Pinning policy**: For IPFS, use a managed pinset or internal pinning service; document pin TTL and re‑pin procedures.  
- **Local usage**: Local storage is for development and constrained deployments only; do not use for production data.  
- **Forensics**: Preserve channel‑specific request IDs and ephemeral correlation tokens for debugging, but avoid storing raw submission content in logs.

---

## Files in this Directory
- `arweave.md` — channel integration details and operational reminders.  
- `ipfs.md` — pinning, gateway, and availability notes.  
- `local.md` — local storage layout, test data handling, and cleanup guidance.  
- `README.md` — this file (channel overview and pointers).

---

## Summary
This directory contains concise, Emergency Channel–specific storage guidance and operational notes. It is intentionally lightweight and implementation‑focused; global storage policies (retention, encryption, backups, cost models, and detailed operational playbooks) are maintained at the repository root `/storage/README.md`. Use the channel files for quick integration details, deployment constraints, and short operational reminders; avoid duplicating global policy text here.

---
