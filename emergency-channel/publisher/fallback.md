# Publisher Module — Fallback & Resilience

## Overview

The Publisher module includes a dedicated fallback subsystem designed to
ensure that content remains deliverable even under severe network
degradation, censorship, or partial infrastructure failure.  
Fallback mechanisms operate automatically and transparently, without
requiring upstream modules to change their behavior.

The fallback layer is **modular, deterministic, and extensible**, allowing
deployments to customize or extend fallback strategies based on
operational needs.

---

## Fallback Triggers

Fallback is activated when any of the following conditions occur:

- **Primary channel failure**  
  The main publishing channel returns an error or becomes unreachable.

- **High latency or timeouts**  
  Delivery exceeds configured thresholds.

- **Censorship indicators**  
  Repeated connection resets, TLS interference, or content filtering
  patterns.

- **Network instability**  
  Packet loss, intermittent connectivity, or degraded bandwidth.

- **Channel health check failures**  
  Scheduled probes indicate that a channel is unhealthy.

Fallback triggers are logged for later analysis but do not interrupt the
publishing pipeline.

---

## Fallback Strategies

The Publisher module supports multiple fallback strategies:

### 1. Channel Switching
Automatically switches from a primary channel (e.g., HTML publishing) to
an alternative channel (e.g., RSS, JSON API, IPFS).

### 2. Multi-Channel Broadcasting
Delivers content through several channels simultaneously to maximize
reach.

### 3. Minimal Payload Mode
Generates a text-only micro-feed optimized for extreme censorship
environments.

### 4. Opportunistic Delivery
Attempts delivery when temporary network windows appear, useful in
unstable or high-risk regions.

### 5. Offline Export
Packages content into bundles that can be distributed manually or through
offline networks.

---

## Health Checks

The fallback system relies on continuous health monitoring:

- Periodic channel probes  
- Latency and error-rate tracking  
- TLS handshake anomaly detection  
- Endpoint availability checks  

Health data is used to determine when to switch channels or degrade
gracefully.

---

## Summary

The fallback subsystem ensures:

- High reliability under adverse network conditions  
- Automatic recovery from channel failures  
- Multiple redundant delivery paths  
- Compatibility with both online and offline environments  
- Strong resilience against censorship and interference  

It guarantees that verified content reaches users even when the network
is hostile or unstable.
