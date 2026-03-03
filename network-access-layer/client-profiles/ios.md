# iOS Client Profile

The iOS client profile defines platform-specific behavior for transport negotiation, TLS/QUIC fingerprints, fallback logic, and timing normalization.  
iOS has a highly opinionated networking stack, strict TLS behavior, and Safari-style fingerprints that must be replicated precisely for stealth.

This profile ensures that the system behaves like a native iOS application and blends into legitimate Safari/Chrome-on-iOS traffic.

---

## Platform Characteristics

iOS exhibits the following network traits:

- TLS behavior is tightly controlled by the OS  
- Safari fingerprints are low entropy and widely deployed  
- QUIC support is available but mobile-optimized  
- SNI behavior is stable and predictable  
- Timing patterns match mobile applications  
- iOS aggressively caches TLS session state  
- Background networking constraints may apply  

These characteristics inform transport priority and camouflage selection.

---

## Transport Priority

iOS uses a TLS-first strategy:

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

Rationale:

- TLS is universally supported and stable  
- QUIC is available but not always preferred by the OS  
- HTTP/2 is a reliable fallback  
- CDN fallback is used only when necessary  

---

## TLS Fingerprint

iOS uses a Safari-on-iOS TLS fingerprint:

    tls_fingerprint:
      client: Safari (iOS)
      version: TLS 1.3
      extensions:
        - supported_versions
        - signature_algorithms
        - key_share
        - alpn
        - sni
        - status_request
      notes: Must match Safari exactly; iOS is strict about TLS behavior.

Safari fingerprints are extremely common and safe in high-risk regions.

---

## QUIC Fingerprint

iOS QUIC support is mobile-optimized:

    quic_fingerprint:
      client: Safari QUIC (iOS)
      version: h3
      notes: Enabled only when QUIC is confirmed available.

If QUIC is blocked or degraded, fallback triggers immediately.

---

## SNI Strategy

iOS uses a stable SNI strategy:

    sni_strategy:
      default: stable
      high_risk_regions: ECH (if supported)
      notes: Stable SNI matches Safari-on-iOS behavior.

iOS rarely rotates SNI, making stable SNI the safest option.

---

## Bootstrap Behavior

iOS bootstrap flow:

    bootstrap_behavior:
      - TLS-first handshake
      - Safari-style handshake ordering
      - Mobile timing and packet-size normalization
      - QUIC attempted only after TLS success

This mirrors real Safari and Chrome behavior on iOS.

---

## Fallback Chain

iOS uses a region-aware fallback chain:

    fallback_chain: sni-blocking (region-dependent)

Region-specific overrides:

    region_overrides:
      quic_blocking: tls-first
      tls_fingerprint_filtering: tls-fingerprint-rotation
      sni_blocking: sni-blocking
      active_probing: active-probing

---

## Timing Profile

iOS uses a mobile-optimized timing profile:

    timing_profile:
      quic_retry: 150–450 ms
      tls_retry: 250–700 ms
      http2_retry: 400–900 ms
      cdn_retry: 700–1400 ms
      notes: Matches real iOS networking behavior.

Mobile timing is faster and more jittery than desktop.

---

## Error Normalization

iOS error normalization:

    error_normalization:
      tls: TLS alerts (handshake_failure, internal_error)
      quic: QUIC close frames (IDLE_TIMEOUT)
      http2: 502/503 pages
      cdn: 504 Gateway Timeout
      http: generic mobile browser errors

Errors must match real Safari/Chrome-on-iOS behavior.

---

## Integration

iOS profile integrates with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Platform-specific overrides ensure realistic behavior.

---

## Summary

The iOS client profile defines platform-specific transport priorities, TLS/QUIC fingerprints, fallback behavior, and timing normalization.  
By matching Safari-on-iOS behavior, the system blends into legitimate mobile traffic and avoids cross-platform inconsistencies.
