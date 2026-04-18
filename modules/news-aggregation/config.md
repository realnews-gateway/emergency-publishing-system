
# Configuration

The configuration file defines global parameters for the aggregation pipeline.  
It provides lightweight settings that guide fetcher, parser, deduplication, and classification modules.  
Unlike **sources.md** (which defines source attributes) and **region-config.md** (which defines region blocking rules), this file specifies **system-wide operational parameters**.

---

## Objectives

The configuration provides:

- Unified control of fetch frequency and retry policies  
- Deduplication strategy settings  
- Classification rules and thresholds  
- Source enable/disable switches  
- Integration points for telemetry and monitoring  

---

## Example Configuration

```
fetch:
  frequency: 5m
  retry_backoff: [1s, 2s, 4s, 8s]
  jitter: true

deduplication:
  strategy: "similarity-hash"
  threshold: 0.85
  preserve_copyright: true

classification:
  ruleset: "default"
  language_detection: true
  topic_threshold: 0.7

sources:
  enable_all: true
  disable_list:
    - "test_source"
    - "deprecated_feed"

telemetry:
  metrics_enabled: true
  log_level: "info"
```

---

## Integration with Other Modules

- **fetcher.md** — Reads fetch frequency and retry policies  
- **parser.md** — Uses classification and language detection settings  
- **deduplication.md** — Applies deduplication strategy and thresholds  
- **classification.md** — Uses ruleset and topic thresholds  
- **sources.md** — Controlled by enable/disable switches  
- **region-config.md** — Works alongside config.md to enforce region blocking  

---

## Summary

The configuration file defines lightweight, system-wide parameters for the aggregation pipeline.  
By centralizing fetch, deduplication, classification, and telemetry settings, it ensures consistent behavior across modules while avoiding duplication of source definitions or region rules.
