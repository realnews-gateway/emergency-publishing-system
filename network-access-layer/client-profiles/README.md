# Client Profiles

Client profiles define platform-specific behavior for session initialization, transport negotiation, camouflage selection, fallback logic, and error normalization.  
Each platform has unique network characteristics, TLS/QUIC fingerprints, and OS-level constraints that must be reflected in the system's behavior.

Client profiles ensure that the system behaves like a native application on each platform and blends into legitimate traffic patterns.

---

## Purpose

Client profiles provide:

- Platform-specific TLS/QUIC fingerprints  
- Transport priority ordering  
- Camouflage profile selection  
- Bootstrap behavior  
- Fallback chain overrides  
- Timing and retry adjustments  
- OS-level networking constraints  

Profiles ensure that each platform behaves realistically and avoids cross-platform fingerprint mismatches.

---

## Platform Profiles

The system defines profiles for:

- Windows  
- macOS  
- iOS  
- Android  
- Linux  

Each profile includes:

- transport_priority  
- tls_fingerprint  
- quic_fingerprint  
- sni_strategy  
- bootstrap_behavior  
- fallback_chain  
- timing_profile  
- notes  

---

## Windows Profile

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

    tls_fingerprint: Chrome (Windows)
    quic_fingerprint: Chrome QUIC
    sni_strategy: rotating or fronted (region-dependent)

    bootstrap_behavior:
      - TLS-first handshake
      - QUIC attempted after TLS success
      - Windows-native timing patterns

    fallback_chain: global-default
    timing_profile: desktop-normal

    notes:
      Windows QUIC support varies by version; TLS-first is safest.

---

## macOS Profile

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

    tls_fingerprint: Safari (macOS)
    quic_fingerprint: Safari QUIC (if available)
    sni_strategy: stable SNI or ECH (if supported)

    bootstrap_behavior:
      - Safari-style TLS handshake
      - QUIC enabled only on newer macOS versions

    fallback_chain: global-default
    timing_profile: desktop-smooth

    notes:
      Safari fingerprints are low entropy and safe in high-risk regions.

---

## iOS Profile

    transport_priority:
      - TLS
      - QUIC
      - HTTP/2
      - CDN

    tls_fingerprint: Safari (iOS)
    quic_fingerprint: Safari QUIC (mobile)
    sni_strategy: stable SNI or ECH

    bootstrap_behavior:
      - Mobile-optimized timing
      - TLS-first due to iOS network stack behavior

    fallback_chain: sni-blocking (region-dependent)
    timing_profile: mobile-optimized

    notes:
      iOS aggressively caches TLS state; fingerprints must match Safari exactly.

---

## Android Profile

    transport_priority:
      - QUIC
      - TLS
      - HTTP/2
      - CDN

    tls_fingerprint: Chrome (Android)
    quic_fingerprint: Chrome QUIC (mobile)
    sni_strategy: rotating or fronted

    bootstrap_behavior:
      - QUIC-first
      - Mobile timing and packet-size normalization

    fallback_chain: quic-blocking (region-dependent)
    timing_profile: mobile-fast

    notes:
      Android QUIC is widely deployed; QUIC-first improves performance and stealth.

---

## Linux Profile

    transport_priority:
      - QUIC
      - TLS
      - HTTP/2
      - CDN

    tls_fingerprint: Chrome (Linux)
    quic_fingerprint: Chrome QUIC
    sni_strategy: rotating

    bootstrap_behavior:
      - QUIC-first
      - Desktop timing with reduced jitter

    fallback_chain: tls-fingerprint-rotation (region-dependent)
    timing_profile: desktop-normal

    notes:
      Linux environments vary; Chrome fingerprints are safest.

---

## Timing Profiles

Timing profiles define platform-specific retry and handshake timing:

### desktop-normal
    quic_retry: 200–600 ms
    tls_retry: 300–900 ms
    http2_retry: 500–1200 ms

### desktop-smooth
    quic_retry: 250–700 ms
    tls_retry: 350–950 ms

### mobile-optimized
    quic_retry: 150–450 ms
    tls_retry: 250–700 ms

### mobile-fast
    quic_retry: 120–350 ms
    tls_retry: 200–600 ms

Timing randomness prevents fingerprinting.

---

## Integration

Client profiles integrate with:

- session-init/bootstrap-flow.md  
- camouflage/tls-fingerprints.md  
- entrypoints/  
- fallback/chains.md  
- fallback/region-profiles.md  

Profiles override defaults when platform-specific behavior is required.

---

## Summary

Client profiles define platform-specific behavior for transport negotiation, camouflage selection, fallback logic, and timing normalization.  
By matching native application behavior on each platform, the system avoids cross-platform inconsistencies and blends seamlessly into legitimate traffic patterns.
