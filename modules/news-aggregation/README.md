
# News Aggregation Module

The news aggregation module is responsible for collecting, normalizing, deduplicating, and classifying news content from diverse sources.  
It serves as the primary ingestion layer for the Empus publishing pipeline, feeding structured and verified content into the emergency channel, publisher, and distribution systems.

This module ensures that the system can continuously gather reliable information even under censorship, network instability, or partial source failure.

---

## Objectives

The news aggregation module provides:

- Multi-source content ingestion (RSS, APIs, scrapers, mirrors)  
- Content normalization and metadata extraction  
- Deduplication across heterogeneous sources  
- Topic classification and clustering  
- Source reliability scoring  
- Region-aware source selection  
- Integration with emergency publishing workflows  

---

## Architecture Overview

The aggregation pipeline consists of four major stages:

### 1. Source Discovery
- Maintains a registry of news sources (RSS feeds, APIs, mirrors, curated lists)  
- Supports region-specific source sets  
- Automatically rotates or replaces failing sources  

### 2. Content Fetching
- Fetches content using HTTP(S), CDN-backed endpoints, or proxy layers  
- Supports retries and fallback transports (TLS, QUIC, HTTP/2)  
- Handles rate limits, retries, and network failures  

### 3. Normalization and Parsing
- Extracts titles, timestamps, authors, tags, and body text  
- Converts HTML to clean structured text  
- Removes tracking parameters and noise  
- Normalizes timestamps across time zones  

### 4. Deduplication and Classification
- Identifies duplicate or near-duplicate articles  
- Clusters related content  
- Assigns topic labels (politics, economy, world, tech, etc.)  
- Scores source reliability and content quality  

---

## Directory Structure

```
news-aggregation/
├── README.md          # Overview and architecture
├── classification.md  # Topic classification and source scoring
├── config.md          # Configuration for fetch intervals, deduplication, classification rules
├── deduplication.md   # Duplicate detection and clustering
├── fetcher.md         # Fetching logic, retries, mirror rotation
├── parser.md          # HTML parsing, metadata extraction, normalization
├── region-config.md   # Mandatory file defining country-specific blocked foreign sources
└── sources.md         # Source registry and global source sets
```

---

## Integration with Other Modules

The news aggregation module integrates with:

- **ingest/** — Provides raw content for ingestion and transformation  
- **publisher/** — Supplies structured articles for publishing workflows  
- **emergency-channel/** — Feeds verified content into emergency broadcast pipelines  
- **network-access-layer/** — Ensures reliable client-server communication under censorship or network interference  
- **region-config.md** — Mandatory file defining which foreign sources are blocked in specific countries, enabling servers in free-network regions (e.g. Japan, Singapore) to fetch them  

---

## Design Principles

### Redundancy
Multiple sources for each topic ensure resilience against outages.  

### Normalization
All content is converted into a consistent internal format.  

### Reliability Scoring
Sources are continuously evaluated for accuracy and stability.  

### Censorship Resistance
Fetching logic integrates retries, mirror rotation, and proxy layers.  

### Minimal Metadata Leakage
Requests avoid identifiable patterns or unnecessary headers.  

---

## Summary

The news aggregation module provides a robust, censorship-resistant pipeline for collecting and structuring news content from diverse sources.  
By combining multi-source ingestion, normalization, deduplication, classification, and region-aware configuration, it ensures a continuous flow of high-quality information into the Empus publishing ecosystem.
