# Security Architecture

The security subsystem defines the threat model, DPI evasion strategies, active probing defenses, and overall security guarantees of the **network-access-layer**.  
This module ensures that the system remains resilient against state-level adversaries, network-level attackers, and active probing systems while maintaining stealth and indistinguishability from legitimate traffic.

Security is integrated across all layers of the **network-access-layer**, including camouflage, session initialization, fallback logic, and transport behavior.

---

## Objectives

The security subsystem provides:

- A formal threat model  
- DPI evasion strategies  
- Active probing defenses  
- Traffic analysis resistance  
- Protocol-level obfuscation  
- Error normalization and timing camouflage  
- Region-aware security adaptations  
- Key management and replay protection  

These components ensure that the system remains robust under adversarial conditions.

---

## Components

The security subsystem consists of:

- **threat-model.md**  
  Defines adversary capabilities, attack surfaces, and security guarantees.

- **dpi-evasion.md**  
  Describes techniques for evading deep packet inspection, including TLS/QUIC camouflage and handshake obfuscation.

- **active-probing-defenses.md**  
  Defines defenses against active probing, replay attacks, and protocol fingerprinting.

- **traffic-analysis-resistance.md** *(optional extension)*  
  Describes strategies for resisting traffic analysis, including timing camouflage and packet size normalization.

- **key-management.md** *(optional extension)*  
  Defines secure key exchange, rotation, and replay protection strategies.

These documents collectively define the security posture of the **network-access-layer**.

---

## Integration with Other Modules

Security integrates with:

- **camouflage/**  
  TLS fingerprints, SNI strategies, handshake obfuscation.

- **session-init/**  
  Key exchange, transport negotiation, bootstrap flow.

- **fallback/**  
  Error normalization, region-aware fallback chains.

- **entrypoints/**  
  Protocol-specific entrypoint behavior.

- **client-profiles/**  
  Platform-specific security adaptations.

Security is not a standalone module; it is embedded throughout the system.

---

## Security Principles

The system follows these core principles:

### Defense in Depth
Multiple layers of protection ensure resilience even if one layer is compromised.

### Indistinguishability
Traffic must be indistinguishable from legitimate TLS/QUIC/HTTP traffic.

### Region Awareness
Security posture adapts to local censorship and adversary capabilities.

### Minimal Attack Surface
Avoid unnecessary protocol features, metadata, or identifiers.

### Active Probing Resistance
Endpoints must not reveal themselves under probing or replay attempts.

### Error Normalization
All failures must resemble real-world browser or CDN errors.

### Traffic Analysis Resistance
Timing, packet size, and flow behavior must blend into normal application traffic.

### Key Management
Keys must be rotated securely, with replay protection and forward secrecy.

---

## Summary

The security subsystem defines the threat model, DPI evasion strategies, active probing defenses, and traffic analysis resistance that protect the **network-access-layer** from state-level adversaries.  
By integrating security across camouflage, session initialization, fallback logic, and transport behavior, the system maintains strong censorship resistance and stealth under hostile network conditions.  
Extensions such as **traffic-analysis-resistance.md** and **key-management.md** provide additional depth, ensuring resilience against evolving threats.
