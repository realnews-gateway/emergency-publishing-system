# DPI Evasion

This document describes the techniques used to evade Deep Packet Inspection (DPI) systems deployed by state-level censorship infrastructures.  
DPI systems analyze packet contents, metadata, timing, and protocol behavior to identify and block VPN traffic.  
The VPN access layer is designed to remain indistinguishable from legitimate TLS, QUIC, HTTP/2, and CDN traffic under both passive and active inspection.

DPI evasion is achieved through camouflage, handshake obfuscation, error normalization, and transport-level mimicry.

---

## DPI Capabilities Considered

The system assumes that DPI systems may perform:

### 1. Protocol Fingerprinting
    - TLS ClientHello fingerprinting
    - QUIC Initial packet fingerprinting
    - HTTP/2 frame sequence analysis
    - CDN domain-fronting detection

### 2. Metadata Inspection
    - SNI extraction
    - ALPN inspection
    - Certificate field analysis
    - Packet size and timing analysis

### 3. Statistical Traffic Analysis
    - Flow classification using ML models
    - Burstiness and idle period detection
    - Packet size histogram comparison

### 4. Active Manipulation
    - Injecting malformed packets
    - Triggering protocol errors
    - Forcing fallback transitions

### 5. Selective Blocking
    - Blocking QUIC Initial packets
    - Blocking TLS fingerprints
    - Blocking specific SNI values
    - Blocking domain-fronting patterns

The system must remain functional and stealthy under all of these conditions.

---

## Evasion Techniques

The VPN access layer uses multiple layers of DPI evasion:

---

### 1. TLS Fingerprint Camouflage

TLS handshakes are crafted to match real-world fingerprints:

    - Chrome (Windows, macOS, Linux, Android)
    - Safari (iOS, macOS)
    - Firefox (optional)
    - CDN-origin fingerprints

Techniques include:

    - Extension ordering mimicry
    - Supported cipher suite alignment
    - ALPN negotiation matching
    - KeyShare group selection
    - Session ticket behavior mimicry

This prevents TLS fingerprint-based blocking.

---

### 2. QUIC Initial Packet Camouflage

QUIC Initial packets mimic:

    - Chrome QUIC (desktop/mobile)
    - Safari QUIC (iOS/macOS)
    - CDN QUIC deployments

Techniques include:

    - Version negotiation mimicry
    - Packet size padding
    - Retry token behavior
    - Stream frame ordering

This prevents QUIC fingerprint-based blocking.

---

### 3. SNI Randomization and Fronting

The system uses region-aware SNI strategies:

    - Stable SNI (Safari-like)
    - Rotating SNI (Chrome-like)
    - Domain fronting (CDN-based)
    - ECH (Encrypted ClientHello) when available

This prevents SNI-based blocking and classification.

---

### 4. Real-Site Mimicry

The system mimics real websites:

    - Certificates match real-world patterns
    - ALPN matches expected services
    - Traffic patterns resemble legitimate browsing
    - CDN behavior is replicated when applicable

This prevents DPI from distinguishing VPN traffic from normal HTTPS traffic.

---

### 5. Handshake Obfuscation

Session initialization is obfuscated:

    - Key exchange messages embedded in legitimate TLS extensions
    - Transport negotiation hidden inside encrypted channels
    - No custom protocol identifiers
    - No unusual handshake timing

This prevents DPI from detecting custom protocols.

---

### 6. Error Normalization

All errors are normalized to real-world equivalents:

    - TLS alerts (handshake_failure, protocol_version)
    - QUIC close frames (CONNECTION_TIMEOUT)
    - HTTP/2 502/503 responses
    - CDN 504 Gateway Timeout

This prevents DPI from detecting fallback transitions.

---

### 7. Timing and Packet Size Camouflage

Traffic patterns mimic real applications:

    - Browser-like timing jitter
    - Mobile vs desktop timing profiles
    - Packet size padding
    - Flow burstiness matching real TLS/QUIC traffic

This prevents ML-based traffic classification.

---

### 8. Region-Aware Behavior

The system adapts to regional censorship:

    - QUIC-first in permissive regions
    - TLS-first in high-risk regions
    - CDN-fronting in extreme censorship environments
    - SNI rotation frequency adjusted per region

This prevents region-specific DPI heuristics from triggering blocks.

---

## Integration with Other Modules

DPI evasion integrates with:

- **camouflage/**  
  TLS fingerprints, SNI strategies, handshake obfuscation.

- **session-init/**  
  Obfuscated key exchange and transport negotiation.

- **fallback/**  
  Error normalization and region-aware fallback chains.

- **entrypoints/**  
  Protocol-specific DPI evasion behavior.

- **client-profiles/**  
  Platform-specific fingerprint selection.

DPI evasion is not a standalone feature; it is embedded across all layers.

---

## Summary

The DPI evasion subsystem ensures that VPN traffic remains indistinguishable from legitimate TLS, QUIC, HTTP/2, and CDN traffic.  
By combining fingerprint camouflage, handshake obfuscation, error normalization, and region-aware behavior, the system resists state-level DPI systems and maintains strong censorship resistance under hostile network conditions.
