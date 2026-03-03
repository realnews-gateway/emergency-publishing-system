# Linux Client Profile

The Linux client profile defines platform-specific behavior for transport negotiation, TLS/QUIC fingerprints, fallback logic, and timing normalization.  
Linux environments vary widely, from desktop distributions to server environments, requiring flexible but realistic fingerprinting and transport behavior.

This profile ensures that the system behaves like a native Linux application and blends into legitimate Chrome-on-Linux traffic.

---

## Platform Characteristics

Linux exhibits the following network traits:

- QUIC is widely supported in modern browsers  
- TLS behavior closely matches Chrome on Linux  
- SNI rotation is common  
- Timing patterns match desktop applications  
- Packet sizes follow Linux networking conventions  
- Network conditions vary across distributions and environments  

These characteristics inform transport priority and camouflage selection.

---

## Transport Priority

Linux uses a QUIC-first strategy:

    transport_priority:
      - QUIC
      - TLS
      - HTTP/2
      - CDN

Rationale:

- QUIC is widely supported on Linux  
- QUIC-first improves performance and stealth  
- TLS is a reliable fallback  
- HTTP/2 and CDN are used when necessary  

---

## TLS Fingerprint

Linux uses a Chrome-on-Linux TLS fingerprint:

    tls_fingerprint:
      client: Chrome (Linux)
      version: TLS 1.3
      extensions:
        - supported_versions
        - signature_algorithms
        - key_share
        - alpn
        - sni
        - status_request
      notes: Matches real Chrome-on-Linux handshake ordering.

This fingerprint is widely deployed and safe in most regions.

---

## QUIC Fingerprint

Linux QUIC fingerprint:

    quic_fingerprint:
      client: Chrome QUIC (Linux)
      version: h3
      notes: Matches Chrome desktop QUIC behavior.

If QUIC is blocked or degraded, fallback triggers immediately.

---

## SNI Strategy

Linux uses a rotating SNI strategy:

    sni_strategy:
      default: rotating
      high_risk_regions: fronted or ECH (if supported)
      notes: Rotating SNI matches Chrome-on-Linux behavior.

This strategy reduces fingerprintability.

---

## Bootstrap Behavior

Linux bootstrap flow:

    bootstrap_behavior:
      - QUIC-first handshake
      - Chrome-style handshake ordering
      - Desktop timing and packet-size normalization
      - TLS attempted only after QUIC failure

This mirrors real Chrome behavior on Linux.

---

## Fallback Chain

Linux uses a region-aware fallback chain:

    fallback_chain: tls-fingerprint-rotation (region-dependent)

Region-specific overrides:

    region_overrides:
      quic_blocking: tls-first
      tls_fingerprint_filtering: tls-fingerprint-rotation
      sni_blocking: sni-blocking
      active_probing: active-probing

Linux is often targeted by TLS fingerprint filtering, so rotation is important.

---

## Timing Profile

Linux uses a desktop-normal timing profile:

    timing_profile:
      quic_retry: 200–600 ms
      tls_retry: 300–900 ms
      http2_retry: 500–1200 ms
      cdn_retry: 800–1500 ms
      notes: Matches real Linux networking behavior.

Desktop timing is smoother and less jittery than mobile.

---

## Error Normalization

Linux error normalization:

    error_normalization:
      tls: TLS alerts (handshake_failure, protocol_version)
      quic: QUIC close frames (CONNECTION_TIMEOUT)
      http2: 502/503 pages
      cdn: 504 Gateway Timeout
      http: generic browser errors

Errors must match real Chrome-on-Linux behavior.

---

## Integration

Linux profile integrates with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Platform-specific overrides ensure realistic behavior.

---

## Summary

The Linux client profile defines platform-specific transport priorities, TLS/QUIC fingerprints, fallback behavior, and timing normalization.  
By matching Chrome-on-Linux behavior, the system blends into legitimate desktop traffic and avoids cross-platform inconsistencies.
