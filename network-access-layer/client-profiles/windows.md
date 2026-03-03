# Windows Client Profile

The Windows client profile defines platform-specific behavior for transport negotiation, TLS/QUIC fingerprints, fallback logic, and timing normalization.  
Windows has unique networking characteristics, including variable QUIC support, system-level TLS behavior, and browser-like traffic patterns that must be replicated for stealth.

This profile ensures that the system behaves like a native Windows application and blends into legitimate traffic.

---

## Platform Characteristics

Windows exhibits the following network traits:

- QUIC support varies by OS version  
- TLS stack closely resembles Chrome on Windows  
- SNI behavior is stable and predictable  
- Timing patterns match desktop applications  
- Packet sizes follow typical Windows networking patterns  

These characteristics inform transport priority and camouflage selection.

---

## Transport Priority

Windows uses a TLS-first strategy due to OS-level consistency:

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

Rationale:

- TLS is universally supported  
- QUIC availability varies by Windows version  
- HTTP/2 is a reliable fallback  
- CDN fallback is used only when necessary  

---

## TLS Fingerprint

Windows uses a Chrome-on-Windows TLS fingerprint:

    tls_fingerprint:
      client: Chrome (Windows)
      version: TLS 1.3
      extensions:
        - supported_versions
        - signature_algorithms
        - key_share
        - alpn
        - sni
        - status_request
      notes: Matches real Chrome handshake ordering and extension layout.

This fingerprint is widely deployed and safe in most regions.

---

## QUIC Fingerprint

Windows QUIC support is inconsistent, so QUIC is secondary:

    quic_fingerprint:
      client: Chrome QUIC (Windows)
      version: h3
      notes: Used only when QUIC is confirmed available.

If QUIC is blocked or unavailable, fallback triggers immediately.

---

## SNI Strategy

Windows uses a region-aware SNI strategy:

    sni_strategy:
      default: rotating
      high_risk_regions: fronted or ECH (if supported)
      notes: Rotating SNI reduces fingerprintability.

---

## Bootstrap Behavior

Windows bootstrap flow:

    bootstrap_behavior:
      - TLS-first handshake
      - QUIC attempted only after TLS success
      - Desktop timing and packet-size normalization
      - Chrome-like handshake ordering

This mirrors real Windows Chrome behavior.

---

## Fallback Chain

Windows uses the global default chain unless region overrides apply:

    fallback_chain: global-default

Region-specific overrides:

    region_overrides:
      quic_blocking: tls-first
      tls_fingerprint_filtering: tls-fingerprint-rotation
      sni_blocking: sni-blocking
      active_probing: active-probing

---

## Timing Profile

Windows uses a desktop-normal timing profile:

    timing_profile:
      quic_retry: 200–600 ms
      tls_retry: 300–900 ms
      http2_retry: 500–1200 ms
      cdn_retry: 800–1500 ms
      notes: Randomized timing prevents fingerprinting.

---

## Error Normalization

Windows error normalization:

    error_normalization:
      tls: TLS alerts (handshake_failure, protocol_version)
      quic: QUIC close frames (CONNECTION_TIMEOUT)
      http2: 502/503 pages
      cdn: 504 Gateway Timeout
      http: generic browser errors

Errors must match real Windows Chrome behavior.

---

## Integration

Windows profile integrates with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Platform-specific overrides ensure realistic behavior.

---

## Summary

The Windows client profile defines platform-specific transport priorities, TLS/QUIC fingerprints, fallback behavior, and timing normalization.  
By matching Chrome-on-Windows behavior, the system blends into legitimate desktop traffic and avoids cross-platform inconsistencies.
