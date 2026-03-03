# Authentication

The Authentication subsystem defines how clients prove legitimacy to servers without exposing long‑term identifiers, static tokens, or replayable credentials.  
It is designed to operate covertly inside TLS/QUIC handshakes and remain indistinguishable from normal web traffic.

Authentication is tightly integrated with negotiation and key exchange, forming a unified, censorship‑resistant session initialization flow.

---

## Purpose

Authentication enables:

- Verification of legitimate clients  
- Protection against unauthorized access  
- Avoidance of static or replayable identifiers  
- Covert embedding inside existing protocol structures  
- Region‑aware authentication strategies  
- Compatibility with domain‑fronted and CDN-backed entrypoints  

This ensures secure access control without creating detectable patterns.

---

## Design Principles

### **1. No Static Identifiers**
The system must avoid:

- Long-term user IDs  
- Static API keys  
- Reusable tokens  
- Predictable authentication headers  

Static identifiers are easily harvested by censors and must never appear in traffic.

---

### **2. Ephemeral and Time‑Bound**
Authentication relies on:

- Short‑lived tokens  
- One-time session tickets  
- Time‑windowed credentials  
- Server-issued challenges  

This prevents replay attacks and fingerprint collection.

---

### **3. Covert Embedding**
Authentication data is embedded inside:

- TLS extensions  
- QUIC transport parameters  
- HTTP/2 pseudo-headers  
- CDN-compatible metadata  

This ensures authentication traffic blends into normal encrypted flows.

---

### **4. Region‑Aware Behavior**
Authentication strategy adapts to:

- High-risk regions  
- Environments with active probing  
- Networks with TLS/SNI filtering  
- CDN-only access paths  

Different regions may require different authentication modes.

---

## Authentication Modes

The subsystem supports three primary authentication modes:

---

### **1. Ephemeral Token Authentication**
Clients receive short-lived tokens via:

- Out-of-band channels  
- Domain-fronted bootstrap flows  
- Time-limited distribution endpoints  

Tokens are:

- Single-use  
- Time-bound  
- Bound to negotiation parameters  

This is the default mode.

---

### **2. Session Ticket Authentication**
Servers issue encrypted session tickets that allow:

- Fast reconnection  
- Reduced negotiation overhead  
- Stateless server-side validation  

Tickets are:

- Encrypted  
- Rotated frequently  
- Bound to client capabilities  

Session tickets never contain static identifiers.

---

### **3. Challenge/Response Authentication**
Used in high-risk regions.

Flow:

1. Client sends an obfuscated authentication hint  
2. Server returns a challenge embedded in TLS/QUIC metadata  
3. Client responds using key-exchange-derived secrets  
4. Server validates and completes authentication  

This mode provides strong protection against active probing.

---

## Authentication Flow

Authentication occurs in four stages:

### **1. Pre‑Authentication Hint**
Client sends:

- Ephemeral token  
- Session ticket  
- Or challenge request  

Embedded covertly inside handshake metadata.

---

### **2. Server Validation**
Server validates:

- Token freshness  
- Ticket integrity  
- Challenge request legitimacy  

Invalid attempts receive normalized errors.

---

### **3. Challenge (Optional)**
Server may issue:

- Time-bound challenge  
- Nonce tied to negotiation  
- Obfuscated response instructions  

Used only in high-risk environments.

---

### **4. Authentication Confirmation**
Client responds, and server confirms:

- Authentication success  
- Replay protection  
- Camouflage alignment  
- Transport consistency  

Session proceeds to full establishment.

---

## Error Normalization

Authentication failures must resemble:

- TLS alerts  
- QUIC close frames  
- HTTP error pages  
- CDN-style responses  

This prevents censors from distinguishing authentication failures from normal web failures.

---

## Integration

Authentication integrates with:

- **negotiation.md**  
  Authentication hints may be exchanged during negotiation.

- **key-exchange.md**  
  Challenge/response uses key-exchange-derived secrets.

- **camouflage/**  
  Authentication data is embedded inside camouflage layers.

- **fallback/**  
  Authentication mode may change based on fallback chain.

- **client-profiles/**  
  Platform-specific authentication behavior is defined here.

---

## Security Considerations

Authentication must:

- Avoid static identifiers  
- Prevent replayable credentials  
- Normalize timing and packet sizes  
- Resist active probing  
- Support region-aware strategies  
- Blend into TLS/QUIC traffic patterns  

---

## Summary

The Authentication subsystem provides secure, covert, and censorship‑resistant client verification.  
By relying on ephemeral tokens, session tickets, and optional challenge/response flows, it ensures strong access control while remaining indistinguishable from legitimate encrypted traffic.
