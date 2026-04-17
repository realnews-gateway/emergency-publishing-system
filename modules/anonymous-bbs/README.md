
# Anonymous BBS Module

The anonymous BBS module provides a censorship‑resistant, privacy‑preserving message board system designed for high‑risk environments.  
Its core function is to give users a secure interface to input and publish content, while ensuring that all public identity remains pseudonymous.  
This module is strictly intended for emergency and critical communications — it does **not** serve as a platform for adult content or other non‑emergency material.

---

## Objectives

The anonymous BBS module provides:

- Account‑based access with pseudonymous public identity (via root **accounts**)  
- Secure input and posting of user content  
- No exposure of real user identifiers in public views  
- Metadata cleaning delegated to **emergency-channel**  
- Transport delegated to **network-access-layer**  
- Storage and distribution delegated to root modules  
- Optional moderation and filtering of inappropriate or irrelevant content (e.g., spam, adult material, non‑emergency posts)  
- Compatibility with low‑bandwidth or unstable networks  

---

## Architecture Overview

The BBS system focuses on user input and message flow, directly connected to root modules:

### 1. Identity Integration
- Relies on **accounts** for registration, authentication, and rate limiting  
- Public identity is pseudonymous only (system‑generated names + avatars)  

### 2. Message Gateway
- Receives posts from authenticated accounts  
- Delegates metadata cleaning and stripping to **emergency-channel**  
- Attaches pseudonymous identity for display  
- Applies lightweight filters against spam, adult content, and non‑emergency material  

### 3. Content Flow
- **Publishing Path**:  
  Client → accounts → BBS (input content) → emergency-channel (cleaning & stripping) → network-access-layer (transport) → server → storage → distribution  

- **Reading Path**:  
  Client request → network-access-layer → server → network-access-layer → client (temporary cache handled by emergency-channel)  

---

## Directory Structure

```  
anonymous-bbs/  
├── README.md              # Overview and architecture  
├── message-format.md      # Message structure, pseudonymous identity, metadata stripping (via emergency-channel)  
├── gateway.md             # Message ingestion, filtering, and pseudonym attachment  
├── storage.md             # Interface to root storage (no duplication)  
└── distribution.md        # Interface to root distribution (no duplication)  
```  

---

## Design Principles

- **User Input as Core Function**  
  The BBS exists to provide a safe interface for users to write and publish emergency content.  

- **Delegated Metadata Cleaning**  
  All metadata stripping and normalization are handled by **emergency-channel**.  

- **Minimal Metadata**  
  Only message body, pseudonymous identity, and coarse timestamps are exposed.  

- **Censorship Resistance**  
  Relies on **network-access-layer** for transport and **distribution** for region‑aware delivery.  

- **Low Bandwidth Operation**  
  Compact message formats, optional compressed feeds, offline posting support.  

- **Safety and Abuse Prevention**  
  Filters block spam, adult material, and non‑emergency content to preserve the integrity of Empus.  

---

## Integration with Root Modules

The anonymous BBS **interfaces with** the following modules:

- **accounts/** — Registration, authentication, and rate limiting  
- **emergency-channel/** — Metadata cleaning, stripping, and temporary client cache  
- **storage/** — Append‑only logs and replication  
- **distribution/** — Region‑aware delivery  
- **network-access-layer/** — Covert transport between client and server  

---

## Summary

The anonymous BBS module provides a secure, censorship‑resistant message board system designed for hostile environments.  
By focusing on user input and delegating account management, metadata cleaning, storage, distribution, and transport to root modules, it ensures safe communication for emergency content while explicitly excluding adult or non‑emergency material.
