# Camouflage Layer

The Camouflage Layer provides all mechanisms that make VPN access indistinguishable from legitimate traffic.  
It is responsible for TLS fingerprint shaping, real‑site mimicry, SNI randomization, handshake obfuscation, and traffic pattern normalization.

This subsystem is essential for bypassing DPI, active probing, and traffic classification systems.

---

## Purpose

The Camouflage Layer enables:

- Real‑site mimicry using genuine TLS certificates  
- TLS fingerprint shaping to match popular browsers  
- SNI randomization and domain rotation  
- Obfuscated handshakes that evade protocol classifiers  
- Traffic pattern normalization to resemble real HTTPS or QUIC  
- Integration with CDN‑backed entrypoints  

It ensures that access to the system cannot be reliably distinguished from normal internet usage.

---

## Directory Structure

This directory includes:

- **real-site-mimicry.md**  
  Techniques for mimicking real websites using legitimate TLS handshakes.

- **tls-fingerprint.md**  
  Methods for matching Chrome/Firefox TLS fingerprints.

- **sni-randomization.md**  
  Strategies for rotating SNI values and avoiding static fingerprints.

- **handshake-obfuscation.md**  
  Obfuscation of TLS/QUIC handshakes to evade DPI classifiers.

- **camouflage.md**  
  High-level specification of camouflage behavior and integration.

---

## Key Features

- **Genuine TLS mimicry**  
  Uses real certificates and domain‑fronted traffic patterns.

- **Browser‑grade fingerprints**  
  Matches TLS ClientHello fingerprints of major browsers.

- **Dynamic SNI rotation**  
  Prevents static blocking and fingerprint accumulation.

- **Obfuscated handshakes**  
  Makes protocol identification significantly harder.

- **Traffic normalization**  
  Ensures packet sizes, timing, and flows resemble real HTTPS/QUIC.

- **CDN compatibility**  
  Works seamlessly with Cloudflare, Fastly, and Akamai entrypoints.

---

## Integration

The Camouflage Layer connects to:

- **entrypoints/**  
  Provides camouflage for TLS/QUIC/HTTP/CDN entrypoints.

- **session-init/**  
  Ensures handshake obfuscation is compatible with key exchange.

- **fallback/**  
  Adjusts camouflage strategies based on regional blocking behavior.

It is the system’s primary defense against DPI and active probing.

---

## Summary

The Camouflage Layer ensures that all access to the system appears indistinguishable from normal internet traffic.  
By combining real‑site mimicry, TLS fingerprint shaping, SNI randomization, and handshake obfuscation, it provides strong resistance against censorship and traffic classification.

This subsystem is essential for operating safely in hostile network environments.
