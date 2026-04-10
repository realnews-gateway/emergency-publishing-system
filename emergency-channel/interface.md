# Emergency Channel — Internal Interface Specification

This document defines the internal interfaces used within the **Emergency Channel**.  
These interfaces describe how ingestion, sanitization, routing, storage, and distribution subsystems interact.  
They are not public APIs and are not exposed to clients or external partners.

The goal of this specification is to ensure consistent behavior across all modules while maintaining strict separation between ingestion, processing, storage, and distribution.

---

## 1. Ingestion Interfaces

### ingest.submit(content)
Accepts sanitized or pre‑sanitized content from upstream modules.

Input:
- content.raw — raw or partially sanitized content  
- content.source — `news | bbs`  
- content.region — region tag (coarse‑grained)  

Output:
- content.normalized — normalized internal message structure  

### ingest.validate(content)
Ensures content meets internal safety and formatting requirements.

Output:
- `ok | reject(reason)`  

---

## 2. Sanitization Interfaces

### sanitizer.strip(content)
Removes all client‑side metadata.

Removes:
- EXIF  
- GPS  
- device identifiers  
- timestamps  
- embedded objects  

### sanitizer.normalize(content)
Converts content into the internal canonical format.

Operations:
- text cleanup  
- image re‑encoding  
- video transcoding  
- document flattening  

Output:
- content.cleaned  

---

## 3. Chunking & Redundancy Interfaces

### core.chunk(content)
Splits content into small, independent chunks.

Output:
- chunk.list — array of chunks  
- chunk.map — mapping for reconstruction  

### core.redundancy(chunks)
Applies redundancy for unstable or high‑risk networks.

Output:
- chunk.redundant  

---

## 4. Routing Interfaces

### router.score(transports, region)
Scores available transports based on:
- censorship intensity  
- latency  
- packet loss  
- recent failures  
- region‑specific rules  

Output:
- ranked list of transports  

### router.select(ranked_transports)
Selects the optimal transport for this session.

### router.fallback(previous_transport)
Returns the next viable transport when a failure occurs.

---

## 5. Transport Interfaces

These interfaces abstract the Empus transport stack:

- VLESS  
- REALITY  
- uTLS  
- XTLS‑Vision  
- XHTTP (Stream / Packet)  
- TUIC v5  

### transport.send(chunk, context)
Sends a chunk using the selected transport.

### transport.receive(context)
Receives inbound data (used for synchronization and distribution).

### transport.probe(network_state)
Evaluates transport viability.

---

## 6. Storage Interfaces

### storage.store(chunk)
Stores encrypted chunks in regional or DTN storage.

### storage.retrieve(id)
Retrieves stored chunks for reconstruction.

### storage.retain(policy)
Applies region‑aware retention policies.

---

## 7. Distribution Interfaces

### distributor.dispatch(chunks, region)
Delivers chunks using region‑appropriate transports.

### distributor.sync()
Performs opportunistic synchronization across regions.

### distributor.fallback()
Switches to covert or offline delivery modes when required.

---

## 8. Publisher Interfaces

### publisher.package(content)
Prepares sanitized, normalized content for downstream consumers.

### publisher.deliver(content, context)
Delivers content to:
- clients  
- mirrors  
- downstream systems  

### publisher.notify(event)
Signals availability of new content.

---

## Summary

This interface specification defines how the Emergency Channel’s internal modules interact.  
It provides a unified abstraction for ingestion, sanitization, chunking, routing, storage, and distribution—without exposing any public APIs.

These interfaces ensure that Empus behaves consistently across regions, transports, and deployment models, while maintaining strict metadata minimization and censorship‑resistant operation.
