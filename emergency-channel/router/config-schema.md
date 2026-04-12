# Router — Configuration Schema

## Overview

This document defines the **Router configuration schema**: the canonical
set of configuration fields, types, defaults, validation rules, and
deployment guidance used by the Emergency Channel Router.

The schema is intended to be both **machine‑validatable** (JSON Schema)
and **human‑readable**. It documents the configuration contract for
deployments, CI, and operators.

---

## Design Principles

- **Deterministic defaults**: sensible defaults that produce reproducible
  behavior across environments.  
- **Safe‑by‑default**: conservative settings that avoid aggressive routing
  or risky fail‑open behavior.  
- **Separation of concerns**: policy, scoring, timeouts, health, telemetry
  and limits are distinct sections.  
- **Explicit validation**: schema validation at CI and service startup to
  prevent silent misconfiguration.  
- **Versioned changes**: any schema change must be versioned and
  documented in release notes.

---

## Top‑Level Sections

- **service**: runtime metadata and logging.  
- **policy**: high‑level routing policies and priorities.  
- **scoring**: weights, penalties, and thresholds for content‑node scoring.  
- **timeouts**: decision time budget and retry/backoff settings.  
- **health**: probe and debounce configuration.  
- **telemetry**: metrics and logging options.  
- **limits**: operational hard limits (queue depth, concurrency).  
- **feature_flags**: opt‑in experimental behaviors.

---

## JSON Schema (deployable)

Use this JSON Schema to validate configuration files in CI and at
service startup.

    {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "title": "EmergencyChannelRouterConfig",
      "type": "object",
      "required": ["service","policy","scoring","timeouts","health","telemetry","limits"],
      "properties": {
        "service": {
          "type": "object",
          "properties": {
            "name": {"type":"string"},
            "environment": {"type":"string","enum":["production","staging","testing","development"]},
            "instance_id": {"type":"string"},
            "log_level": {"type":"string","enum":["error","warn","info","debug"], "default":"info"}
          },
          "required":["name","environment"],
          "additionalProperties": false
        },
        "policy": {
          "type":"object",
          "properties": {
            "default_priority": {"type":"string","enum":["high","normal","low"], "default":"normal"},
            "mandatory_storage_classes": {
              "type":"array",
              "items":{"type":"string"},
              "default":[]
            },
            "fast_path_enabled": {"type":"boolean","default":true},
            "require_ack_before_commit": {"type":"boolean","default":false}
          },
          "additionalProperties": false
        },
        "scoring": {
          "type":"object",
          "properties": {
            "weights": {
              "type":"object",
              "properties": {
                "availability": {"type":"number","minimum":0,"maximum":1,"default":0.6},
                "backlog": {"type":"number","minimum":0,"maximum":1,"default":0.3},
                "capability": {"type":"number","minimum":0,"maximum":1,"default":0.1}
              },
              "required":["availability","backlog","capability"],
              "additionalProperties": false
            },
            "penalties": {
              "type":"object",
              "properties": {
                "recent_failures": {"type":"number","minimum":0,"default":200},
                "overload": {"type":"number","minimum":0,"default":1000}
              },
              "additionalProperties": false
            },
            "score_thresholds": {
              "type":"object",
              "properties": {
                "exclude_below": {"type":"number","default":0},
                "prefer_above": {"type":"number","default":500}
              },
              "additionalProperties": false
            }
          },
          "additionalProperties": false
        },
        "timeouts": {
          "type":"object",
          "properties": {
            "route_decision_ms": {"type":"integer","minimum":10,"default":200},
            "retry_backoff_ms": {"type":"integer","minimum":10,"default":500},
            "max_retries": {"type":"integer","minimum":0,"default":3},
            "decision_commit_timeout_ms": {"type":"integer","minimum":10,"default":1000}
          },
          "additionalProperties": false
        },
        "health": {
          "type":"object",
          "properties": {
            "probe_interval_s": {"type":"integer","minimum":1,"default":10},
            "failure_threshold": {"type":"integer","minimum":1,"default":3},
            "recovery_threshold": {"type":"integer","minimum":1,"default":2},
            "history_window_s": {"type":"integer","minimum":30,"default":300}
          },
          "additionalProperties": false
        },
        "telemetry": {
          "type":"object",
          "properties": {
            "metrics_enabled": {"type":"boolean","default":true},
            "metrics_namespace": {"type":"string","default":"emergency_router"},
            "trace_sampling_rate": {"type":"number","minimum":0,"maximum":1,"default":0.01}
          },
          "additionalProperties": false
        },
        "limits": {
          "type":"object",
          "properties": {
            "max_queue_depth": {"type":"integer","minimum":1,"default":10000},
            "max_concurrent_routes": {"type":"integer","minimum":1,"default":200},
            "max_route_candidates": {"type":"integer","minimum":1,"default":10}
          },
          "additionalProperties": false
        },
        "feature_flags": {
          "type":"object",
          "properties": {
            "enable_experimental_scoring": {"type":"boolean","default":false},
            "enable_deterministic_tiebreaker": {"type":"boolean","default":true}
          },
          "additionalProperties": false
        }
      },
      "additionalProperties": false
    }

---

## Example Configuration (YAML)

    service:
      name: emergency-router
      environment: production
      instance_id: router-01
      log_level: info

    policy:
      default_priority: normal
      mandatory_storage_classes: ["archive"]
      fast_path_enabled: true
      require_ack_before_commit: false

    scoring:
      weights:
        availability: 0.6
        backlog: 0.3
        capability: 0.1
      penalties:
        recent_failures: 200
        overload: 1000
      score_thresholds:
        exclude_below: 0
        prefer_above: 500

    timeouts:
      route_decision_ms: 150
      retry_backoff_ms: 500
      max_retries: 3
      decision_commit_timeout_ms: 1000

    health:
      probe_interval_s: 10
      failure_threshold: 3
      recovery_threshold: 2
      history_window_s: 300

    telemetry:
      metrics_enabled: true
      metrics_namespace: emergency_router
      trace_sampling_rate: 0.01

    limits:
      max_queue_depth: 5000
      max_concurrent_routes: 100
      max_route_candidates: 8

    feature_flags:
      enable_experimental_scoring: false
      enable_deterministic_tiebreaker: true

---

## Validation and CI Guidance

- **Validate** configuration files against the JSON Schema in CI.  
- **Fail builds** when required fields are missing or unknown fields are present
  (`additionalProperties: false` enforces strictness).  
- **Canary rollouts**: changes to scoring weights or penalties must be
  deployed via canary and monitored for failover spikes.  
- **Immutable fields**: `service.name` and `environment` should be
  immutable for a running instance; changing them requires a redeploy.

---

## Operational Notes

- **Conservative timeouts**: keep `route_decision_ms` small enough to
  avoid blocking upstream pipelines but large enough to allow scoring and
  health aggregation.  
- **Penalties**: set `recent_failures` and `overload` penalties high
  enough to exclude unstable nodes but not so high that transient
  recovery is impossible.  
- **Limits**: tune `max_queue_depth` and `max_concurrent_routes` to the
  deployment's capacity; prefer backpressure over silent drops.  
- **Feature flags**: gate experimental scoring behind flags and disable
  by default in production.

---

## Versioning and Change Log

- **Schema versioning**: when changing the schema, increment the router
  config schema version in release notes and provide migration steps.  
- **Backward compatibility**: prefer additive changes; when removing or
  renaming fields, provide a migration path and deprecation window.

---

## Troubleshooting

- **Startup validation failures**: check for unknown fields or missing
  required sections. Use `jsonschema` tooling to validate locally.  
- **Unexpected routing behavior**: verify scoring weights and penalties,
  and confirm canary rollout status.  
- **Health‑related exclusions**: inspect probe history and `history_window_s`
  to ensure debounce behavior is understood.

---

## Quick Operational Checks

- Validate JSON/YAML against the schema before deployment.  
- Run a dry‑run with `feature_flags.enable_experimental_scoring: true`
  in staging only.  
- Monitor `emergency_router_failover_events_total` after any config
  change.
