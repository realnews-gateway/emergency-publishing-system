
# Message Gateway: Input, Pseudonym Assignment, and Content Filtering

The message gateway is the entry point for all posts submitted to the anonymous BBS.  
Users authenticate with lightweight accounts, but the gateway ensures that all public‑facing identity is pseudonymous and that no real user information is ever exposed.  
Its core responsibility is to handle **user input**, attach pseudonyms, and filter out inappropriate or non‑emergency content before messages are handed off to the global pipeline.

---

## Objectives

The gateway provides:

- Account authentication without exposing account identity  
- Pseudonym assignment (name + avatar) for public display  
- Delegation of metadata cleaning and stripping to **emergency-channel**  
- Message normalization and safety filtering  
- Rate limiting and abuse prevention  
- Filtering of spam, adult material, and non‑emergency content (Empus is strictly for emergency publishing)  
- Forwarding of valid messages to **storage** and **distribution**  

---

## Gateway Pipeline

The gateway processes each incoming message through four stages:

### 1. Authentication
- Validates the user's lightweight account (via **accounts**)  
- Ensures the account is not banned or rate‑limited  
- Does NOT attach account ID to the public message  

### 2. Pseudonym Assignment
- Generates a pseudonym (name + avatar) for the message  
- Pseudonym can be:  
  - Per‑account (stable identity)  
  - Per‑thread (contextual identity)  
  - Per‑session (rotating identity)  
- No correlation to account ID  

### 3. Message Normalization
- Cleans and sanitizes message body  
- Removes HTML, scripts, and unsafe markup  
- Enforces maximum length  
- Normalizes whitespace and punctuation  
- Applies optional keyword moderation  

### 4. Content Filtering and Abuse Prevention
- Delegates metadata cleaning and stripping to **emergency-channel**  
- Filters out spam, adult material, and non‑emergency posts  
- Applies rate limiting (account‑level, not identity‑level)  
- Detects flooding or abnormal posting patterns  
- Flags inappropriate content internally without exposing user identity  

Messages that pass all checks are forwarded to **storage** and then handed off to **distribution**.

---

## Pseudonym Assignment Details

The gateway uses a pseudonym generator that produces:

```
pseudonym:
  name: e.g., "Quiet Lantern", "Blue Sparrow", "Silent River"
  avatar: abstract shapes, animals, geometric icons
```

Properties:

- Deterministic per‑account if configured  
- Or randomized per‑thread for anonymity  
- Never reused across unrelated contexts  
- Never derived from account data  

This ensures anonymity while maintaining conversational continuity.

---

## Integration with Other Modules

The gateway **interfaces with**:

- **accounts/** — Validates user authentication and rate limiting  
- **emergency-channel/** — Handles metadata cleaning, stripping, and temporary cache  
- **message-format.md** — Defines the public and internal message structures  
- **storage/** — Receives normalized, pseudonymous messages for append‑only storage  
- **distribution/** — Delivers messages to clients via region‑aware channels  
- **network-access-layer/** — Provides censorship‑resistant transport  

---

## Summary

The message gateway ensures that all incoming posts are authenticated, pseudonymized, normalized, and safe before entering the BBS system.  
By focusing on user input, pseudonym assignment, and filtering out spam, adult material, and non‑emergency content, it protects users while preserving Empus as a platform dedicated to emergency publishing.
