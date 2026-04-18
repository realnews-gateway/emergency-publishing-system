
# Source Registry and Region-Aware Source Sets

The source registry defines all news sources used by the aggregation pipeline.  
It includes RSS feeds, APIs, curated lists, mirrors, and fallback endpoints.  
The registry is region-aware, allowing the system to adapt to censorship conditions and source availability.

This document describes how sources are organized, selected, rotated, and scored.

---

## Objectives

The source registry provides:

- A unified list of global and regional news sources  
- Multiple mirrors for each source to ensure redundancy  
- Region-aware source sets for censorship resistance  
- Reliability scoring and automatic source rotation  
- Integration with the fetcher and fallback transports  

These capabilities ensure continuous content flow even under partial source failure.

---

## Source Categories

Sources are grouped into several categories:

### 1. Global News Sources
- International news agencies  
- Major global newspapers  
- High-reliability RSS feeds  
- API-based content providers  

### 2. Regional News Sources
- Local newspapers and broadcasters  
- Region-specific RSS feeds  
- Community-curated lists  
- Local-language sources  

### 3. Mirror Sources
- CDN-backed mirrors  
- Proxy-based mirrors  
- Community-maintained mirrors  
- Archive-based fallback endpoints  

### 4. High-Risk Region Sources
- Sources frequently blocked or throttled  
- Sources requiring fallback transports  
- Sources with unstable availability  

---

## Region-Aware Source Sets

The system maintains separate source sets for different regions:

### Example Regions
- Global (default)  
- East Asia  
- Middle East  
- Europe  
- North America  
- Africa  
- High-censorship regions (CN, IR, RU)  

Each region set defines:

- Primary sources  
- Secondary sources  
- Mirrors  
- Fallback-only sources  
- Disabled sources (blocked or unstable)  

The fetcher automatically selects the appropriate set based on **region-config.md**.

---

## Source Metadata

Each source entry includes:

```
id: Unique identifier
url: Primary RSS/API endpoint
mirrors: List of fallback URLs
region: Applicable regions
language: Primary language
category: News category (general, politics, tech, etc.)
reliability: Initial reliability score (0–1)
update_interval: Expected update frequency
requires_proxy: Whether fallback transports are required
```

Example:

```
- id: "global_reuters"
  url: "https://www.reuters.com/rssFeed"
  mirrors:
    - "https://cdn.reuters.com/rssFeed"
    - "https://mirror.example.org/reuters"
  region: ["global", "high-censorship"]
  language: "en"
  category: "general"
  reliability: 0.95
  update_interval: 5m
  requires_proxy: false
```

---

## Reliability Scoring

Each source is continuously evaluated based on:

### 1. Availability
- Success rate of fetch attempts  
- Response time  
- Mirror performance  

### 2. Content Quality
- Completeness of metadata  
- Parsing success rate  
- Duplicate ratio  

### 3. Stability
- Frequency of outages  
- Consistency of update intervals  

Scores are updated dynamically and used to:

- Prioritize high-reliability sources  
- Rotate out failing sources  
- Promote stable mirrors  

---

## Source Rotation

When a source becomes unavailable:

- The system retries with fallback transports  
- If still failing, it switches to a mirror  
- If all mirrors fail, the source is temporarily disabled  
- Disabled sources are periodically rechecked  

This ensures uninterrupted content flow.

---

## Integration with Other Modules

The source registry integrates with:

- **fetcher.md** — Provides URLs, mirrors, and region-aware source sets  
- **parser.md** — Supplies metadata for parsing and normalization  
- **deduplication.md** — Helps identify duplicates across sources  
- **classification.md** — Provides category and language metadata  
- **region-config.md** — Defines region-specific blocking rules by referencing source IDs from this registry, ensuring no duplication of source attributes  

---

## Summary

The source registry defines all news sources used by the aggregation pipeline, including global feeds, regional sources, and mirrors.  
By maintaining region-aware source sets, reliability scoring, and automatic rotation, and integrating **region-config.md** (which references source IDs instead of duplicating attributes), the system ensures continuous and censorship-resistant content ingestion.
