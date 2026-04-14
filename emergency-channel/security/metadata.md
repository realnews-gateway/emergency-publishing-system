---
owner: "@Empus/security"
oncall: "security-oncall@company.com"
last-reviewed: "2026-04-15"
---

# Security Module — Metadata Minimization

## Overview

The metadata minimization layer ensures that the Emergency Channel system does not leak sensitive operational details through logs, payloads, headers, or publishing artifacts.  
Metadata can be more revealing than content itself: it may expose identities, timing patterns, infrastructure layout, or internal processing behavior.

This document defines what metadata may be generated, stored, transmitted, or exported, and prescribes normalization, anonymization, and retention rules to reduce correlation and fingerprinting risk.

---

## Core Principles

Metadata minimization is based on the following principles:

- **Only keep what is necessary**  
  Remove or replace all non-essential metadata.

- **Never expose internal details**  
  Strip internal routing, internal module identifiers, raw timestamps, and debug traces before export.

- **Minimize correlation risk**  
  Avoid metadata that could link multiple pieces of content or reveal user behavior.

- **Uniformity over uniqueness**  
  Prefer generic, predictable metadata to avoid fingerprinting.

- **Defend against traffic analysis**  
  Reduce timing, size, and structural signals that adversaries could exploit.

These principles apply to every module in the pipeline and to all telemetry exported outside the internal trust boundary.

---

## Metadata Categories and Handling

### 1. Internal Processing Metadata
**Examples:** module names, internal IDs, routing paths, processing timestamps, debug traces.  
**Handling:** Remove or replace with non‑reversible, aggregated values. Do not export raw processing traces; keep them internal and access‑controlled.

### 2. User‑Related Metadata
**Examples:** IP addresses, user identifiers, device fingerprints, behavioral patterns.  
**Handling:** Never export raw user identifiers. Use deterministic hashing only for internal correlation and only in access‑restricted stores; external exports must use aggregated buckets.

### 3. Network and Transport Metadata
**Examples:** source/destination hints, TLS session identifiers, timing patterns.  
**Handling:** Use encrypted channels; avoid exporting TLS internals. Normalize timing and avoid unique transport identifiers in external artifacts.

### 4. Storage Metadata
**Examples:** filesystem paths, internal record IDs, raw encryption key identifiers.  
**Handling:** Strip or replace with per‑module non‑reversible identifiers. Never include raw key material or plaintext storage references in exported metadata.

### 5. Publishing Metadata
**Examples:** server-side timestamps, internal version numbers, channel-specific internal IDs.  
**Handling:** Publish only sanitized, minimal metadata. Do not include processing history or internal timestamps; if a timestamp is required, use a public, sanitized timestamp field.

---

## Allowed Metadata (examples)

Retain only metadata that is:

- **Required for correct operation**, and  
- **Non-sensitive**, and  
- **Non-identifying**, and  
- **Non-correlatable**.

Permitted examples:
- Content title (if non-identifying)  
- Public timestamp (optional, sanitized/rounded)  
- Language  
- Broad category or tags (predefined, low‑cardinality)  
- Channel type (e.g., "emergency", "public")

All allowed metadata values must be drawn from controlled enumerations documented in `metrics-values.md` or equivalent.

---

## Normalization and Anonymization Techniques

To reduce fingerprinting and correlation risk, apply the following techniques:

- **Timestamp handling**: round timestamps to coarse buckets or replace with relative buckets; avoid exporting high‑resolution timestamps.  
- **Size normalization**: optionally pad payload sizes to reduce size‑based fingerprinting.  
- **Field normalization**: enforce canonical field ordering and canonical serialization.  
- **Noise and jitter**: where appropriate, add small, bounded noise to timing signals before export.  
- **Identifier regeneration**: regenerate identifiers per module or per export session; avoid persistent cross‑module identifiers.  
- **Aggregation**: export only aggregated counts or histograms for high‑volume signals.

Document the chosen normalization parameters and aggregation windows in each module's implementation notes.

---

## Transport Considerations

- Use encrypted channels for all internal transport.  
- Avoid custom headers that leak internal topology.  
- Do not include unique identifiers in URLs or query strings that are exported.  
- Use uniform request patterns where feasible to reduce observable differences between requests.

---

## Logging and Telemetry

- **Internal logs** may contain higher‑fidelity metadata but must remain in access‑controlled internal stores.  
- **Exported telemetry** must be anonymized and aggregated; never export per‑submission identifiers or raw removed metadata lists.  
- **Correlation tokens**: use ephemeral, short‑lived correlation tokens for debugging; do not persist or export them externally.  
- **Audit logs**: retain detailed audit logs internally for incident response, but apply strict RBAC and retention rules (see `audit-and-logging.md`).

---

## Publishing Rules

When preparing content for external publishing:

- Strip all internal processing metadata.  
- Replace internal timestamps with sanitized public timestamps or omit them.  
- Remove module names, routing information, and storage references.  
- Ensure any retained metadata is drawn from the allowed list and uses controlled enumerations.

---

## Testing and Validation

- **Unit tests**: assert that modules strip prohibited metadata fields before export.  
- **Integration tests**: validate that exported artifacts contain only allowed metadata and conform to normalization rules.  
- **Adversarial tests**: simulate correlation attacks to verify that normalization and aggregation prevent linkage.  
- **CI gates**: include automated checks that scan for prohibited fields (e.g., `user_id`, `ip_address`, `internal_id`) in files intended for export.

---

## Governance

- Define allowed metadata enumerations and publish them in `metrics-values.md` or a central policy file.  
- Changes to allowed metadata or normalization parameters require approval from `@Empus/security`.  
- Review metadata policies quarterly or after any incident that indicates leakage risk.

---

## References

- `audit-and-logging.md`  
- `trust-boundaries.md`  
- `key-management.md`

---

## Summary

Metadata minimization reduces the risk that adversaries can infer sensitive information from non‑content signals. By enforcing strict removal, normalization, and aggregation rules, the Emergency Channel minimizes correlation and fingerprinting risks while preserving necessary operational observability.
