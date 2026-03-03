# Key Exchange

The Key Exchange subsystem defines how clients and servers derive secure, ephemeral session keys during session initialization.  
It is designed to resist replay attacks, active probing, metadata leakage, and protocol fingerprinting, while remaining indistinguishable from normal TLS/QUIC behavior.

Key exchange is a foundational component of censorship‑resistant session establishment.

---

## Purpose

Key exchange enables:

- Secure derivation of ephemeral session keys  
- Replay-resistant handshake flows  
- Covert embedding inside TLS/QUIC handshakes  
- Optional hybrid post-quantum protection  
- Authentication without exposing identifiers  
- Integration with camouflage and negotiation layers  

This ensures that every session begins with strong cryptographic guarantees and minimal detectability.

---

## Design Principles

The key exchange subsystem follows four core principles:

### **1. Ephemeral by Default**
All session keys are derived from:

- One-time ephemeral x25519 key pairs  
- Optional hybrid post-quantum KEM components  
- Nonces and tokens that prevent replay  

No long-term keys are exposed during handshake.

---

### **2. Covert Embedding**
Key exchange messages are embedded inside:

- TLS ClientHello / ServerHello extensions  
- QUIC transport parameters  
- HTTP/2 pseudo-headers (fallback mode)  
- CDN-compatible metadata (domain-fronted mode)  

This prevents DPI from identifying custom handshake patterns.

---

### **3. Replay Resistance**
Replay protection is enforced through:

- One-time handshake tokens  
- Nonces tied to session timestamps  
- Rejecting repeated ClientHello messages  
- Optional server-issued challenge/response  

Replay resistance is essential for preventing fingerprint harvesting.

---

### **4. Camouflage Alignment**
Key exchange must align with:

- TLS fingerprint camouflage  
- Real-site mimicry  
- SNI randomization  
- Handshake obfuscation  

This ensures that cryptographic operations do not create detectable anomalies.

---

## Key Exchange Flow

The key exchange process consists of five stages:

### **1. Client Ephemeral Key Generation**
Client generates:

- Ephemeral x25519 key pair  
- Optional PQ hybrid component  
- Nonce for replay protection  

### **2. Server Ephemeral Key Generation**
Server generates:

- Ephemeral x25519 key pair  
- Optional PQ hybrid component  
- Session token for authentication

### **3. Shared Secret Derivation**
Both sides compute the shared secret:

    shared_secret = ECDH(client_ephemeral, server_ephemeral)

If hybrid post‑quantum mode is enabled:

    shared_secret = HKDF(ECDH_secret || PQ_secret)

This ensures forward secrecy and optional post‑quantum resistance.

### **4. Session Key Derivation**
Session keys are derived using HKDF with domain‑separated labels:

- Encryption keys  
- Authentication keys  
- Traffic normalization parameters  
- Camouflage profile parameters  
- Optional fallback negotiation hints  

All derived keys are ephemeral and discarded after session termination.

### **5. Session Confirmation**
Both sides confirm:

- Transport selection  
- Camouflage profile  
- Authentication status  
- Replay protection validity  
- Key derivation success  

Once confirmed, the session transitions into the fully established state.

---

## Optional Hybrid Post‑Quantum Mode

Hybrid mode combines:

- x25519 ECDH  
- A post‑quantum KEM (e.g., Kyber)  

Advantages:

- Future‑proof security  
- Protection against “harvest now, decrypt later” attacks  
- Minimal overhead when embedded inside TLS/QUIC  
- Region‑configurable activation  

Hybrid mode is recommended for high‑risk regions.

---

## Integration

Key exchange integrates with:

- **negotiation.md**  
  Begins immediately after transport and camouflage negotiation.

- **authentication.md**  
  Authentication tokens may be exchanged during key exchange.

- **camouflage/**  
  Key exchange is embedded inside TLS/QUIC camouflage layers.

- **fallback/**  
  Fallback chains may trigger alternate key exchange flows.

- **client-profiles/**  
  Platform‑specific key exchange behavior is defined here.

---

## Security Considerations

Key exchange must:

- Avoid static key reuse  
- Prevent replayable handshake flows  
- Normalize timing and packet sizes  
- Resist active probing  
- Avoid exposing backend infrastructure  
- Support optional post‑quantum hybrid mode  
- Ensure covert embedding inside TLS/QUIC  

---

## Summary

The Key Exchange subsystem provides secure, ephemeral, and covert session key derivation.  
By embedding key exchange inside TLS/QUIC handshakes and aligning with camouflage techniques, the system achieves strong cryptographic security while remaining indistinguishable from legitimate internet traffic.
