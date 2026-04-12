# Publisher Module — Content Formatting

## Overview

The formatting layer transforms sanitized internal content into
publishable artifacts.  
Each publishing channel requires a specific output format, and the
formatting layer ensures that content is correctly structured, styled,
and packaged before delivery.

Formatting is strictly **non-destructive**:  
it does not alter the meaning of the content, only its representation.

Formatting is **deterministic and reproducible**, ensuring consistent
output across deployments.

---

## Formatting Responsibilities

The formatting subsystem performs:

- **Content structuring**  
  Converting internal objects into channel‑specific layouts.

- **Template rendering**  
  Applying HTML, RSS, JSON, Markdown, or micro‑feed templates.

- **Metadata shaping**  
  Including only safe, non-sensitive metadata.

- **Payload generation**  
  Producing final artifacts ready for delivery.

- **Validation**  
  Ensuring the output conforms to channel specifications.

Formatting is transport‑agnostic:  
it prepares artifacts without knowing which adapter will deliver them.

---

## Supported Output Formats

The Publisher module currently supports:

### 1. HTML
- Full article pages  
- Minimal mirror‑friendly pages  
- CDN‑optimized static bundles  

### 2. RSS / Atom
- Standard feed items  
- Lightweight feeds for low‑bandwidth environments  

### 3. JSON
- API responses  
- Mobile‑client payloads  
- Third‑party integration formats  

### 4. Markdown
- Developer‑friendly or archival formats  
- Optional plaintext fallbacks  

### 5. Text‑Only Micro‑Feeds
- Extremely compact payloads  
- Designed for censorship‑heavy environments  

---

## Metadata Policy

The formatting layer enforces strict metadata hygiene:

- Removes internal processing metadata  
- Removes sensitive timestamps  
- Removes internal IDs and routing information  
- Keeps only safe, minimal metadata (title, time, tags, language)

This prevents accidental leakage of internal system details.

---

## Template System

The formatting layer uses a modular template system:

- Each channel has its own template set  
- Templates can be overridden per deployment  
- Templates support versioning for long‑term stability  
- Templates are stateless and side‑effect‑free  

This ensures that formatting remains predictable and easy to maintain.

---

## Summary

The formatting subsystem provides:

- Clean, channel‑specific output formats  
- Strong metadata protection  
- Deterministic and reproducible rendering  
- Modular templates for easy customization  
- Compatibility with both rich and minimal publishing channels  

It ensures that sanitized content is transformed into safe, high‑quality
artifacts ready for delivery.
