# Emergency Channel — Router Module

## Overview

The Router module is the **content‑level decision layer** of the Emergency
Channel pipeline.  
Its role is to determine the optimal **content delivery path** within the
system — selecting which internal components should handle each piece of
sanitized content.

The Router does **not** perform network routing, proxy selection, or
transport‑layer decisions.  
Those responsibilities belong to the `network-access-layer`.

Instead, the Router focuses exclusively on **content flow routing**:
choosing the appropriate distributor, storage path, fallback route, or
emergency micro‑feed channel based on system state and content
characteristics.

---

## Responsibilities

The Router module is responsible for:

- **Content route selection**  
  Choosing the appropriate internal path (e.g., distributor, storage,
  micro‑feed).

- **Content node selection**  
  Selecting which *content‑handling component* should process or deliver
  the item next.

- **Fallback routing**  
  Switching to alternative distributors or minimal‑path routes when
  primary paths fail.

- **Deterministic decision‑making**  
  Ensuring identical inputs always produce identical routing outcomes.

- **Load distribution across content paths**  
  Balancing content across multiple distributors or storage backends.

Router decisions operate entirely at the **content layer**, not the
network layer.

---

## Non‑Responsibilities

The Router module does **not** perform:

- Network node selection  
- Proxy or upstream selection  
- Transport routing  
- Exit node or region routing  
- Bandwidth or link‑level failover  
- Any network‑layer decision‑making  

These responsibilities belong to:

- `network-access-layer`  
- `transport-layer`  
- `entry-point selection`  

This separation ensures a clean, auditable architecture.

---

## Module Structure

This directory contains the following files:

### Routing Architecture

- **design.md**  
  Describes the content‑routing architecture, decision flow, and routing
  strategies.

- **route-selection.md**  
  Defines how the Router selects the next content‑handling component
  (distributor, storage, micro‑feed, fallback path).

- **failover.md**  
  Explains how the Router detects content‑path failures and performs
  fallback routing.

### Future Extensions

Potential future enhancements include:

- Multi‑path content routing  
- Priority‑based routing policies  
- Content‑type‑aware routing  
- Adaptive routing based on distributor health  

All extensions remain strictly within the **content routing** domain.

---

## Deterministic Routing

Routing decisions must be deterministic:

- Identical inputs must produce identical routing choices  
- No randomness or probabilistic selection  
- No environment‑dependent behavior  
- No hidden heuristics  

Determinism ensures predictable, auditable routing behavior across
deployments.

---

## Security Boundary

The Router enforces strict security boundaries:

- Only sanitized content may enter the routing layer  
- Routing decisions must not leak internal metadata  
- No untrusted code execution  
- No external influence on routing decisions  

The Router treats all downstream components as potentially unreliable and
therefore routes conservatively and defensively.

---

## Summary

The Router module provides:

- Deterministic, content‑level routing  
- Reliable fallback and path selection  
- Strong separation from network‑layer routing  
- Clear architectural boundaries  
- A stable foundation for content distribution within the Emergency
  Channel

It ensures that sanitized content flows through the correct internal
paths before reaching external publishing channels.
