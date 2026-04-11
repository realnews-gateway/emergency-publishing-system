# Operational Layers

This document defines the three operational layers of the Empus Emergency
Publishing System. Each layer is an intentional, auditable combination of
the six source protocols. The design emphasizes clarity, reproducibility,
and defensibility under adversarial conditions.

Empus does not invent new protocols.  
It composes existing, widely deployed protocols into transport profiles
that behave predictably across censorship environments.

---

## Design Principles

The three‑layer model is built on three architectural principles:

### **1. Explicit Threat Models**
Each layer corresponds to a specific adversarial capability:
- statistical DPI  
- TLS/QUIC fingerprinting  
- active probing  
- targeted disruption  
- collateral‑damage‑sensitive environments  

The system does not guess; it responds to defined threat levels.

### **2. Minimal but Sufficient Composition**
Every layer uses the smallest protocol set required to achieve its goal.
This avoids unnecessary complexity and ensures each configuration can be
audited, reasoned about, and reproduced.

### **3. Graceful Degradation**
As censorship intensifies, Empus transitions from:
- **performance → resilience → survivability**  
without collapsing or requiring reconfiguration of the entire system.

---

## Layer 1 — Performance Layer (TCP)

**Objective:**  
Maximize throughput and maintain strong camouflage when TLS traffic is
still broadly permitted.

### Protocol Combinations

- **VLESS + REALITY + uTLS + XTLS‑Vision**  
- **VLESS + REALITY + uTLS + XHTTP (Stream)**

### Architectural Rationale

- **VLESS** provides a clean, metadata‑minimal carrier.  
- **REALITY** aligns the certificate surface with legitimate HTTPS
  endpoints.  
- **uTLS** reproduces Chrome/Firefox fingerprints, blending into the
  dominant TLS ecosystem.  
- **XTLS‑Vision** obscures statistical features (packet size, timing,
  flow shape).  
- **XHTTP Stream** emulates HTTP/3‑style multiplexed behavior.

This layer assumes:

> TLS remains viable, DPI is heuristic or statistical, and long‑lived
> flows are not aggressively targeted.

### Strengths

- High throughput for continuous synchronization  
- Strong camouflage at both TLS and behavioral layers  
- Stable long‑duration connections  
- Suitable for CDN‑adjacent deployments  

### When to Use

- Moderate censorship  
- Environments with abundant legitimate HTTPS traffic  
- Operators prioritizing performance and stability  

---

## Layer 2 — High‑Performance UDP Layer

**Objective:**  
Deliver low latency, rapid recovery, and mobile‑friendly behavior when
UDP is available.

### Protocol Combination

- **TUIC v5**

### Architectural Rationale

This layer prioritizes transport efficiency over camouflage. TUIC v5
provides:

- low latency  
- path migration  
- mobile network resilience  
- high throughput under unstable conditions  

When UDP is not blocked, TUIC v5 outperforms any TCP‑based configuration
in responsiveness and recovery.

### Strengths

- Excellent performance on mobile networks  
- Fast failover during network transitions  
- Efficient for bursty or real‑time workloads  

### When to Use

- Regions where UDP is not systematically filtered  
- Mobile‑dominant user bases  
- Scenarios requiring rapid delivery or control signaling  

---

## Layer 3 — Emergency Layer (Extreme Censorship)

**Objective:**  
Maintain a narrow but highly resilient communication channel under
state‑level or targeted censorship.

### Protocol Combinations

- **VLESS + XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise**  
- **TUIC v5 + Cloudflare Spectrum**

### Architectural Rationale (TCP Path)

- **VLESS** minimizes metadata exposure.  
- **XHTTP Packet** shifts from long‑lived flows to discrete,
  HTTP/3‑like packet exchanges, reducing flow‑based detectability.  
- **TLS 1.3 + ECH** hides SNI and handshake metadata inside encrypted
  envelopes.  
- **Cloudflare Enterprise** provides a camouflage surface where blocking
  Empus traffic implies collateral damage to high‑value web services.

This configuration forces adversaries into an unfavorable cost model:

> To block Empus, they must disrupt legitimate enterprise‑grade HTTPS
> traffic.

### Architectural Rationale (UDP Path)

- **TUIC v5 + Cloudflare Spectrum** places high‑performance UDP transport
  behind Cloudflare’s infrastructure, raising the blocking cost to the
  level of CDN disruption.

### Strengths

- Extremely high adversarial cost  
- Strong indistinguishability from high‑value HTTPS/HTTP3 traffic  
- Survival under active probing, throttling, and protocol bans  

### When to Use

- Targeted or state‑level censorship  
- Large‑scale TLS/QUIC fingerprint blocking  
- Partial or near‑total network blackouts  

---

## Why Three Layers Instead of One “Universal Mode”?

A single “universal mode” would be:

- too slow in permissive environments  
- too fragile in hostile environments  
- too complex to audit  
- too unpredictable across networks  

The three‑layer model provides:

### **Clear Mental Models**
Operators can reason about system behavior:
“we are in Layer 1 / Layer 2 / Layer 3.”

### **Auditable Compositions**
Each layer uses a small, fixed set of protocol stacks that can be
reviewed and validated.

### **Predictable Transitions**
As censorship intensifies, Empus shifts layers instead of collapsing.

### **Explicit Trade‑offs**
- Layer 1: maximum performance  
- Layer 2: maximum responsiveness  
- Layer 3: maximum survivability  

This structure is intentional, minimal, and defensible.

---

## Summary

The three operational layers form the backbone of the Empus transport
architecture:

- **Layer 1 — Performance Layer (TCP)**  
  High throughput, strong camouflage, stable connections.

- **Layer 2 — High‑Performance UDP Layer**  
  Low latency, mobile resilience, rapid recovery.

- **Layer 3 — Emergency Layer (Extreme Censorship)**  
  CDN‑anchored survivability under the harshest conditions.

Together, these layers provide a coherent, explainable, and
professionally defensible strategy for operating in diverse censorship
environments.
