# Privacy and Identity Protection

The Empus account layer is designed to provide persistent identity
features without compromising user anonymity. This document defines the
privacy guarantees, isolation rules, and non‑retention policies that
govern all account‑related data.

The account layer is fully isolated from the Emergency Channel and never
interacts with transport metadata, routing logic, or published content.

---

## 1. Privacy Principles

The account system is built on three core principles:

### 1.1 Minimal Data Collection
Only the information strictly required for account functionality is
stored. No personal identifiers are required.

### 1.2 Strict Isolation
Account data is never shared with the Emergency Channel or any transport
layer.

### 1.3 No Behavioral Tracking
No analytics, profiling, or behavioral correlation is performed.

---

## 2. Data That Is Never Stored

The account layer does not store:

- IP addresses
- Device identifiers
- Browser fingerprints
- Transport metadata
- Session history
- Login timestamps
- Location information
- Behavioral metrics
- Publishing history linked to identity
- Any metadata from uploaded content

These fields are permanently excluded from all logs and storage.

---

## 3. Identity Separation

Empus enforces strict separation between:

- Account identity  
- Pseudonym identity  
- Publishing identity  
- Transport identity  

### 3.1 Account Identity
Internal, opaque, never exposed outside the account layer.

### 3.2 Pseudonym Identity
Public‑facing but non‑reversible.  
Cannot be linked back to account_id.

### 3.3 Publishing Identity
Content published through applications (e.g., Anonymous BBS) is not
linked to account_id or pseudonym.

### 3.4 Transport Identity
Network‑level identifiers (IP, TLS fingerprint, etc.) never reach the
account layer.

---

## 4. Isolation from Emergency Channel

The Emergency Channel never receives:

- account_id
- pseudonym_name
- pseudonym_avatar
- email or recovery data
- 2FA secrets
- favorites
- personal content lists
- login state
- session tokens

All content entering the Emergency Channel is fully sanitized and
identity‑free.

---

## 5. Optional Recovery Data

Users may optionally add:

- Email for password reset
- Time‑based 2FA

Privacy guarantees:

- Recovery data is optional
- Recovery data is never shared with applications
- Recovery data is never shared with the Emergency Channel
- Recovery data is never used for analytics or correlation

---

## 6. Favorites Privacy

Favorites are:

- Private to the user
- Stored only at the account layer
- Never exposed to other users
- Never sent to the Emergency Channel
- Never used for profiling or recommendations

Applications may read favorites but cannot infer identity.

---

## 7. Application Access Rules

Applications may:

- Read pseudonym identity
- Read/write favorites
- Read/write personal content references

Applications may not:

- Access recovery data
- Access password hashes
- Access 2FA secrets
- Access internal security flags
- Access account_id directly

Applications must not embed account identifiers in published content.

---

## 8. Logging and Audit Rules

The account layer logs only:

- Internal operational errors (anonymized)
- Security flags (non‑identifying)
- Account creation events (non‑identifying)

Logs must not include:

- Content
- Metadata
- Identifiers
- Transport information
- Behavioral data

Logs must be purgeable and non‑persistent in hostile deployments.

---

## 9. Threat Model Considerations

The account layer is designed to resist:

- Network‑level surveillance
- Metadata correlation attacks
- Cross‑application identity inference
- Transport‑layer fingerprinting
- Behavioral profiling
- Credential compromise (via optional 2FA)

The system assumes adversaries may have:

- Network visibility
- Device visibility
- Access to published content

But adversaries must not be able to link:

- Account → Pseudonym  
- Account → Published content  
- Account → Transport metadata  

---

## 10. Summary

The account layer provides persistent identity features while preserving
complete anonymity. It stores minimal data, avoids all metadata
retention, and maintains strict isolation from the Emergency Channel and
transport layers.

These guarantees ensure that users can safely participate in the Empus
ecosystem without exposing personal identity or behavioral patterns.
