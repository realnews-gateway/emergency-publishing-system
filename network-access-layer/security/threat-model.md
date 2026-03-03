# Threat Model

This document defines the threat model for the VPN access layer, including adversary capabilities, attack surfaces, and the security guarantees provided by the system.  
The threat model focuses on state-level adversaries, network-level attackers, and censorship infrastructures capable of deep packet inspection, active probing, and traffic analysis.

The goal is to ensure that the system remains secure, stealthy, and censorship-resistant under hostile network conditions.

---

## Adversary Classes

The system considers the following adversary classes:

### 1. Local Network Adversary
    - Controls local Wi-Fi or ISP infrastructure
    - Can observe, modify, or block traffic
    - Can perform passive DPI
    - Can inject TCP resets or QUIC close frames

### 2. State-Level Censorship Infrastructure
    - Full visibility into national backbone traffic
    - Can deploy large-scale DPI systems
    - Can block protocols selectively
    - Can perform active probing
    - Can correlate traffic across regions

### 3. Active Probing Adversary
    - Sends synthetic traffic to suspected endpoints
    - Attempts to trigger protocol-specific responses
    - Replays captured handshakes
    - Tests for protocol deviations or errors

### 4. Traffic Analysis Adversary
    - Performs statistical analysis on packet sizes, timing, and flow patterns
    - Attempts to distinguish VPN traffic from normal TLS/QUIC traffic
    - Uses machine learning classifiers

### 5. Endpoint Compromise Adversary (Out of Scope)
    - Full device compromise
    - Malware with root access
    - Kernel-level monitoring

    These threats are explicitly out of scope for the VPN access layer.

---

## Attack Surfaces

The system exposes the following potential attack surfaces:

### 1. Transport Layer
    - TLS handshake fingerprinting
    - QUIC version negotiation
    - HTTP/2 frame patterns
    - CDN domain-fronting behavior

### 2. Session Initialization
    - Key exchange messages
    - Transport negotiation metadata
    - Bootstrap timing patterns

### 3. Camouflage Layer
    - SNI values
    - Certificate fields
    - ALPN identifiers
    - TLS extension ordering

### 4. Fallback Logic
    - Protocol downgrade patterns
    - Error messages
    - Retry timing

### 5. Traffic Patterns
    - Packet size distributions
    - Flow timing
    - Burstiness and idle periods

---

## Adversary Capabilities

The system assumes adversaries may:

### Passive Capabilities
    - Inspect all packets
    - Extract TLS ClientHello fields
    - Analyze QUIC Initial packets
    - Perform statistical traffic analysis
    - Detect protocol anomalies

### Active Capabilities
    - Inject packets
    - Replay captured handshakes
    - Perform active probing
    - Block specific protocols (QUIC/TLS/HTTP2)
    - Block specific SNI values or IP ranges

### Correlation Capabilities
    - Correlate flows across networks
    - Detect simultaneous connections
    - Identify fallback transitions

---

## Security Goals

The system provides the following guarantees:

### 1. Confidentiality
    - All traffic is encrypted end-to-end
    - No metadata leakage beyond unavoidable TLS/QUIC fields

### 2. Integrity
    - All packets are authenticated
    - Replay attacks are rejected

### 3. Indistinguishability
    - Traffic resembles legitimate TLS/QUIC/HTTP traffic
    - No protocol-specific identifiers
    - No custom error codes

### 4. Censorship Resistance
    - Automatic fallback across transports
    - Region-aware behavior
    - Error normalization to avoid detection

### 5. Active Probing Resistance
    - Endpoints do not reveal themselves
    - Challenge/response authentication
    - Replay-resistant handshake

---

## Out-of-Scope Threats

The system does not attempt to defend against:

    - Compromised client devices
    - Compromised servers
    - Kernel-level monitoring
    - Side-channel attacks on hardware
    - Global adversaries with full end-to-end visibility

These threats require solutions beyond the VPN access layer.

---

## Integration with Other Modules

The threat model informs:

- **camouflage/**  
  TLS fingerprints, SNI strategies, handshake obfuscation.

- **session-init/**  
  Key exchange, transport negotiation, replay protection.

- **fallback/**  
  Error normalization, region-aware fallback chains.

- **entrypoints/**  
  Protocol-specific attack surface reduction.

- **client-profiles/**  
  Platform-specific security adaptations.

Security is embedded across all layers of the system.

---

## Summary

The threat model defines adversary capabilities, attack surfaces, and security guarantees for the VPN access layer.  
By assuming state-level adversaries with DPI, active probing, and traffic analysis capabilities, the system is designed to remain stealthy, resilient, and censorship-resistant under hostile network conditions.
