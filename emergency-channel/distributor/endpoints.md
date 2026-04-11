# Distribution Endpoints

This document defines the downstream distribution endpoints used by the
Distributor module. Endpoints represent the final destinations where
sanitized, redundancy‑encoded content is delivered. They are technical
targets, not organizational partners, and require no trust relationships
or onboarding procedures.

Endpoints include application consumers, regional storage nodes,
multi‑transport delivery targets, and offline bundle generators.

---

## 1. Application Endpoints

Application endpoints consume Emergency Channel output and present it to
end users. These endpoints do not receive account identifiers, metadata,
or unsanitized content.

Examples:

- Anonymous BBS (pseudonymous publishing)
- News aggregation interfaces
- Tools or utilities that consume Emergency Channel content

Properties:

- Read‑only access to sanitized content
- No identity linkage
- No transport metadata
- No write‑back to the Emergency Channel

---

## 2. Regional Storage Endpoints

Regional storage endpoints provide localized persistence for content
retrieved from the Emergency Channel. They support:

- Region‑aware replication
- Localized retrieval for downstream applications
- Fallback access when upstream connectivity is degraded

These endpoints do not modify content and do not store user metadata.

---

## 3. Multi‑Transport Delivery Targets

These endpoints represent delivery paths across multiple censorship‑
resistant transports, including:

- REALITY
- uTLS
- XTLS‑Vision
- XHTTP (Stream and Packet modes)
- VLESS
- TUIC v5

The Distributor selects and schedules delivery across these transports
based on:

- Regional network conditions
- Transport scoring
- Fallback chains
- Degradation signals

These endpoints are stateless and do not retain identity information.

---

## 4. DTN (Delay‑Tolerant Networking) Endpoints

DTN endpoints support delivery in environments with:

- Intermittent connectivity
- High packet loss
- Air‑gapped or partially connected networks

DTN endpoints may include:

- Bundle generators
- Local relay devices
- Opportunistic peer‑to‑peer carriers

DTN bundles contain only sanitized, redundancy‑encoded content.

---

## 5. Offline Distribution Endpoints

Offline endpoints enable content delivery without continuous network
access. Examples include:

- Offline bundle generators
- Local storage devices
- Portable relay nodes

These endpoints are used in extreme censorship or blackout scenarios.

---

## 6. Endpoint Requirements

All endpoints must:

- Serve content exactly as received
- Preserve redundancy encoding
- Avoid injecting metadata or identifiers
- Avoid modifying or reordering content
- Avoid storing transport metadata or identity information

Endpoints do not require:

- Trust relationships
- Organizational affiliation
- Onboarding or approval
- Persistent connectivity

---

## 7. Endpoint Isolation

Endpoints are strictly isolated from:

- User accounts
- Application identity
- Transport metadata
- Submission metadata
- Emergency Channel internal state

They receive only sanitized, identity‑free content.

---

## Summary

Distribution endpoints represent the final destinations for Emergency
Channel output. They include application consumers, regional storage
nodes, multi‑transport delivery targets, DTN endpoints, and offline
delivery mechanisms.

All endpoints operate without trust assumptions, without identity
linkage, and without modifying sanitized content.
