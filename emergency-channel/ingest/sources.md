# Ingest Module — Sources

## Overview

The Ingest module accepts raw, untrusted content from multiple external
sources. All sources are treated equally with no trust assumptions, no
special handling, and no partner-specific logic. This document defines
the supported ingestion sources and their characteristics.

---

## Source Categories

The system supports the following categories of ingestion sources:

- **User Submissions**  
  Direct submissions from individuals through web forms, apps, or
  anonymous channels.

- **Automated Crawlers**  
  Scheduled or event-driven crawlers that fetch content from predefined
  locations.

- **Mirrored Content Streams**  
  External content streams replicated from decentralized or
  censorship-resistant ecosystems.

- **Opportunistic or Fallback Sources**  
  Low-bandwidth or intermittent channels used during network disruption.

All sources are treated as untrusted and must pass validation and
normalization before entering the pipeline.

---

## 1. User Submissions

User submissions are the most diverse and unpredictable source type.

Characteristics:

- Highly unstructured  
- Potentially malicious  
- Requires strict validation  
- May include mixed formats (text, images, attachments)  

User submissions always operate at the lowest trust level.

---

## 2. Automated Crawlers

Crawlers fetch content from predefined URLs, feeds, or APIs.

Characteristics:

- Predictable structure  
- High volume  
- Requires rate limiting  
- Must handle network failures gracefully  

Crawler output is still considered untrusted until validated.

---

## 3. Mirrored Content Streams

Mirrored streams replicate content from external ecosystems such as:

- Decentralized networks  
- Public archives  
- Community-maintained mirrors  

Characteristics:

- May include duplicate or outdated content  
- Structure varies by source  
- Requires normalization  
- Trust level is always low  

Mirrored streams improve resilience and coverage but remain untrusted.

---

## 4. Opportunistic or Fallback Sources

Opportunistic sources are used during extreme network disruption.

Examples:

- Low-bandwidth relays  
- Offline submissions  
- Delay-tolerant networks  
- Emergency fallback channels  

Characteristics:

- Highly variable structure  
- Low reliability  
- Prioritizes availability over format consistency  

These sources ensure the system remains operational under adverse
conditions.

---

## Trust Model

All ingestion sources share the same trust model:

- All content is untrusted  
- No source receives elevated trust  
- No partner-specific rules or trust boundaries  
- All content must pass validation and normalization  
- No identity, transport, or behavioral metadata is retained  

This uniform trust model simplifies ingestion and strengthens security.

---

## Summary

The Ingest module supports diverse content sources, including:

- User submissions  
- Automated crawlers  
- Mirrored content streams  
- Opportunistic fallback sources  

All sources are treated as untrusted and must pass validation and
normalization before entering the Emergency Channel pipeline.
