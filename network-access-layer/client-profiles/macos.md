# macOS Client Profile

The macOS client profile defines platform-specific behavior for transport negotiation, TLS/QUIC fingerprints, fallback logic, and timing normalization.  
macOS has a highly consistent TLS stack, predictable SNI behavior, and Safari-style fingerprints that are low entropy and safe in high-risk regions.

This profile ensures that the system behaves like a native macOS application and blends into legitimate Safari/Chrome traffic.

---

## Platform Characteristics

macOS exhibits the following network traits:

- TLS behavior closely matches Safari  
- QUIC support depends on macOS version  
- SNI behavior is stable and predictable  
- Timing patterns match desktop applications  
- Packet sizes follow macOS networking conventions  
- Safari fingerprints are widely deployed and low-risk  

These characteristics inform transport priority and camouflage selection.

---

## Transport Priority

macOS uses a TLS-first strategy:

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

Rationale:

- Safari-style TLS is extremely common  
- QUIC support varies by OS version  
- HTTP/2 is a reliable fallback  
- CDN fallback is used only when necessary  

---

## TLS Fingerprint

macOS uses a Safari-on-macOS TLS fingerprint:

    tls_fingerprint:
      client: Safari (macOS)
      version: TLS 1.3
      extensions:
        - supported_versions
        - signature_algorithms
        - key_share
        - alpn
        - sni
        - status_request
      notes: Matches real Safari handshake ordering and extension layout.

Safari fingerprints are low entropy and safe in high-risk regions.

---

## QUIC Fingerprint

macOS QUIC support is version-dependent:

    quic_fingerprint:
      client: Safari QUIC (macOS)
      version: h3
      notes: Enabled only on macOS versions with native QUIC support.

If QUIC is unavailable or blocked, fallback triggers immediately.

---

## SNI Strategy

macOS uses a stable SNI strategy:

    sni_strategy:
      default: stable
      high_risk_regions: ECH (if supported)
      notes: Stable SNI matches Safari behavior.

Safari rarely rotates SNI, making stable SNI the safest option.

---

## Bootstrap Behavior

macOS bootstrap flow:

    bootstrap_behavior:
      - TLS-first handshake
      - Safari-style handshake ordering
      - QUIC attempted only when supported
      - Desktop timing and packet-size normalization

This mirrors real Safari and Chrome behavior on macOS.

---

## Fallback Chain

macOS uses the global default chain unless region overrides apply:

    fallback_chain: global-default

Region-specific overrides:

    region_overrides:
      quic_blocking: tls-first
      tls_fingerprint_filtering: tls-fingerprint-rotation
      sni_blocking: sni-blocking
      active_probing: active-probing

---

## Timing Profile

macOS uses a desktop-smooth timing profile:

    timing_profile:
      quic_retry: 250–700 ms
      tls_retry: 350–950 ms
      http2_retry: 500–1200 ms
      cdn_retry: 800–1500 ms
      notes: Smooth timing matches macOS networking behavior.

---

## Error Normalization

macOS error normalization:

    error_normalization:
      tls: TLS alerts (handshake_failure, internal_error)
      quic: QUIC close frames (IDLE_TIMEOUT)
      http2: 502/503 pages
      cdn: 504 Gateway Timeout
      http: generic browser errors

Errors must match real Safari/Chrome behavior on macOS.

---

## Integration

macOS profile integrates with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Platform-specific overrides ensure realistic behavior.

---

## Summary

The macOS client profile defines platform-specific transport priorities, TLS/QUIC fingerprints, fallback behavior, and timing normalization.  
By matching Safari-on-macOS behavior, the system blends into legitimate desktop traffic and avoids cross-platform inconsistencies.
