# Favorites System

The favorites subsystem provides a unified, cross‑application mechanism
for users to save content they consider important or worth revisiting.
Favorites are stored at the account layer and remain private to the
user. Applications define what can be favorited and how each favorite
item is interpreted.

The favorites system is fully isolated from the Emergency Channel and
never appears in any transport, routing, or distribution pipeline.

---

## 1. Purpose

The favorites subsystem enables:

- Saving posts, news items, or application‑defined objects
- Cross‑application access to saved items
- Private, account‑scoped personal collections
- Consistent behavior across all Empus applications

Favorites are optional and do not affect publishing or distribution.

---

## 2. Design Principles

### 2.1 Application‑Defined Content Types

The account layer does not interpret the meaning of a favorite.
Each application defines:

- What can be favorited
- How item identifiers are structured
- How favorites are displayed to the user

Examples:

- Anonymous BBS: posts, comments
- News Aggregation: news items
- Tools: saved reports or drafts

### 2.2 Privacy and Isolation

Favorites are:

- Private to the user
- Never shared with other users
- Never exposed to the Emergency Channel
- Never included in published content
- Never used for analytics or behavioral profiling

### 2.3 Minimal and Portable

Favorites are stored in a simple, portable format that allows:

- Cross‑application access
- Future application integration
- Migration without revealing identity

---

## 3. Data Model

Favorites are stored as an array of objects under each account.

Fields:

- app_id — Application identifier
- item_id — Application‑defined content identifier
- added_at — Timestamp when the item was saved

Properties:

- The account layer does not interpret item_id
- Applications must ensure item_id is stable and resolvable
- No metadata beyond the three fields is stored

---

## 4. Application Integration Rules

Applications integrating with the favorites system must follow:

### 4.1 Application‑Scoped Identifiers

Applications must define:

- A stable app_id
- A stable item_id format

### 4.2 No Identity Leakage

Applications must not:

- Encode account_id inside item_id
- Encode transport metadata
- Encode session identifiers
- Encode IP or device information

### 4.3 Local Rendering Only

Applications are responsible for:

- Rendering favorite items
- Resolving item_id to actual content
- Handling missing or deleted items gracefully

The account layer does not perform lookups.

---

## 5. Security and Privacy Guarantees

- Favorites are private to the account
- No favorites data is ever sent to the Emergency Channel
- No favorites data is logged or analyzed
- No cross‑application behavioral profiling
- No inference of user identity or preferences

Favorites exist solely as a user convenience feature.

---

## 6. Future Extensions

Potential future enhancements include:

- Tagging or grouping favorites (client‑side only)
- Export/import for offline use
- Application‑specific favorite categories

All extensions must preserve privacy and isolation guarantees.

---

The favorites subsystem provides a simple, private, and extensible
mechanism for personal content management across the Empus ecosystem.
