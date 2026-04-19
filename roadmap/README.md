# Roadmap Overview

This directory provides a high-level overview of the project roadmap.  
Detailed objectives, milestones, and timelines are documented in the corresponding files  
(`milestones.md`, `timeline.md`, `future-work.md`).

---

## Phase Breakdown

### Phase 1 — Architecture and Documentation
- **Completed**: System architecture design and documentation.
- Scope: Provide full design specifications, no code development.

### Phase 2 — Code Development  
This phase is divided into **eight sub‑phases**, each including testing, security review, and documentation updates:

- **Sub‑phase 1: Standards** → Define unified coding conventions and CI rules.  
- **Sub‑phase 2: Storage (Local)** → Deploy first main server, establish minimal persistence.  
- **Sub‑phase 3: Accounts** → Implement identity layer (registration, authentication, isolation).  
- **Sub‑phase 4: Network‑Access‑Layer (NAL)** → Core transport foundation, client-server linkage.  
- **Sub‑phase 5: Modules (news‑aggregation / anonymous‑bbs)** → Input channels, ensure real data sources.  
- **Sub‑phase 6: Emergency Channel** → Core output, subject to strict testing and review.  
- **Sub‑phase 7: Storage (Extended)** → Server mirroring, IPFS, Arweave integration.  
- **Sub‑phase 8: Distribution** → Content delivery, mirroring, fallback mechanisms.

### Phase 3 — Final Testing
- Cross‑module integration tests.  
- Performance and resilience validation.  
- Internal security checks.

### Phase 4 — Final Review
- Consolidated internal review.  
- Third‑party security audit.  
- Documentation and compliance verification.

### Phase 5 — Release
- Build release packages.  
- Deployment guide.  
- Reviewer-facing package.

---

## File Structure

- **README.md** — Overview and phase outline (this file).  
- **milestones.md** — Stage goals and progress tracking.  
- **timeline.md** — Development schedule and sequencing.  
- **future-work.md** — Long-term extensions and research directions.
