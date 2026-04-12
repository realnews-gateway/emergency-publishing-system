# Emergency Channel — Route Selection

## 1. Purpose

Route selection determines **which internal content‑handling component**
(distributor, storage backend, micro‑feed path, or fallback route) should
process each sanitized content item.

The goal is to choose the most reliable and appropriate **content path**
based on system state, routing policies, and component health.

Route selection is:

- Deterministic  
- Fast  
- Non‑destructive  
- Independent of network‑layer routing  

It operates strictly at the **content layer**, not the network layer.

---

## 2. Selection Criteria

Route selection evaluates each **content node** (e.g., distributor,
storage, micro‑feed) using a combination of signals and policy rules.

### 2.1 Availability

A content node must:

- Pass recent health checks  
- Not be in a degraded or maintenance state  
- Report successful operations within a defined window  

Unavailable nodes are excluded immediately.

### 2.2 Backlog / Load

Content nodes are evaluated based on:

- Queue depth  
- Processing backlog  
- Throughput capacity  
- Recent delivery success rate  

Nodes nearing overload receive penalties.

### 2.3 Capability Requirements

A content node must support:

- The required content type  
- The required delivery mode (e.g., batch, real‑time, micro‑feed)  
- Any mandatory formatting or storage constraints  

Nodes lacking required capabilities are excluded.

### 2.4 Policy Constraints

Policies may require:

- Priority‑based routing  
- Content‑type‑specific routing  
- Mandatory storage for certain content classes  
- Fast‑path routing for urgent items  

Policies are applied before scoring.

---

## 3. Content‑Node Scoring

Each valid content node receives a score based on the selection criteria.

### 3.1 Base Score

All nodes begin with a neutral base score.

### 3.2 Availability Score

- Positive score for consistent health  
- Penalties for intermittent failures  
- Heavy penalties for recent degradation  

### 3.3 Load Score

- Lower backlog increases score  
- High queue depth reduces score  
- Severe overload results in exclusion  

### 3.4 Capability Score

- Required capabilities → mandatory  
- Optional capabilities → small bonuses  
- Missing required capabilities → exclusion  

### 3.5 Policy Score

- Priority content may boost fast‑path nodes  
- Archival content may boost storage nodes  
- Emergency content may boost micro‑feed paths  

All scoring is deterministic and reproducible.

---

## 4. Node Filtering

Before scoring, nodes are filtered out if:

- They are offline or unhealthy  
- They lack required capabilities  
- Their backlog exceeds hard limits  
- They are in maintenance mode  
- They cannot handle the content type  

Filtering ensures only valid candidates proceed to scoring.

---

## 5. Final Selection

After filtering and scoring, the Router selects the content node with the
highest final score.

If multiple nodes share the same score, deterministic tie‑breaking is
applied:

1. Prefer nodes with lower backlog  
2. Prefer nodes with fewer recent failures  
3. Prefer nodes with higher capability scores  
4. Fall back to lexicographical ordering of node IDs  

This ensures predictable, auditable routing behavior.

---

## 6. Failover Handling

If the selected content node fails during routing:

- The Router retries with the next highest‑scoring node  
- Failed nodes are penalized to avoid repeated selection  
- If all nodes fail, the Router escalates to degraded mode  
- Micro‑feed or minimal‑path routing may be activated as fallback  

Failover ensures content is never silently dropped.

---

## 7. Summary

Route selection is a deterministic, content‑layer process that evaluates
availability, backlog, capabilities, and policy constraints.

By combining filtering, scoring, and failover logic, the Router ensures
that each piece of sanitized content is routed through the most suitable
internal path before distribution and publication.
