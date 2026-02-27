# Deployment Models

This document describes the deployment models of the Emergency Publishing System. Different regions require different operational strategies, levels of resilience, and infrastructure footprints. The system is designed to operate across environments with varying censorship intensity, network stability, and security risks.

Deployment models are categorized into four tiers:
1. Standard Deployment
2. Resilient Deployment
3. High‑Risk Deployment
4. Offline and Hybrid Deployment

All models share the same core architecture but adjust transports, routing rules, and retention policies based on regional conditions.

---

# 1. Standard Deployment

Used in regions with moderate or low censorship.

## Characteristics
- Stable network conditions
- Minimal blocking or throttling
- Standard HTTPS transports available
- CDN‑backed delivery is reliable

## Components
- Emergency Channel (full features)
- vpn-access-layer (optional)
- news-aggregation module
- anonymous-bbs module

## Operational Notes
- HTTPS and CDN‑backed delivery are preferred
- Routing is optimized for throughput rather than evasion
- Retention policies can be longer
- Monitoring focuses on performance and availability

This model provides the highest performance and lowest operational complexity.

---

# 2. Resilient Deployment

Used in regions with active censorship but not extreme suppression.

## Characteristics
- DPI and selective blocking present
- Intermittent throttling
- Some transports unreliable or unstable

## Components
- Emergency Channel with fallback routing enabled
- vpn-access-layer required
- Obfuscated transports enabled
- Region‑aware routing rules active

## Operational Notes
- Transport selection becomes adaptive
- Obfuscation and mimicry patterns are used
- Retention policies become shorter
- Distribution uses multiple parallel transports

This model balances performance and resilience.

---

# 3. High‑Risk Deployment

Used in regions with heavy censorship, surveillance, or targeted disruption.

## Characteristics
- Aggressive DPI and protocol fingerprinting
- Active probing of endpoints
- Frequent blocking of known transports
- High surveillance risk for users

## Components
- Emergency Channel with strict sanitization
- vpn-access-layer with covert entry points
- Covert transports enabled by default
- anonymous-bbs with enhanced pseudonymity
- Region‑specific fallback chains

## Operational Notes
- Standard transports are rarely used
- Covert channels and DNS‑based tunneling become primary
- Routing is randomized and multi‑hop
- Retention policies are minimal
- Distribution favors small, redundant message chunks

This model prioritizes survivability over performance.

---

# 4. Offline and Hybrid Deployment

Used in regions with unstable connectivity or near‑total network shutdowns.

## Characteristics
- Intermittent or no internet access
- Local networks may still function
- Users rely on opportunistic sync

## Components
- Emergency Channel with offline mode
- Local caching nodes
- Delay‑tolerant message bundles
- Region‑aware sync rules

## Operational Notes
- Messages are stored locally until sync is possible
- Sync occurs opportunistically through any available transport
- Distribution uses compact, low‑bandwidth formats
- Routing avoids assumptions about connectivity

This model ensures the system continues functioning even during large‑scale outages.

---

# Deployment Selection Logic

The system selects a deployment model based on:

- Region
- Censorship intensity
- Network stability
- Transport availability
- User risk level

## Selection Process

    1. Evaluate region profile
    2. Determine censorship level
    3. Select appropriate deployment tier
    4. Enable or disable modules accordingly
    5. Apply region‑specific routing and retention rules

Deployment models can switch dynamically if conditions change.

---

# Summary

The Emergency Publishing System supports multiple deployment models to adapt to different censorship environments. Standard deployments prioritize performance, resilient deployments balance speed and evasion, high‑risk deployments maximize survivability, and offline deployments ensure continuity during network collapse. All models share the same core architecture but adjust transports, routing, and retention policies to match regional conditions.
