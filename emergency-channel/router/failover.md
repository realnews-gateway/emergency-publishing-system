# Emergency Channel — Router Failover

## 1. Purpose

Router failover ensures that **content continues to flow through the
Emergency Channel** even when internal content‑handling components
(distributors, storage backends, micro‑feed paths) experience failures.

Failover operates at the **content layer**, not the network layer.  
It does not handle network nodes, regions, or transport‑level outages —
those belong to the `network-access-layer`.

Router failover must be:

- Fast  
- Deterministic  
- Automatic  
- Non‑disruptive to upstream modules  

It guarantees that sanitized content is never silently dropped.

---

## 2. Failure Types

The Router recognizes several categories of **content‑path failures**.

### 2.1 Distributor‑Level Failures

- Distributor becomes unreachable  
- Distributor fails health checks  
- Distributor backlog exceeds safe thresholds  
- Distributor enters a degraded state  

### 2.2 Storage‑Level Failures

- Storage backend unavailable  
- Storage write failures  
- Storage quota or capacity exhaustion  
- Storage health signals indicate degradation  

### 2.3 Micro‑Feed Failures

- Micro‑feed channel unavailable  
- Minimal‑path encoder errors  
- Micro‑feed backend not ready  

### 2.4 Transient Failures

- Temporary backlog spikes  
- Short‑lived distributor slowdowns  
- Intermittent health‑check anomalies  

Transient failures require graceful handling without overreaction.

---

## 3. Detection Mechanisms

Failover is triggered based on signals from multiple internal modules:

- Distributor health reports  
- Storage availability signals  
- Backlog / queue depth thresholds  
- Micro‑feed readiness indicators  
- Delivery failure reports  
- Router‑level scoring penalties  

The Router aggregates these signals to determine when failover is
necessary.

---

## 4. Failover Process

Failover follows a deterministic sequence to ensure predictable behavior.

### 4.1 Step 1 — Detect Failure

A failure is confirmed when:

- Health checks fail repeatedly  
- Backlog exceeds configured thresholds  
- Storage reports write errors  
- Distributor returns structured failure codes  
- Micro‑feed path reports unavailability  

Only confirmed failures trigger failover.

### 4.2 Step 2 — Penalize Failed Content Node

Once a content node is marked as failed:

- Its score is reduced significantly  
- It is excluded from routing decisions  
- A cooldown timer prevents immediate re‑selection  
- Failure metadata is recorded for analysis  

This prevents repeated attempts to use unstable components.

### 4.3 Step 3 — Select Next Content Path

The Router selects the next viable content path:

- Another distributor  
- Storage fallback  
- Micro‑feed fallback  
- Minimal‑path routing  

If no primary paths remain, the Router escalates to degraded mode.

### 4.4 Step 4 — Retry Routing

The Router attempts to route the content through the new path.  
If this path also fails, the process repeats until a healthy path is
found or all options are exhausted.

### 4.5 Step 5 — Escalate if Necessary

If all content paths fail:

- Content may be queued for retry  
- Minimal‑path micro‑feed may be activated  
- System‑level degraded mode may be entered  
- Alerts may be emitted (if configured)  

Escalation ensures content is never silently lost.

---

## 5. Coordination with Other Modules

Router failover relies on coordination with:

- **Distributor** — delivery failures, backlog, health  
- **Storage** — availability, write errors, capacity  
- **Micro‑feed subsystem** — readiness, fallback availability  
- **Analytics** — long‑term failure patterns  
- **Core pipeline** — backpressure and overload signals  

Failover decisions are based on system‑wide signals, not isolated
symptoms.

---

## 6. Recovery and Reintegration

When a failed content node recovers, it must rejoin the routing pool
gradually.

### 6.1 Recovery Detection

A node is considered recovered when:

- It passes multiple consecutive health checks  
- Backlog returns to normal  
- No new errors occur within a defined window  
- Storage or distributor reports full availability  

### 6.2 Gradual Reintegration

Recovered nodes are reintegrated cautiously:

- Start with a low initial score  
- Increase score as successful operations accumulate  
- Monitor for regression  
- Restore full score only after sustained stability  

This prevents unstable nodes from re‑entering too aggressively.

---

## 7. Degraded Mode

If no healthy content paths are available, the Router enters degraded
mode:

- Content may be queued for delayed processing  
- Only essential routing logic is executed  
- Optional analytics or non‑critical tasks may be skipped  
- Minimal‑path micro‑feed may be used as last resort  

Degraded mode ensures the Emergency Channel remains operational under
extreme conditions.

---

## 8. Summary

Router failover is a deterministic, content‑layer mechanism that ensures
routing continuity when distributors, storage backends, or micro‑feed
paths fail.

Through structured detection, penalization, retry logic, coordination
with other modules, and controlled reintegration, the Router maintains
system reliability even under severe or unpredictable conditions.
