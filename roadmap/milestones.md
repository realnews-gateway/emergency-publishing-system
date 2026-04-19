# Milestones

This document defines the major milestones for the Empus project, grouped into phases.  
Each milestone represents a concrete deliverable or completion criterion.

---

## Phase 1 — Architecture and Documentation
**Status: Completed**  
All architecture design and documentation milestones have been delivered.

- **Milestone 1: System Architecture**  
  Complete high-level architecture design, including system overview, data flow, protocol integration, security design, and deployment models.  
- **Milestone 2: Repository Structure**  
  Establish repository directories (`architecture/`, `modules/`, `network-access-layer/`, `emergency-channel/`, `accounts/`, `docs/`).  
- **Milestone 3: Module Boundaries**  
  Define clear boundaries for `network-access-layer`, `news-aggregation`, `anonymous-bbs`, and `emergency-channel core`.  
- **Milestone 4: Threat Model**  
  Document security assumptions, risks, and mitigations.

---

## Phase 2 — Code Development  
**Status: Pending Start**  
This phase is divided into eight sub‑phases, each with its own milestone.

- **Sub‑phase 1: Standards**  
  Define coding conventions, CI/CD rules, and documentation practices.  
- **Sub‑phase 2: Storage (Local)**  
  Deploy first main server, implement minimal persistence, and provide storage APIs.  
- **Sub‑phase 3: Accounts**  
  Implement registration, authentication, and isolation logic.  
- **Sub‑phase 4: Network‑Access‑Layer (NAL)**  
  Implement client-server transport, routing, and failover logic.  
- **Sub‑phase 5: Modules (news‑aggregation / anonymous‑bbs)**  
  - News‑aggregation: implement source registry, fetcher, parser, deduplication.  
  - Anonymous‑bbs: implement pseudonymous posting, metadata minimization, storage and distribution.  
- **Sub‑phase 6: Emergency Channel**  
  Implement core output pipeline, sanitization, routing, and operator interface.  
- **Sub‑phase 7: Storage (Extended)**  
  Add distributed storage (server mirroring, IPFS, Arweave).  
- **Sub‑phase 8: Distribution**  
  Implement content delivery, mirroring, fallback mechanisms.

---

## Phase 3 — Final Testing
- **Milestone 1: Integration Testing**  
  Verify cross‑module interactions and end‑to‑end workflows.  
- **Milestone 2: Performance Testing**  
  Measure latency, throughput, and resilience under stress.  
- **Milestone 3: Internal Security Validation**  
  Conduct internal audits of sanitization, routing, and storage.

---

## Phase 4 — Final Review
- **Milestone 1: Internal Review**  
  Consolidate documentation, compliance checks, and operator guides.  
- **Milestone 2: Third‑Party Audit**  
  Commission external security and resilience review.  

---

## Phase 5 — Release
- **Milestone 1: Packaging**  
  Build release packages and installation artifacts.  
- **Milestone 2: Deployment Guide**  
  Provide operator documentation and deployment scenarios.  
- **Milestone 3: Reviewer Package**  
  Deliver summary package for reviewers and funders.

---
