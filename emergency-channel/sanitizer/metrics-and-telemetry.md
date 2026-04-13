Metrics and Telemetry

Purpose
Define the metrics, telemetry, and observability requirements for the Sanitizer component under emergency-publishing-system/emergency-channel/sanitizer. This document specifies what to measure, how to instrument, privacy and aggregation rules, retention policies, alerting thresholds, and example schemas for structured telemetry artifacts.

---

Goals
- Detect and respond to operational failures, regressions, and adversarial activity.  
- Measure determinism and correctness of sanitization outputs.  
- Provide actionable signals for capacity planning, performance tuning, and security monitoring.  
- Preserve privacy by anonymizing and aggregating telemetry before external exposure.  
- Keep telemetry low‑cardinality and cost‑predictable.

---

Telemetry Categories
1. Operational Metrics — performance and resource usage.  
2. Processing Metrics — per‑submission pipeline metrics and stage outcomes.  
3. Security and Adversarial Metrics — detections, anomaly scores, and adversarial counters.  
4. Reliability and SLO Metrics — availability, latency SLOs, error budgets.  
5. Audit and Compliance Artifacts — structured, versioned processing reports (anonymized).  
6. Tracing and Logs — structured traces for debugging and minimal logs for post‑incident analysis.

---

Instrumentation Principles
- Deterministic tagging: Use a fixed set of tags/labels. Do not emit high‑cardinality identifiers (user IDs, raw submission IDs) in metrics. Use hashed or bucketed identifiers only in internal, access‑restricted traces.  
- Anonymization: All telemetry leaving the secure environment must be anonymized and aggregated. Raw processing reports containing reversible identifiers must remain internal and access‑controlled.  
- Low cardinality: Limit label values to controlled enumerations (e.g., format=text|image|video|document, stage=intake|detection|removal|reencode|package, result=ok|recoverableerror|unrecoverableerror|rejected).  
- Sampling: Apply deterministic sampling for high‑volume events (e.g., 1% sample for large media processing traces) while ensuring sampled sets are representative.  
- Cost predictability: Prefer counters and histograms with bounded buckets over unbounded distributions.

---

Required Metrics (examples and naming conventions)
Use Prometheus‑style metric names and conventions. All metrics must include service=sanitizer and channel=emergency labels.

| Metric | Type | Labels (examples) | Purpose |
|---|---:|---|---|
| sanitizersubmissionstotal | Counter | format, result | Count of submissions processed by result |
| sanitizerstageduration_seconds | Histogram | stage, format | Latency distribution per pipeline stage |
| sanitizerreencodecount_total | Counter | format, fallback | Count of reencodes and fallback usage |
| sanitizerrejectiontotal | Counter | reason, format | Count of rejections by reason |
| sanitizerresourcecpuseconds | Gauge | tasktype | CPU seconds consumed per task type (sampled/aggregated) |
| sanitizerresourcememorybytes | Gauge | tasktype | Memory usage high‑water mark per task (aggregated) |
| sanitizeradversarialdetectionstotal | Counter | detectiontype, severity | Adversarial detection counts |
| sanitizerprocessingrisk_score | Gauge | bucket | Aggregated risk score distribution (bucketed) |
| sanitizerslolatencyp99seconds | Gauge | stage | SLO tracking metric for p99 latency |

---

Metric Labeling and Cardinality Rules
- Allowed label keys: service, channel, format, stage, result, reason, fallback, detectiontype, severity, tasktype, bucket.  
- Prohibited labels: any raw identifiers (user IDs, email addresses, full submission IDs, IP addresses).  
- Label value limits: each label must have a bounded set of values documented in metrics-values.md. New values require governance approval.

---

Processing Report Schema
Every processed submission must produce an internal, versioned SanitizationReport. These reports are stored in internal telemetry stores and are not exported externally without anonymization.

```json
{
  "version": "string",
  "submission_id": "string",
  "removed_metadata": ["string"],
  "reencoded": true,
  "format_conversion": "string or null",
  "processing_steps": [
    { "stage": "string", "status": "ok | recoverableerror | unrecoverableerror", "notes": "string" }
  ],
  "risk_score": 0.0,
  "timestamp": "ISO8601 string",
  "toolchain_versions": { "encoder": "string", "muxer": "string", "lib": "string" }
}
```

Storage and access: Reports are stored in an internal, access‑controlled store for a limited retention period (see Retention Policy). Access requires audit logging and role‑based permissions.

---

Tracing and Logs
- Traces: Instrument each pipeline stage with trace spans. Traces may include hashed submission identifiers for internal correlation only. Traces must be sampled and retained per retention rules.  
- Structured logs: Emit minimal structured logs for errors and recoverable events. Logs must never contain raw content or reversible identifiers. Use consistent keys: timestamp, service, channel, stage, event, status, anonidhash (optional, internal only).  
- Correlation: Use a short, ephemeral correlation token for debugging that is rotated per task and not persisted in external systems.

---

Alerts and Dashboards

Alerting
Define actionable alerts with clear runbooks. Examples:
- High rejection rate: sanitizerrejectiontotal increases > 5% of submissions over 5m → investigate regressions or upstream format changes.  
- Stage latency breach: sanitizerstageduration_seconds p99 for reencode > configured threshold → scale or investigate encoder regressions.  
- Adversarial spike: sanitizeradversarialdetections_total increases sharply → trigger security review and rate limiting.  
- Resource exhaustion: sustained high sanitizerresourcememory_bytes → throttle or reject large submissions.

Dashboards
Provide dashboards for:
- Throughput and latency per format and stage.  
- Rejection reasons and trends.  
- Fallback usage and encoder health.  
- Adversarial detection trends and false positive rates.  
- Resource utilization and queue depth.

---

SLOs and Error Budgets
- Availability SLO: 99.9% of submissions receive a deterministic outcome (ok or rejected) within the global processing time budget.  
- Latency SLO: 99th percentile end‑to‑end processing time per format must be below configured thresholds (documented per format).  
- Error budget: Define monthly error budgets tied to rejection rates and unrecoverable errors. Exceeding budgets triggers incident review and rollback of recent changes.

---

Privacy, Aggregation, and Export Rules
- Internal vs external telemetry: Raw processing reports and high‑cardinality traces remain internal. Aggregated metrics and anonymized reports may be exported to centralized monitoring.  
- Anonymization: Before any export, remove or hash identifiers and aggregate to minimum group sizes. Do not export per‑submission risk scores or raw removed metadata lists.  
- Export formats: Use aggregated time buckets (1m, 5m) and pre‑defined rollups for external dashboards.  
- Third‑party telemetry: Only export aggregated, anonymized metrics to third parties after governance approval.

---

Retention and Storage
- Metrics: retain high‑resolution metrics (1m) for 30 days; downsampled aggregates (5m) for 90 days; long‑term rollups (1h) for 2 years.  
- Traces: sampled traces retained for 30 days.  
- Processing reports: internal reports retained for 90 days; access logged and audited.  
- Logs: error logs retained for 90 days; debug logs retained only when explicitly enabled and for a short window (7 days).

---

Testing and Validation
- Instrumentation tests: unit tests that assert metrics are emitted with correct labels and bounded cardinality.  
- Load tests: validate metric volume, sampling, and aggregation under production‑like load.  
- Adversarial tests: ensure adversarial inputs do not cause telemetry leakage or cardinality explosions.  
- Regression checks: CI gates that fail on new metric label keys or unapproved label values.

---

Governance and Change Control
- All metric names, label keys, and allowed label values must be documented in metrics-values.md.  
- Adding new metrics or label values requires review and approval by the Observability and Security owners.  
- Changes to retention, sampling, or export policies require documented justification and an effective date.

---

Example Telemetry Event (anonymized)
```
json
{
  "service": "sanitizer",
  "channel": "emergency",
  "format": "image",
  "stage": "reencode",
  "result": "ok",
  "duration_seconds": 2.34,
  "timestamp": "2026-04-14T05:21:00Z",
  "anonreasonbucket": "none",
  "toolchain_versions": { "encoder": "libjpeg-2.1.0", "muxer": "ffmpeg-5.1" }
}
```

---

Runbooks and Playbooks
- Maintain runbooks for common alerts (high rejection rate, encoder failures, adversarial spikes).  
- Include steps for triage, mitigation (rate limiting, rollback), and post‑incident analysis.  
- Ensure runbooks reference the dashboards and logs described above.

---

Appendix: Metric Naming Conventions
- Use sanitizer<noun><unit> for counters/gauges (e.g., sanitizerrejectiontotal).  
- Use sanitizer<operation>duration_seconds for histograms.  
- Document all names and semantics in metrics-values.md.

---
