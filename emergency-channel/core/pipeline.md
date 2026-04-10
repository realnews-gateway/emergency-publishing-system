# Emergency Channel — Pipeline

The Emergency Channel Pipeline defines the internal processing sequence
for sanitized, identity‑free content. It transforms normalized input into
a censorship‑resistant, redundantly encoded, globally deliverable format.

The pipeline does not handle user accounts, submission interfaces, or
identity‑linked metadata. All content entering the pipeline has already
been sanitized upstream.

---

## 1. Pipeline Overview

The pipeline consists of five deterministic stages:

1. Sanitized Input  
2. Chunking  
3. Redundancy Encoding  
4. Routing Preparation  
5. Storage & Distribution Coordination  

Each stage enforces strict consistency, integrity, and anonymity
guarantees.

---

## 2. Pipeline Stages

### 2.1 Sanitized Input

The pipeline begins only after upstream sanitization is complete.

Input guarantees:

- No metadata (EXIF, XMP, IPTC, PDF, headers)
- No device, location, or software identifiers
- No user identifiers or account linkage
- Normalized, safe binary format

The Core never receives raw or untrusted content.

---

### 2.2 Chunking

Content is split into deterministic, transport‑friendly chunks.

Properties:

- Fixed or adaptive chunk size depending on transport conditions
- Deterministic ordering for reproducibility
- Chunk map generated for reconstruction
- Suitable for unstable, high‑loss networks

Chunking is the foundation for redundancy and multi‑path delivery.

---

### 2.3 Redundancy Encoding

Forward‑error‑correction and multi‑path redundancy are applied.

Capabilities:

- Reed‑Solomon or fountain‑code style redundancy
- Reconstruction possible even with partial chunk loss
- Region‑aware redundancy levels
- DTN‑compatible bundle generation

This stage ensures survivability under censorship and packet loss.

---

### 2.4 Routing Preparation

Routing metadata is generated based on:

- Transport viability (REALITY, uTLS, XTLS‑Vision, XHTTP Stream/Packet,
  VLESS, TUIC v5)
- Region‑specific fallback chains
- Network degradation signals from Monitoring
- Multi‑path delivery opportunities

Output includes:

- Primary routing hints
- Fallback routing hints
- Transport scoring
- Region‑aware delivery strategies

No user identity or submission metadata is included.

---

### 2.5 Storage & Distribution Coordination

The final stage prepares content for persistence and delivery.

Responsibilities:

- Store chunk bundles in regional storage backends
- Generate DTN bundles for intermittent networks
- Coordinate multi‑transport distribution
- Ensure integrity verification before replication
- Maintain redundant copies across regions

This stage ensures long‑term availability and global accessibility.

---

## 3. Data Contracts

The pipeline uses strict, versioned data contracts.

### 3.1 SanitizedContent

{
  "normalized_blob": <binary>,
  "content_size": <int>,
  "sanitization_report": { ... }
}

### 3.2 ChunkBundle

{
  "chunks": [ ... ],
  "chunk_map": { ... },
  "bundle_id": "string"
}

### 3.3 RedundantBundle

{
  "redundant_chunks": [ ... ],
  "redundancy_level": <int>,
  "reconstruction_rules": { ... }
}

### 3.4 RoutingHints

{
  "primary_routes": [ ... ],
  "fallback_routes": [ ... ],
  "transport_scores": { ... }
}

### 3.5 PersistedRecord

{
  "storage_regions": [ ... ],
  "dtn_bundles": [ ... ],
  "integrity_hash": "string"
}

All contracts exclude identity, account data, or submission metadata.

---

## 4. Error Handling & Recovery

### Chunking Errors
- Fallback to smaller chunk size
- Regenerate chunk map
- Reject only if input is structurally invalid

### Redundancy Errors
- Reduce redundancy level
- Retry encoding
- Switch to minimal redundancy mode

### Routing Errors
- Switch to fallback routes
- Reduce routing complexity
- Queue for delayed delivery

### Storage Errors
- Retry with exponential backoff
- Switch to alternate region
- Mark as pending replication

### Distribution Errors
- Retry on next cycle
- Defer until connectivity improves
- Use DTN‑only mode

---

## 5. Security Notes

- No identity linkage at any stage
- No account identifiers or submission metadata
- All content processed in sanitized form
- Deterministic transformations for auditability
- Integrity verified at every stage
- Logs contain no content or routing metadata

The pipeline is designed to operate safely under active surveillance and
network interference.

---

## 6. Key Principles

- Deterministic, reproducible processing
- Region‑aware routing and fallback
- Multi‑path redundancy for survivability
- Strict separation from user identity and accounts
- Storage and distribution optimized for hostile environments

The Emergency Channel Pipeline is the backbone of censorship‑resistant
content delivery.
