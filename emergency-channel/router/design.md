# Emergency Channel — Router Design

## 1. Purpose

The Router is the **content‑level decision engine** of the Emergency
Channel.  
Its purpose is to determine the optimal **content processing path** for
each sanitized item, selecting which internal component should handle it
next (e.g., distributor, storage, micro‑feed).

The Router does **not** perform network routing, proxy selection, or
transport‑layer decisions.  
Those responsibilities belong to the `network-access-layer`.

The Router is strictly **representation‑agnostic** and **non‑destructive**:
it does not modify content, only determines its internal path.

---

## 2. Responsibilities

The Router performs several key content‑routing functions:

### 2.1 Content Path Selection

- Selects the appropriate internal path based on content type, urgency,
  and system state  
- Chooses between distributor, storage, micro‑feed, or fallback paths  
- Ensures deterministic routing decisions for auditability  

### 2.2 Content Load Distribution

- Distributes content across multiple distributors  
- Prevents overload of any single content‑handling component  
- Balances throughput across available content paths  

### 2.3 Content‑Level Fault Tolerance

- Detects failures in content‑handling components (e.g., distributor
  unavailable)  
- Automatically reroutes content to healthy alternatives  
- Ensures no single point of failure in the content pipeline  

### 2.4 Policy Enforcement

- Applies routing rules defined by system configuration  
- Supports priority‑based or content‑type‑based routing policies  
- Ensures compliance with operational constraints  

All responsibilities operate strictly at the **content layer**, not the
network layer.

---

## 3. Routing Architecture

The Router is designed as a lightweight, stateless component that makes
fast, deterministic routing decisions.  
It does not store content or maintain long‑term state; instead, it relies
on signals from other modules (Distributor, Storage, Health subsystem).

### 3.1 Stateless Core

- Routing decisions are computed per request  
- No persistent state is stored inside the Router  
- All stateful information (health, availability, backlog) is fetched
  from external modules  

### 3.2 Signal‑Driven Decision Making

Routing decisions are influenced by:

- Distributor health signals  
- Storage availability  
- Micro‑feed readiness  
- Backlog or queue depth of content‑handling components  
- System‑wide routing policies  

No network‑layer metrics (latency, region, bandwidth, link health) are
used.

### 3.3 Modular Routing Pipeline

The routing pipeline consists of several isolated stages:

- Input normalization  
- Policy evaluation  
- Content‑node scoring  
- Path selection  
- Failover evaluation  
- Final routing decision  

Each stage is testable and replaceable, allowing the Router to evolve
without affecting the rest of the Emergency Channel.

---

## 4. Routing Strategies

The Router supports multiple **content‑level** routing strategies.  
These strategies can be combined or switched based on system state.

### 4.1 Priority‑Based Routing

- Routes urgent or critical content through fast‑path distributors  
- Defers low‑priority content to slower or batch‑oriented paths  

### 4.2 Availability‑Optimized Routing

- Prefers distributors or storage backends that are currently healthy  
- Avoids components with recent failures or degraded performance  

### 4.3 Load‑Aware Routing

- Distributes content based on backlog or queue depth  
- Prevents overload of any single distributor  
- Ensures smooth throughput under high load  

### 4.4 Policy‑Driven Routing

- Applies administrator‑defined routing rules  
- Supports content‑type‑based or urgency‑based routing  
- Ensures compliance with operational constraints  

All strategies operate strictly on **content‑handling components**, not
network nodes.

---

## 5. Extensibility

The Router is designed for long‑term extensibility:

- New routing strategies can be added without modifying existing ones  
- Content‑node scoring algorithms can be replaced or enhanced  
- Additional health or backlog signals can be integrated  
- Failover logic can be extended to support new content paths  

The modular architecture ensures that improvements to routing logic do
not disrupt the rest of the Emergency Channel.

---

## 6. Summary

The Router is a stateless, deterministic, and highly extensible
content‑routing component.  
Its design ensures reliable path selection, strong fault tolerance, and
clean separation from the network layer.

It forms a critical part of the Emergency Channel pipeline, ensuring that
sanitized content flows through the correct internal paths before
distribution and publication.
