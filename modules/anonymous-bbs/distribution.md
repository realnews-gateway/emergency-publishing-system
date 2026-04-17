
# Distribution Layer: Interface to Global Delivery

The distribution layer in **anonymous-bbs** does not implement its own delivery logic.  
Instead, it serves as the interface that connects BBS content to the **global distribution module** in the root directory.  
Its role is to ensure that messages created in BBS are properly handed off to the system‑wide distribution pipeline, which handles region‑aware delivery, censorship resistance, and bandwidth adaptation.

---

## Objectives

The distribution layer in BBS provides:

- A clear interface for handing off BBS content to root **distribution**  
- Ensures pseudonymous messages are delivered without exposing account or network metadata  
- Guarantees that only emergency‑relevant content is passed downstream (spam, adult material, and non‑emergency posts are excluded)  
- Supports low‑bandwidth and unstable network conditions by leveraging root distribution formats  

---

## Content Flow

- **Publishing Path**:  
  Client → accounts → BBS (input content) → emergency-channel (cleaning & stripping) → network-access-layer (transport) → server → storage → **root distribution**  

- **Reading Path**:  
  Client request → network-access-layer → server → network-access-layer → client (temporary cache handled by emergency-channel)  

---

## Responsibilities

Within BBS, the distribution layer is limited to:

- Passing normalized, pseudonymous messages from **storage** to root **distribution**  
- Ensuring that inappropriate or non‑emergency content is filtered before handoff  
- Maintaining consistency with the message format defined in `message-format.md`  
- Avoiding duplication of routing or transport logic (handled globally)  

---

## Integration with Root Modules

The anonymous BBS distribution layer **interfaces with**:

- **storage/** — Provides append‑only logs of BBS content  
- **distribution/** — Global delivery system for region‑aware, censorship‑resistant routing  
- **emergency-channel/** — Ensures metadata cleaning and temporary cache for clients  
- **network-access-layer/** — Handles transport between server and client  

---

## Summary

The distribution layer in anonymous‑bbs is an **interface**, not a standalone delivery system.  
It ensures that BBS content is properly filtered, normalized, and handed off to the global **distribution** module, which performs the actual region‑aware, censorship‑resistant delivery.  
This design avoids duplication and keeps BBS focused on its core function: secure user input and pseudonymous communication.
