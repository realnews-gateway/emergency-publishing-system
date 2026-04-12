# Router — Health Checks

## Overview

This document defines the health probe contract used by the Router to evaluate the readiness, availability, and stability of all content-handling components (distributors, storage backends, micro-feed paths).  
All probes operate strictly at the content layer.

---

## Probe Types

### Liveness probe
Indicates whether the component is running and able to accept work.

### Readiness probe
Indicates whether the component is ready to process new content.

### Backlog probe
Reports queue depth, backlog pressure, and throughput capacity.

### Operational probe
Reports recent success and failure counts for content delivery.

### Capability probe
Reports supported content types, modes, and optional features.

---

## Probe Payload Format

All probes must emit a structured JSON object with the following fields:

```json
{
  "component_id": "string",
  "timestamp": "ISO8601 string",
  "probe_type": "liveness | readiness | backlog | operational | capability",
  "status": "healthy | degraded | unhealthy",
  "metrics": { "key": "value" },
  "message": "optional human readable note"
}
```

### Example

```json
{
  "component_id": "distributor-42",
  "timestamp": "2026-04-13T04:05:00Z",
  "probe_type": "backlog",
  "status": "degraded",
  "metrics": {
    "queue_depth": 1200,
    "processing_rate_per_min": 300
  },
  "message": "queue depth above soft limit"
}
```

---

## Thresholds and Debounce Rules

To avoid overreacting to transient anomalies, the Router applies debounce logic:

- **probe_interval_s**: default 10 seconds  
- **failure_threshold**: 3 consecutive failing probes → mark unhealthy  
- **recovery_threshold**: 2 consecutive healthy probes → mark recovered  
- **history_window_s**: 300 seconds of rolling probe history

Soft limits trigger warnings but do not exclude the component.  
Hard limits trigger immediate exclusion from routing.

---

## Health Aggregation

The Router maintains a rolling history for each component:

- recent probe statuses  
- backlog metrics  
- operational success/failure counts  
- capability changes

From this, the Router computes a health score used for:

- availability scoring  
- failover decisions  
- degraded-mode activation

Components below threshold are excluded.

---

## Signal Sources

The Router receives probes from:

- Distributor: liveness, readiness, backlog, operational  
- Storage: liveness, readiness, operational  
- Micro-feed: readiness, capability  
- Monitoring: aggregated alerts as operational probes

All signals must be authenticated.

---

## Operational Recommendations

- Use UTC timestamps.  
- Keep probes lightweight.  
- Monitor probe delivery latency and loss.  
- Extend probe schema only with versioning.  
- Ensure backlog probes reflect real queue depth.

---

## Troubleshooting

- If a component is repeatedly degraded, inspect backlog and operational metrics.  
- If probes are missing, verify authentication and delivery pipeline.  
- Correlate Router failover events with probe history.

---
