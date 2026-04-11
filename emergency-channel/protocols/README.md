# Protocols Module

The Protocols module defines the transport foundations of the Empus
Emergency Publishing System. It documents the six underlying transport
protocols used by the system and the three operational layers that
combine them into censorship‑resistant, high‑performance delivery
strategies.

This module does **not** define new protocols.  
Instead, it documents how existing, widely deployed protocols are
combined to achieve resilience under diverse network conditions.

---

## Contents

This directory contains four documents:

- **overview.md**  
  High‑level introduction to the Six‑Protocol Transport Stack and the
  Three Operational Layers.

- **source-protocols.md**  
  Detailed descriptions of the six underlying transport protocols:
  REALITY, uTLS, XTLS‑Vision, XHTTP, VLESS, and TUIC v5.

- **operational-layers.md**  
  Explanation of the three operational layers (Performance, High‑Performance
  UDP, Emergency), including their design goals, advantages, and
  recommended usage conditions.

- **README.md**  
  This file.

---

## Six‑Protocol Transport Stack

Empus uses six existing transport protocols as building blocks:

- **REALITY** — certificate camouflage  
- **uTLS** — Chrome/Firefox fingerprint mimicry  
- **XTLS‑Vision** — dynamic padding and statistical DPI evasion  
- **XHTTP** — HTTP/3‑like behavioral camouflage (Stream & Packet modes)  
- **VLESS** — universal carrier layer  
- **TUIC v5** — high‑performance UDP transport  

These protocols are not used in isolation.  
They are combined into layered operational modes optimized for different
censorship and performance environments.

---

## Three Operational Layers

Empus organizes protocol usage into three layers:

### **1. Performance Layer (TCP)**  
Optimized for high throughput and stable networks:

- VLESS + REALITY + uTLS + XTLS‑Vision  
- VLESS + REALITY + uTLS + XHTTP (Stream)

### **2. High‑Performance UDP Layer**  
Optimized for low latency and mobile switching:

- TUIC v5

### **3. Emergency Layer (Extreme Censorship)**  
Designed for severe blocking and active interference:

- VLESS + XHTTP Packet + TLS 1.3 + ECH + Cloudflare Enterprise  
- TUIC v5 + Cloudflare Spectrum

Each layer is documented in detail in `operational-layers.md`.

---

## Design Principles

The protocol system follows three principles:

- **Camouflage**  
  Blend into high‑value, high‑volume traffic (TLS 1.3, HTTP/3, CDN flows).

- **Resilience**  
  Maintain connectivity under throttling, probing, and partial blackouts.

- **Modularity**  
  Combine existing protocols into layered strategies without modifying
  their underlying specifications.

---

## Summary

The Protocols module provides:

- A unified view of the six transport protocols used by Empus  
- A clear explanation of how these protocols are combined  
- A structured three‑layer model for adapting to censorship conditions  
- A clean, modern, reviewer‑friendly documentation layout  

All protocol details and operational strategies are defined in the
accompanying documents.
