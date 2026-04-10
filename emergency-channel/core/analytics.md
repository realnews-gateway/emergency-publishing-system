# Emergency Channel — Analytics Module

The Analytics module provides internal, system-level metrics for the
Emergency Channel. It focuses on transport viability, routing behavior,
redundancy effectiveness, storage health, and region-level availability.
No user-level analytics or behavioral tracking is performed.

---

## 1. Purpose

The module provides:
- Operational visibility into routing and transport performance
- Reliability metrics for chunk delivery and redundancy
- Region-aware availability insights
- Health indicators for storage, sanitizer, and distributor

All analytics are aggregated and anonymized.

---

## 2. Responsibilities

### Transport and Routing Metrics
- Viability scores for REALITY, uTLS, XTLS-Vision, XHTTP Stream/Packet,
  VLESS, TUIC v5
- Fallback frequency and fallback chains
- Multi-hop routing success rates
- Region-specific degradation patterns

### Chunk Delivery and Redundancy
- Chunk delivery success and failure ratios
- Redundancy reconstruction success rate
- DTN bundle latency
- Retry patterns

### Storage and Sync
- Regional storage availability
- DTN sync success rate
- Retention policy compliance
- Storage backend health

### Sanitizer and Validation
- Sanitizer rejection rate (system-level)
- Validation failures (format, structure, chunk map)
- Escalation categories

### System Health
- Module failure trends
- Pipeline latency
- Region-level availability degradation
- Impact of routing or transport configuration changes

---

## 3. Data Flow

Events are sent asynchronously from:
core, router, sanitizer, storage, distributor, monitoring.

Analytics must never block the pipeline.

---

## 4. Data Model

### Event Format
{
  "event_type": "string",
  "timestamp": <unix>,
  "source": "core|router|sanitizer|storage|distributor|monitoring",
  "payload": { ... }
}

### Aggregated Metrics
{
  "metric": "transport_viability",
  "window": "1h",
  "avg": 0.92,
  "p95": 0.75,
  "samples": 1842
}

---

## 5. Dashboards (Optional)

Possible dashboards:
- Transport viability
- Fallback chain visualization
- Chunk delivery success
- Redundancy effectiveness
- DTN sync latency
- Sanitizer rejection
- Storage availability

---

## 6. Privacy and Safety

- No user-level analytics
- No behavioral tracking
- No content inspection
- Only aggregated metrics
- Strict retention policies
- Access must be restricted and auditable

---

## 7. Future Extensions

- Predictive routing
- Censorship anomaly detection
- Region availability forecasting
- Automated fallback tuning
- Optional integration with observability systems
