```＊＊＊
# Test Block — Stable Format v2

Event Format:

{
  "event_type": "string",
  "timestamp": 1712345678,
  "source": "core|router|sanitizer|storage|distributor|monitoring",
  "payload": { "key": "value" }
}

Aggregated Metric:

{
  "metric": "transport_viability",
  "window": "1h",
  "avg": 0.92,
  "p95": 0.75,
  "samples": 1842
}
＊＊＊
```
