# VPN Access Layer

The VPN access layer provides the transport, camouflage, session initialization, fallback logic, and security mechanisms required to establish censorship-resistant connections in hostile network environments.  
It is designed to operate under state-level adversaries capable of deep packet inspection (DPI), active probing, protocol fingerprinting, and traffic analysis.

This module defines how clients connect, negotiate transports, evade detection, and maintain reliable communication across diverse regions and platforms.

---

## Objectives

The VPN access layer provides:

- A unified transport abstraction (TLS, QUIC, HTTP/2, CDN)
- Camouflage mechanisms that mimic real-world traffic
- Secure session initialization and key exchange
- Automatic fallback across protocols and regions
- Platform-specific client profiles
- DPI evasion and active probing resistance
- Region-aware behavior and error normalization

These components work together to ensure stealth, reliability, and censorship resistance.

---

## Architecture Overview

The access layer is composed of six major subsystems:

### 1. Entrypoints (`entrypoints/`)
    Defines protocol-specific entrypoints for TLS, QUIC, HTTP, and CDN-based transports.
    Provides transport-level behavior, ALPN negotiation, and protocol mimicry.

### 2. Camouflage (`camouflage/`)
    Implements TLS/QUIC fingerprint mimicry, SNI strategies, certificate behavior,
    handshake obfuscation, and real-site mimicry.

### 3. Session Initialization (`session-init/`)
    Handles key exchange, transport negotiation, bootstrap flow, and replay protection.
    Ensures secure and indistinguishable session establishment.

### 4. Fallback (`fallback/`)
    Defines region-aware fallback chains, error normalization, and protocol downgrade logic.
    Ensures connectivity under protocol blocking or network interference.

### 5. Client Profiles (`client-profiles/`)
    Provides platform-specific behavior for Windows, macOS, Linux, Android, iOS, and embedded devices.
    Ensures fingerprints, timing, and fallback behavior match real-world platform characteristics.

### 6. Security (`security/`)
    Defines the threat model, DPI evasion strategies, and active probing defenses.
    Ensures resilience against state-level adversaries.

---

## Directory Structure

The module is organized as follows:

    vpn-access-layer/
    ├── entrypoints/          # TLS/QUIC/HTTP/CDN entrypoints
    ├── camouflage/           # TLS fingerprints, SNI, handshake obfuscation
    ├── session-init/         # Key exchange, negotiation, bootstrap flow
    ├── fallback/             # Fallback chains, region profiles, error normalization
    ├── client-profiles/      # Platform-specific profiles
    └── security/             # Threat model, DPI evasion, probing defense

Each directory contains a README.md describing its purpose and submodules.

---

## Design Principles

The access layer follows these principles:

### Indistinguishability
    All traffic must resemble legitimate TLS/QUIC/HTTP/CDN traffic.

### Defense in Depth
    Multiple layers of camouflage, fallback, and security mechanisms.

### Region Awareness
    Behavior adapts to local censorship conditions.

### Platform Fidelity
    Client behavior matches real-world platform fingerprints and timing.

### Minimal Attack Surface
    No custom protocol identifiers or distinguishable metadata.

### Active Probing Resistance
    Endpoints remain silent or mimic legitimate services under probing.

---

## Integration with the Full System

The VPN access layer integrates with:

- Transport layer implementations
- Control plane and session management
- Distributed entrypoint infrastructure
- Monitoring and telemetry (optional)
- Region configuration services

It acts as the foundation for censorship-resistant connectivity.

---

## Summary

The VPN access layer defines the transport, camouflage, session initialization, fallback, and security mechanisms required to operate in adversarial network environments.  
By combining realistic protocol mimicry, secure session establishment, region-aware fallback, and strong DPI/active probing resistance, it provides a robust foundation for censorship-resistant communication.
