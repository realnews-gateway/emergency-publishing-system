# System‑Wide Anonymous Account Layer

The Empus account layer provides a lightweight, privacy‑preserving
identity system shared across all applications. It enables users to
maintain a persistent pseudonym, manage their own content, and store
personal favorites — all without exposing identity to the Emergency
Channel or any transport layer.

The account layer is optional, minimal, and fully isolated from network
metadata, device identifiers, and behavioral tracking.

---

## 1. Purpose

The account layer provides:

- A persistent pseudonym (system‑generated name + avatar)
- Personal content management (viewing and managing one’s own posts)
- Cross‑application favorites (news, posts, tools)
- Optional email‑based password recovery
- Optional 2FA for account protection

Accounts do not require phone numbers, real names, or personal
information.

---

## 2. Design Principles

### 2.1 Privacy First

- No IP addresses are stored  
- No device identifiers are stored  
- No behavioral analytics or profiling  
- No cross‑session correlation beyond the account itself  

### 2.2 Isolation from Emergency Channel

The Emergency Channel never receives:

- Account identifiers  
- Login state  
- Session tokens  
- Email addresses  
- 2FA information  

All content passed to the Emergency Channel is fully sanitized and
identity‑free.

### 2.3 Minimal and Optional

Users may:

- Use the system without an account (read‑only)
- Create an account without providing personal information
- Add optional recovery methods only if desired

---

## 3. Core Capabilities

### 3.1 Pseudonym Identity

Each account receives a system‑generated pseudonym consisting of:

- A generated display name  
- A generated avatar  

Pseudonyms are public‑facing, but cannot be reversed to reveal the
account.

### 3.2 Personal Content Management

Users can:

- View all posts they have created  
- Edit or delete their own posts (application‑specific)  
- Track their publishing history privately  

Applications (e.g., Anonymous BBS) integrate with this capability.

### 3.3 Favorites (Cross‑Application)

The account layer provides a unified favorites system:

- Save posts, news items, or other application‑defined objects  
- Favorites remain private to the user  
- Applications define what can be favorited  

### 3.4 Optional Recovery Features

Users may optionally add:

- Email for password reset  
- Time‑based 2FA  

These are never required.

---

## 4. Data Model

The full data model is defined in `model.md`, including:

- Account identifiers  
- Pseudonym structure  
- Recovery fields  
- Security constraints  

---

## 5. Privacy Guarantees

Detailed privacy guarantees are documented in `privacy.md`, including:

- No identity linkage  
- No metadata retention  
- No behavioral tracking  
- Strict separation from Emergency Channel  

---

## 6. Favorites System

The favorites subsystem is documented in `favorites.md`, covering:

- Cross‑application schema  
- Storage model  
- Application integration rules  

---

## 7. Integration with Applications

Applications such as:

- Anonymous BBS  
- News Aggregation  
- Tools  

use the account layer for identity and personal data, but never expose
account identifiers to the Emergency Channel.

---

The account layer provides a safe, minimal, and privacy‑preserving
foundation for user identity across the Empus ecosystem.
