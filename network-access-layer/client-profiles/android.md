# Android Client Profile

The Android client profile defines platform-specific behavior for transport negotiation, TLS/QUIC fingerprints, fallback logic, and timing normalization.  
Android devices exhibit highly diverse network stacks, widespread QUIC adoption, and Chrome-like fingerprints that must be replicated for stealth.

This profile ensures that the system behaves like a native Android application and blends into legitimate Chrome-on-Android traffic.

---

## Platform Characteristics

Android exhibits the following network traits:

- QUIC is widely deployed and often preferred  
- TLS behavior closely matches Chrome on Android  
- SNI rotation is common across apps  
- Timing patterns match mobile applications  
- Packet sizes follow mobile networking conventions  
- Network conditions vary significantly across devices and carriers  

These characteristics inform transport priority and camouflage selection.

---

## Transport Priority

Android uses a QUIC-first strategy:

    transport_priority:
      - QUIC
      - TLS
      - HTTP/2
      - CDN

Rationale:

- QUIC is widely supported on Android  
- QUIC-first improves performance and stealth  
- TLS is a reliable fallback  
- HTTP/2 and CDN are used when necessary  

---

## TLS Fingerprint

Android uses a Chrome-on-Android TLS fingerprint:

    tls_fingerprint:
      client: Chrome (Android)
      version: TLS 1.3
      extensions:
        - supported_versions
        - signature_algorithms
        - key_share
        - alpn
        - sni
        - status_request
      notes: Matches real Chrome-on-Android handshake ordering.

This fingerprint is extremely common and safe in most regions.

---

## QUIC Fingerprint

Android QUIC fingerprint:

    quic_fingerprint:
      client: Chrome QUIC (Android)
      version: h3
      notes: Matches Chrome mobile QUIC behavior.

If QUIC is blocked or degraded, fallback triggers immediately.

---

## SNI Strategy

Android uses a rotating SNI strategy:

    sni_strategy:
      default: rotating
      high_risk_regions: fronted or ECH (if supported)
      notes: Rotating SNI matches Chrome-on-Android behavior.

Mobile apps frequently rotate SNI, making this strategy natural.

---

## Bootstrap Behavior

Android bootstrap flow:

    bootstrap_behavior:
      - QUIC-first handshake
      - Chrome-style handshake ordering
      - Mobile timing and packet-size normalization
      - TLS attempted only after QUIC failure

This mirrors real Chrome behavior on Android.

---

## Fallback Chain

Android uses a region-aware fallback chain:

    fallback_chain: quic-blocking (region-dependent)

Region-specific overrides:

    region_overrides:
      quic_blocking: tls-first
      tls_fingerprint_filtering: tls-fingerprint-rotation
      sni_blocking: sni-blocking
      active_probing: active-probing

---

## Timing Profile

Android uses a mobile-fast timing profile:

    timing_profile:
      quic_retry: 120–350 ms
      tls_retry: 200–600 ms
      http2_retry: 400–900 ms
      cdn_retry: 700–1400 ms
      notes: Matches real Android networking behavior.

Mobile timing is faster and more jittery than desktop.

---

## Error Normalization

Android error normalization:

    error_normalization:
      tls: TLS alerts (handshake_failure, internal_error)
      quic: QUIC close frames (CONNECTION_TIMEOUT)
      http2: 502/503 pages
      cdn: 504 Gateway Timeout
      http: generic mobile browser errors

Errors must match real Chrome-on-Android behavior.

---

## Integration

Android profile integrates with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Platform-specific overrides ensure realistic behavior.

---

## Summary

The Android client profile defines platform-specific transport priorities, TLS/QUIC fingerprints, fallback behavior, and timing normalization.  
By matching Chrome-on-Android behavior, the system blends into legitimate mobile traffic and avoids cross-platform inconsistencies.
