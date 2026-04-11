# Emergency Channel — Distributor Module

The Distributor module is responsible for delivering processed content
from the Emergency Channel to all downstream distribution endpoints. It
coordinates multi‑transport delivery, region‑aware fallback, DTN bundles,
and redundant distribution paths to ensure that content remains
accessible even under severe censorship or network degradation.

The Distributor does not interact with user accounts, submission
metadata, or application‑level identity. It operates exclusively on
sanitized, chunked, redundancy‑encoded content produced by the Emergency
Channel pipeline.

---

## 1. Purpose

The Distributor ensures:

- Reliable delivery across unstable or censored networks
- Multi‑transport, multi‑region distribution
- Region‑aware fallback and retry logic
- DTN (Delay‑Tolerant Networking) support for intermittent connectivity
- Delivery verification and integrity checks
- Redundant distribution paths for survivability

It is the final stage of the Emergency Channel pipeline.

---

## 2. Responsibilities

The Distributor handles:

- Transport selection (REALITY, uTLS, XTLS‑Vision, XHTTP, VLESS, TUIC v5)
- Multi‑path scheduling
- Region‑aware routing decisions
- Fallback logic for degraded networks
- DTN bundle generation and delivery
- Delivery confirmation and integrity verification
- Coordination with storage backends for retrieval

The Distributor does not:

- Process raw content
- Perform sanitization
- Handle user identity or accounts
- Interpret application‑level semantics

---

## 3. Inputs and Outputs

### Input
- RedundantBundle (from redundancy stage)
- RoutingHints (from routing stage)
- Storage references (from storage stage)

### Output
- Delivered content across multiple transports
- DTN bundles for offline or intermittent networks
- Delivery reports for internal monitoring

No user‑visible metadata is produced.

---

## 4. Integration with Emergency Channel

The Distributor is tightly integrated with:

- Routing (transport scoring, fallback chains)
- Storage (regional retrieval, DTN bundle sources)
- Monitoring (network health, region degradation signals)

It receives only sanitized, identity‑free content.

---

## 5. Submodule Documentation

- **overview.md**  
  High‑level design and responsibilities.

- **pipeline.md**  
  Distribution workflow, transport selection, fallback logic, and DTN
  behavior.

- **endpoints.md**  
  Defines distribution endpoints, including application endpoints,
  regional storage nodes, multi‑transport delivery targets, and offline
  bundle generators.

---

The Distributor module provides the final, resilient delivery layer of
the Emergency Channel, ensuring that content reaches its destinations
despite censorship, interference, or network instability.
