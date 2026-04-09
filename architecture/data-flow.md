# Data Flow

This document describes the end‑to‑end data flow of **Empus**.  
It explains how content enters the system, how it is transformed by the Emergency Channel, how it is stored across distributed nodes, and how it is ultimately delivered under diverse and adversarial network conditions.

---

## Overview

All content—whether aggregated news or pseudonymous submissions—passes through a unified processing pipeline.  
The **Emergency Channel** orchestrates this pipeline, ensuring that content remains safe, normalized, redundant, and deliverable even under severe censorship pressure.

The system is language‑agnostic; multi‑language handling is implemented in ingestion modules.

---

## 1. Ingestion

Content enters Empus through two independent ingestion paths:

### News Aggregation Path
- Fetches content from external news sources  
- Normalizes encoding and structure  
- Removes tracking elements  
- Extracts text, metadata, and media references  
- Produces a sanitized, standardized content object  

### Anonymous BBS Path
- Accepts user‑submitted text  
- Strips metadata and identifiers  
- Applies safety and content rules  
- Normalizes formatting  
- Produces a pseudonymous content object  

Both ingestion paths output a **Unified Content Envelope**, which is passed to the Emergency Channel.

---

## 2. Emergency Channel Processing

The Emergency Channel is the core processing layer of Empus.  
It transforms the Unified Content Envelope into a form suitable for global, censorship‑resistant distribution.

Key processing steps include:

- **Sanitization**  
  Removes unsafe elements and normalizes structure.

- **Metadata Minimization**  
  Retains only essential metadata (timestamp, region tags, language tag).

- **Chunking**  
  Splits content into small, transport‑friendly segments.

- **Redundancy Encoding**  
  Adds forward‑error‑correction and multi‑path redundancy.

- **Region‑Aware Routing Preparation**  
  Assigns routing hints based on risk level and network conditions.

- **Storage & Distribution Coordination**  
  Determines which storage nodes and distribution paths will receive the content.

The output of this stage is a **Processed Content Bundle**.

---

## 3. Storage Layer

The Processed Content Bundle is stored using a combination of:

- **Regional Caching Nodes**  
  Fast access for low‑risk and medium‑risk regions.

- **Delay‑Tolerant Bundles**  
  Synchronization for unstable or intermittent networks.

- **Redundant Chunk Distribution**  
  Survivability even if nodes are lost or compromised.

- **Region‑Aware Retention Policies**  
  Storage duration adapts to local risk and legal constraints.

Storage is coordinated by the Emergency Channel but executed by distributed nodes.

---

## 4. Distribution Layer

The distribution layer delivers content across diverse environments:

- **Low‑risk regions**  
  High‑performance transports and full caching.

- **Medium‑risk regions**  
  Camouflaged transports and selective caching.

- **High‑risk regions**  
  Emergency transports, opportunistic sync, and minimal metadata.

- **Offline environments**  
  Bundle‑based distribution and peer‑to‑peer relay.

Clients reconstruct the original content by:

1. Fetching available chunks  
2. Applying redundancy correction  
3. Reassembling the content envelope  
4. Rendering the final content  

---

## Summary

Empus unifies ingestion, processing, storage, and distribution into a single resilient pipeline centered on the Emergency Channel.  
By minimizing metadata, applying redundancy, and coordinating region‑aware routing, Empus ensures that critical information remains accessible even under severe censorship and network degradation.
