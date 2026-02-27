# Data Flow

This document describes the end‑to‑end data flow of the Emergency Publishing System. It explains how content enters the system, how it is normalized and sanitized, how it moves through the Emergency Channel, and how it is distributed under hostile network conditions.

The system has two ingestion paths:
1. News Aggregation Path
2. Pseudonymous BBS Path

Both paths converge in the Emergency Channel.

---

## High‑Level Flow Summary

    External Sources
        ↓
    news-aggregation module
        ↓
    Emergency Channel
        ↓
    Multi‑Protocol Distribution

    User Devices
        ↓
    anonymous-bbs module
        ↓
    Emergency Channel
        ↓
    Multi‑Protocol Distribution

---

# 1. News Aggregation Path

## 1.1 Source Registry
- Maintains a list of feeds and region‑specific sources
- Supports dynamic updates and failover
- Defines fetch intervals, parsing rules, and region profiles

## 1.2 Fetcher
- Retrieves raw content from registered sources
- Handles retries, timeouts, and fallback URLs
- Uses vpn-access-layer for region‑aware routing
- Ensures consistent behavior under partial blocking

## 1.3 Parser
- Converts HTML, JSON, or XML into structured internal objects
- Extracts title, body, timestamp, and media
- Removes tracking parameters and fingerprintable elements
- Normalizes encoding and text structure

## 1.4 Deduplication
- Performs hash‑based and semantic similarity checks
- Prevents mirrored, repeated, or syndicated duplicates
- Ensures downstream modules receive clean, unique content

## 1.5 Classification
- Assigns topic labels
- Applies region‑aware classification rules
- Produces normalized metadata for routing and filtering

## 1.6 Handoff to Emergency Channel

    type: news
    region: <region>
    topic: <topic>
    content: <text>
    metadata: <normalized metadata>

---

# 2. Pseudonymous BBS Path

## 2.1 Account Login
- Lightweight account system
- No personal identifiers required
- Region‑aware access rules and rate limits

## 2.2 Pseudonym Generation
- System‑generated display name and avatar
- Prevents identity correlation across posts
- Ensures uniformity across regions

## 2.3 Post Creation
- User writes a message
- Client strips local metadata
- Server sanitizes HTML, markup, and embedded content

## 2.4 Sanitization
- Removes dangerous markup
- Normalizes text and formatting
- Ensures no fingerprintable artifacts remain
- Produces a clean internal message object

## 2.5 Handoff to Emergency Channel

    type: bbs
    region: <region>
    author: <generated pseudonym>
    content: <text>
    metadata: <normalized metadata>

---

# 3. Emergency Channel Processing

## 3.1 Sanitization Layer
- Removes remaining metadata
- Normalizes timestamps and region fields
- Ensures all messages conform to a unified internal schema

## 3.2 Storage Layer
- Stores messages in encrypted form
- Applies region‑aware retention policies
- Maintains minimal metadata required for routing
- Ensures no user‑identifiable information is retained

## 3.3 Routing Layer
- Determines delivery path based on region and threat level
- Applies multi‑hop routing rules
- Selects fallback transports when primary paths are blocked
- Ensures consistent behavior under adversarial conditions

## 3.4 Distribution Layer
- Converts messages into multiple output formats
- Selects protocol family based on region profile
- Supports offline, low‑bandwidth, and opportunistic modes
- Produces final deliverable artifacts for user devices

---

# 4. Multi‑Protocol Distribution

The Emergency Channel outputs content through multiple independent transport families, including:

- HTTPS (stealth mode)
- DNS‑based tunneling
- CDN‑backed distribution
- Covert channels (timing, padding, mimicry)
- Region‑specific fallback transports

Transport selection factors include:

- Network conditions
- Censorship intensity
- User region
- Protocol availability
- Fingerprint resistance requirements

The system dynamically adapts to ensure reliable delivery under hostile network conditions.

---

# Summary

The Emergency Publishing System processes data through two ingestion paths: news aggregation and pseudonymous posting. Both paths converge in the Emergency Channel, which performs sanitization, storage, routing, and multi‑protocol distribution. This unified flow ensures secure, resilient, and region‑aware delivery of published content, even in adversarial environments.
