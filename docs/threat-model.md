# Threat Model

This document describes the high‑level threat model for **Empus**.  
It outlines the adversaries the system is designed to resist, the assumptions it makes about hostile environments, and the safety guarantees it aims to provide for users and content.

Empus is built for regions facing strong, adaptive, state‑level censorship and surveillance.

---

## Adversary Capabilities

Empus assumes adversaries may possess the following capabilities:

### Network‑Level Interference
- IP, domain, and SNI blocking  
- Deep packet inspection (DPI)  
- Active probing of suspected endpoints  
- Traffic fingerprinting and protocol classification  
- Throttling or selective degradation  
- Regional infrastructure interference  

### Surveillance and Monitoring
- Metadata collection  
- Traffic correlation and timing analysis  
- Device monitoring on compromised networks  
- Social graph inference  

### Platform‑Level Suppression
- Automated content removal  
- Hash‑based media filtering  
- Keyword or pattern‑based detection  
- Takedown of centralized services  

These capabilities reflect real‑world censorship and surveillance conditions in high‑risk regions.

---

## Out‑of‑Scope Adversary Capabilities

Empus does **not** assume adversaries can:

- Compromise both sender and receiver devices simultaneously  
- Break modern cryptography  
- Deploy hardware implants or baseband‑level exploits  
- Perform physical coercion  
- Exploit zero‑day vulnerabilities across all user devices  

These threats exceed the scope of software‑level protections.

---

## User Risks

Users in high‑risk regions may face:

- Device seizure  
- Account compromise  
- Network monitoring  
- Social graph exposure  
- Intermittent or unstable connectivity  

Empus mitigates these risks through metadata minimization, pseudonymity, encrypted transports, and region‑aware routing.

---

## System‑Level Mitigations

Empus incorporates multiple defensive strategies:

### Metadata Minimization
- No IP addresses stored  
- No device identifiers retained  
- No persistent routing identifiers  
- Coarse‑grained timestamps  
- Sanitized ingestion pipeline  

### Transport Security
- Multi‑layer transport stack  
- REALITY, uTLS, XTLS‑Vision, XHTTP, VLESS, TUIC v5  
- Encrypted payloads  
- Behavioral camouflage  
- Region‑aware fallback chains  

### Routing and Distribution
- Multi‑hop routing  
- Randomized path selection  
- Redundant delivery attempts  
- Delay‑tolerant synchronization  
- Region‑aware retention policies  

### Pseudonymity
- System‑generated pseudonyms  
- No persistent identifiers  
- No linkability across sessions  
- Sanitized submissions  

These mitigations ensure that Empus remains operational even under severe censorship pressure.

---

## Threat Model Summary

Empus is designed for adversarial environments where censorship, surveillance, and network disruption are routine.  
The system assumes strong state‑level adversaries but relies on strict data minimization, encrypted transports, pseudonymity, and region‑aware routing to maintain availability and protect users.

This threat model provides the foundation for Empus’s architecture and security design.
