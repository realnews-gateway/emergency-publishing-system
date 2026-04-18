
# Storage Health Checks

The health checks file defines how the storage layer monitors availability, latency, and reliability across backends.  
It ensures that replicas remain healthy, routing decisions are informed, and failures are detected early for recovery.

---

## Objectives

- Continuously monitor storage backends for availability and performance  
- Detect failures, latency spikes, and replica loss  
- Enforce integrity validation during health checks  
- Provide logs and alerts for auditing and troubleshooting  

---

## Probe Types

### 1. Liveness Probe
- Confirms backend is running and responsive  
- Detects crashes or unresponsive nodes  

### 2. Readiness Probe
- Confirms backend is ready to accept new content  
- Ensures synchronization is complete before routing  

### 3. Backlog Probe
- Reports queue depth and pending replication tasks  
- Detects overload conditions  

### 4. Operational Probe
- Tracks recent success/failure counts for storage operations  
- Provides reliability metrics  

### 5. Integrity Probe
- Validates signatures and hash chains during storage operations  
- Ensures replicas remain authentic and uncorrupted  

---

## Probe Payload Format

```
{
  "backend_id": "string",
  "timestamp": "ISO8601 string",
  "probe_type": "liveness | readiness | backlog | operational | integrity",
  "status": "healthy | degraded | unhealthy",
  "metrics": { "key": "value" },
  "message": "optional human readable note"
}
```

---

## Thresholds and Debounce Rules

- **probe_interval_s**: default 10 seconds  
- **failure_threshold**: 3 consecutive failing probes → mark unhealthy  
- **recovery_threshold**: 2 consecutive healthy probes → mark recovered  
- **history_window_s**: 300 seconds of rolling probe history  

Soft limits trigger warnings but do not exclude the backend.  
Hard limits trigger immediate exclusion from routing.  

---

## Monitoring and Audit

- Logs record probe results and backend health status  
- Alerts triggered on repeated failures or degraded performance  
- Audit trails maintained for compliance and recovery analysis  

---

## Integration

- **router.md** — Uses health check results to select optimal backend  
- **redundancy.md** — Ensures replicas are healthy and balanced  
- **schema.md** — Validates integrity during health checks  
- **overview.md** — Provides context for monitored storage models  

---

## Summary

Storage health checks provide continuous visibility into backend availability, readiness, backlog, operational reliability, and integrity.  
By enforcing thresholds, logging results, and integrating with routing and redundancy, the system ensures durable, fault‑tolerant, and censorship‑resistant persistence.
