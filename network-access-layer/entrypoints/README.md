# Entrypoints

The Entrypoints subsystem defines all inbound access paths into the VPN Access Layer.  
Each entrypoint represents a different transport mechanism—TLS, QUIC, HTTP, or CDN-backed—designed to bypass censorship and blend into legitimate traffic patterns.

Entrypoints are the first component a user interacts with when connecting to the system.

---

## Purpose

Entrypoints provide:

- Multiple covert access paths  
- Protocol diversity to evade blocking  
- CDN-backed entry for high stealth  
- TLS/QUIC/HTTP transports that mimic real services  
- Region-specific access strategies  
- Integration with camouflage and session-init layers  

They ensure users can always reach the system, even under aggressive censorship.

---

## Directory Structure

This directory includes:

- **tls/**  
  TLS-based entrypoints (Trojan, XTLS, Reality-style TLS).

- **quic/**  
  QUIC-based entrypoints (Hysteria2, QUIC obfuscation).

- **http/**  
  HTTP-based entrypoints (XHTTP, domain-fronted HTTP).

- **cdn/**  
  CDN-backed entrypoints using Cloudflare, Fastly, or Akamai.

- **entrypoints.md**  
  High-level specification of entrypoint behavior, requirements, and integration.

---

## Key Features

- **Stealth transports**  
  Entrypoints mimic real HTTPS, HTTP/2, or QUIC traffic.

- **CDN integration**  
  Allows access through globally distributed networks.

- **Protocol fallback**  
  Automatically switches between TLS, QUIC, and HTTP.

- **Region-aware behavior**  
  Different entrypoints can be enabled per region.

- **Modular design**  
  Each entrypoint type is independently extensible.

---

## Integration

Entrypoints connect directly to:

- **camouflage/**  
  For TLS fingerprinting, SNI randomization, and real-site mimicry.

- **session-init/**  
  For key exchange and transport negotiation.

- **fallback/**  
  For automatic switching when an entrypoint is blocked.

Entrypoints are the outermost layer of the VPN Access Layer.

---

## Summary

The Entrypoints subsystem provides the system’s first line of defense against censorship.  
By offering multiple covert access paths—TLS, QUIC, HTTP, and CDN-backed—it ensures users can always establish a connection, even in highly restricted environments.
