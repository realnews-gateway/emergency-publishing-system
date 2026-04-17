§
# Anonymous Message Format and Pseudonymous Identity

This document defines the message format used by the anonymous BBS system.  
Although users authenticate with lightweight accounts, all public‑facing identity is pseudonymous:  
messages display only system‑generated names and avatars, never real user information.

The message format is designed to minimize metadata, preserve anonymity, and ensure compatibility with censorship‑resistant distribution channels.

---

## Objectives

The message format provides:

- Strict separation between account identity and public identity  
- System‑generated pseudonyms (name + avatar)  
- Minimal metadata for privacy and censorship resistance  
- Coarse‑grained or jittered timestamps  
- Compact structure for low‑bandwidth environments  
- Compatibility with append‑only logs and replication layers  
- **Mandatory removal of personally identifiable information (PII)** such as names, phone numbers, and precise addresses  
- Reinforcement that Empus is a **Publishing Platform**, not a **Response Center**  

---

## Message Structure

Each message consists of two layers:

### 1. Internal Layer (private, server‑side)
- Contains account ID for access control and abuse prevention  
- Never exposed to clients or included in public feeds  
- Used only by the gateway and moderation systems  

### 2. Public Layer (exposed to clients)
- Contains pseudonymous identity  
- Contains message body and minimal metadata  
- Safe for replication and distribution  

---

## Internal Message Format (Private)

```
internal_message:
  internal_id: Unique message identifier
  account_id: Internal account identifier
  received_at: High-precision timestamp
  raw_body: Original message body before normalization
  flags:
    moderation_flags: List of flags (spam, adult, non-emergency, PII)
```

Notes:

- `account_id` is never exposed publicly.  
- `raw_body` is kept only for debugging or moderation.  
- Metadata such as IP or device info is stripped by **emergency-channel**.  
- PII is **always filtered and removed** before storage or distribution.  

---

## Public Message Format (Exposed)

```
message:
  id: Public message ID
  pseudonym:
    name: System-generated display name
    avatar: System-generated avatar identifier
  body: Cleaned and normalized message text (PII removed)
  timestamp: Coarse-grained or jittered timestamp
  tags: Optional list of system-generated tags
  thread_id: ID of the thread or topic
  reply_to: Optional parent message ID
```

### Pseudonym Rules

- Randomized name generator (e.g., "Silver Lantern", "Quiet Fox")  
- Avatar generator (abstract shapes, colors, animals, etc.)  
- No correlation to account ID  
- Optionally rotated per‑thread or per‑session  

---

## Timestamp Handling

To prevent correlation attacks:

- Rounded to coarse intervals (e.g., 5 minutes)  
- Or jittered randomly within a small window  
- Or replaced with relative time ("2 hours ago")  

Timestamps never include millisecond precision, client local time, or network‑derived metadata.

---

## Message Normalization

Before storage and distribution, messages are normalized:

- Remove HTML or unsafe markup  
- Normalize whitespace  
- Strip tracking links or metadata  
- Enforce maximum message length  
- Convert emojis to safe representations  
- **Mandatory removal of PII**: names, phone numbers, and precise addresses are automatically stripped to protect user safety  

### Safety Filters
- Keyword moderation  
- Spam detection  
- Filtering of adult or non‑emergency content  

---

## Pre‑publishing Disclaimer

Before posting, users are reminded:

- **Not a rescue channel**: this platform is only for anonymous information sharing and publishing. We cannot provide immediate rescue services.  
- **Privacy protection**: do not include names, phone numbers, or precise addresses in your message. The system will automatically strip such information to prevent exposure.  
- **Emergency help**: if you are in life‑threatening danger, please contact official institutions directly:  
  - [Agency A address/hotline]  
  - [Agency B mirror site]  

This disclaimer reinforces that Empus is a **Publishing Platform**, not a **Response Center**.  
Publishing PII in expectation of rescue may expose users to malicious interception during distribution and increase personal risk.  
By providing clear redirection to official rescue channels, Empus ensures that users are guided toward professional help when needed.  

---

## Privacy Guarantees

The message format enforces:

- **No Real Identity Exposure**: no usernames, emails, phone numbers, or account IDs  
- **No Network Metadata**: no IP addresses, device identifiers, or user‑agent strings  
- **No Correlation Across Threads**: pseudonyms can rotate per‑thread or per‑session  

---

## Integration with Other Modules

The message format **interfaces with**:

- **gateway.md** — Applies pseudonym assignment, normalization, and filtering  
- **storage/** — Stores public messages in append‑only logs  
- **distribution/** — Delivers compact public messages to clients  
- **emergency-channel/** — Handles metadata cleaning and temporary cache  

---

## Summary

The anonymous BBS message format provides a strict separation between account identity and public identity.  
By using system‑generated pseudonyms, minimal metadata, privacy‑preserving timestamps, and **mandatory PII removal**, it ensures that users can communicate safely and anonymously while preserving Empus as a platform dedicated to emergency publishing.  
The **Pre‑publishing Disclaimer** further clarifies that Empus is not a rescue service, and directs users to official institutions for urgent help.
