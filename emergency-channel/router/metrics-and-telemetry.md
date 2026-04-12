# Metrics and Telemetry

## Overview

This document defines the metrics and telemetry contract used across Router, Distributors, Storage, and Micro‑feed components.  
It specifies metric types, naming conventions, labels, export formats, retention, and recommended instrumentation practices.

---

## Goals

- Provide consistent, actionable metrics for alerting and capacity planning.  
- Ensure telemetry is lightweight and safe for high‑throughput paths.  
- Enable correlation between traces, logs, and metrics.

---

## Metric Types

### Counters
Monotonic increasing values (e.g., requests served, bytes processed).

### Gauges
Point-in-time values (e.g., queue depth, memory usage).

### Histograms / Summaries
Distributions for latencies, payload sizes, and processing durations.

### Events
Discrete occurrences with rich attributes (e.g., failover, schema change).

---

## Naming Conventions

- Use **dot separated** lowercase names: `service.component.metric_name`  
- Prefix by service: `router.distributor.queue_depth`  
- Suffix units when applicable: `*_bytes`, `*_seconds`, `*_count`  
- Use consistent verbs for counters: `*_total`, `*_errors_total`

**Examples**

- `router.distributor.queue_depth` (gauge)  
- `router.requests_processed_total` (counter)  
- `storage.write_latency_seconds` (histogram)  
- `microfeed.capability_change_event` (event)

---

## Labels (Tags)

Use labels to slice metrics; keep cardinality low.

Recommended labels:

- **component_id**: unique component instance id  
- **region**: deployment region or zone  
- **env**: prod|staging|canary|dev  
- **content_type**: video|image|text|meta  
- **outcome**: success|failure|timeout|rejected

Avoid high-cardinality labels such as user_id, request_id, or full resource paths.

---

## Telemetry Schema

All telemetry exports must include a minimal envelope with metadata and payload.

```json
{
  "service": "string",
  "component_id": "string",
  "env": "string",
  "region": "string",
  "timestamp": "ISO8601 string",
  "metrics": [
    {
      "name": "string",
      "type": "counter | gauge | histogram | summary",
      "value": "number or object",
      "labels": { "key": "value" }
    }
  ],
  "events": [
    {
      "name": "string",
      "timestamp": "ISO8601 string",
      "attributes": { "key": "value" }
    }
  ]
}
```

### Histogram Example

```json
{
  "name": "storage.write_latency_seconds",
  "type": "histogram",
  "value": {
    "buckets": { "0.01": 120, "0.05": 450, "0.1": 900, "0.5": 1200, "+Inf": 1500 },
    "sum": 75.32,
    "count": 1500
  },
  "labels": { "component_id": "storage-7", "region": "us-east-1", "env": "prod" }
}
```

---

## Telemetry Events Example

```json
{
  "name": "router.failover",
  "timestamp": "2026-04-13T05:00:00Z",
  "attributes": {
    "component_id": "distributor-42",
    "reason": "backlog_exceeded",
    "previous_health_score": 0.42,
    "new_health_score": 0.12
  }
}
```

---

## Export Formats and Protocols

- Prefer **OpenTelemetry** protocol for traces and metrics.  
- Support Prometheus exposition for pull-based scraping of metrics.  
- Provide JSON/HTTP POST endpoints for aggregated telemetry ingestion where required.  
- Ensure TLS and authentication for all telemetry endpoints.

---

## Sampling and Aggregation

- **Traces**: sample at a configurable rate; increase sampling for errors and slow traces.  
- **Metrics**: aggregate at source when possible (e.g., local process aggregation) to reduce cardinality and network overhead.  
- **Events**: sample non-critical high-volume events; always capture all error and failover events.

---

## Retention and Storage

- Short-term high-resolution retention (e.g., 1s–10s granularity) for 7–30 days.  
- Medium-term downsampled retention (e.g., 1m granularity) for 90 days.  
- Long-term aggregated retention (e.g., hourly/daily rollups) for 1–3 years depending on compliance and capacity planning needs.

---

## Alerting Guidance

- Alert on **rate of change** and **threshold breaches** combined with debounce windows.  
- Use multi-metric alerts for robust detection (e.g., queue_depth + processing_rate drop).  
- Avoid alerts on single transient spikes; require sustained violation (e.g., 3 samples over 30s).

---

## Instrumentation Best Practices

- Use client libraries that support OpenTelemetry.  
- Emit **component_id** and **trace_id** labels to enable cross-correlation.  
- Keep metric emission lightweight and non-blocking.  
- Guard telemetry emission behind circuit breakers to avoid cascading failures.  
- Version metric names when changing semantics; do not silently repurpose names.

---

## Security and Privacy

- Do not include PII or sensitive identifiers in metric labels or event attributes.  
- Encrypt telemetry in transit and authenticate producers.  
- Rate-limit telemetry ingestion to prevent abuse.

---

## Operational Recommendations

- Monitor telemetry pipeline health (ingestion latency, drop rates, processing errors).  
- Maintain a schema registry or documentation for all exported metric names and labels.  
- Run periodic audits to detect high-cardinality labels or unexpected metric growth.  
- Provide dashboards for SLOs, capacity, and error budgets.

---

## Troubleshooting

- If metrics are missing: verify exporter configuration, network connectivity, and authentication.  
- If cardinality spikes: inspect recent deployments for label changes; roll back if needed.  
- If ingestion latency increases: check pipeline backpressure, storage write throughput, and downstream consumers.

---

## Appendix — Common Metrics

- **router.requests_processed_total** (counter)  
- **router.requests_errors_total** (counter) with label `outcome=timeout|validation|backend_error`  
- **router.distributor.queue_depth** (gauge)  
- **storage.write_latency_seconds** (histogram)  
- **storage.write_errors_total** (counter)  
- **microfeed.capability_changes_total** (counter)  
- **system.cpu_usage_percent** (gauge)  
- **system.memory_bytes** (gauge)

---
