# Sanitizer Rules

## Version and Effective Date
- **Rule version**: v1.0  
- **Effective date**: 2026-04-14  
- **Owner**: Emergency Publishing System — Sanitizer Team

---

## Purpose and Scope
**Purpose**: Define mandatory behavioral rules for the Sanitizer in the `emergency-channel` to ensure deterministic removal of metadata, fingerprints, and identifiable markers before content enters downstream systems, while preserving auditability and unlinkability.  
**Scope**: Applies to all code, configuration, and runtime instances under `emergency-publishing-system/emergency-channel/sanitizer`, covering text, image, video, and document processing and fallback paths.

---

## Core Rules (Mandatory)
1. **Zero Metadata Leakage**  
   - No output may contain original EXIF, IPTC, XMP, PDF metadata, embedded thumbnails, or revision histories.  
   - Container headers must be rebuilt from deterministic templates; original headers or uncleaned container fields must not remain.

2. **Deterministic Output**  
   - Given identical input, configuration, and toolchain versions, outputs must be identical.  
   - Encoding parameters, quantization, timestamps, and other relevant settings must be fixed and recorded in processing reports.

3. **Sandbox Isolation**  
   - All sanitization operations must run inside an isolated sandbox with **no network access**.  
   - Temporary files and intermediate artifacts must be securely wiped at task completion or failure.

4. **Reject Rather Than Partial Sanitize**  
   - If metadata or fingerprints cannot be fully removed, the submission must be rejected. Partial sanitization that leaves traces is prohibited.  
   - Rejection responses must be generic and non‑diagnostic; logs may only contain anonymized indicators.

5. **Minimal Logging**  
   - Log only anonymized operational metrics and structured processing reports. Do not log raw content, unhashed identifiers, or reversible metadata.  
   - Logs must be purgeable and support non‑persistent deployment modes.

---

## Format and Type Specific Rules
### Text
- Normalize to UTF‑8; remove BOM, directionality controls (RLO, LRO), zero‑width characters (ZWJ, ZWNJ), and hidden comments.  
- Strip front‑matter, editor tags, and invisible metadata blocks. Normalize line endings and collapse excessive whitespace.

### Images
- Remove EXIF, IPTC, XMP, embedded thumbnails, and color profile traces. Convert or normalize color space to sRGB.  
- Decode to raw pixel buffer and re‑encode with deterministic JPEG/PNG/WebP parameters in metadata‑free containers.

### Video
- Extract frames and audio tracks; remove container metadata, GPS tracks, and encoder signatures.  
- Normalize frame rate, resolution, and color space; re‑encode with deterministic MP4/WebM profiles and fixed muxer settings.

### Documents
- When permitted, convert DOCX/ODT to sanitized PDF/A. Remove object streams, revision history, macros, and embedded attachments.  
- Flatten layers and annotations as required; rebuild document structure using deterministic PDF/A templates.

---

## Fallback and Failure Strategies
1. **Format Conversion Fallback**  
   - If the original format cannot be safely sanitized, convert to a controlled, deterministic target format (e.g., corrupted JPEG → PNG; complex DOCX → PDF/A).  
   - Discard and securely wipe original format artifacts after conversion.

2. **Re‑Encoding Fallback**  
   - If the primary encoder fails, use a predefined baseline encoder (e.g., H.264 profile mismatch → baseline H.264).  
   - Fallback encoders must use deterministic settings; non‑deterministic compression routines are forbidden.

3. **Structural Repair Fallback**  
   - Apply deterministic repairs for minor structural issues (missing EOF markers, non‑critical padding errors, minor timestamp inconsistencies).  
   - Repairs must not alter semantic content; if deterministic repair is not possible, reject.

4. **Safe Failure Path**  
   - If all fallbacks fail, reject the submission and securely wipe all temporary data.  
   - Rejection must be generic and non‑diagnostic; internal logs may record anonymized failure categories for monitoring.

---

## Resource and Runtime Quotas (Deterministic)
- **CPU**: Fixed CPU quota per task; long‑running operations are terminated deterministically.  
- **Memory**: Sandbox memory ceilings; large media may be downscaled or rejected. No in‑memory caching of originals.  
- **Disk I/O**: Ephemeral isolated temporary storage with per‑task caps; wipe on completion.  
- **Execution Time**: Per‑stage and per‑fallback maximum execution times; exceeding limits triggers safe failure.  
- **Concurrency**: Limit concurrent sanitization tasks; high load triggers queueing or rate limiting.

---

## Observability, Reporting, and Audit
- Every processing run must emit a **structured, versioned SanitizationReport** (no reversible metadata or raw content). Example:

```json
{
  "version": "string",
  "submission_id": "string",
  "removed_metadata": ["string"],
  "reencoded": true,
  "format_conversion": "string or null",
  "processing_steps": [
    { "stage": "string", "status": "ok | recoverable_error | unrecoverable_error", "notes": "string" }
  ],
  "risk_score": 0.0,
  "timestamp": "ISO8601 string"
}
```

- Metrics to collect: latency distribution, throughput, rejection rate (by reason), fallback counts, adversarial detection counts, and false positive rates.  
- Reports and metrics may be used for internal monitoring and post‑incident analysis but must be anonymized and aggregated before any external exposure.

---

## Testing, Adversarial Validation, and CI
- Maintain an adversarial test corpus covering XSS, steganography, malformed containers, codec spoofing, and other attack vectors.  
- Run unit tests, regression tests, fuzzing, and CI validation to ensure deterministic outputs and toolchain compatibility.  
- Any change to rules or toolchain must include regression tests, version records, and a documented migration path.

---

## Governance and Change Management
- Version and document all rules, regexes, thresholds, and processing templates in `rules.md` and `rules-history.md`.  
- Record processing toolchain versions (encoders, muxers, libraries) in each processing report.  
- Rule changes require review, regression testing, and a published effective date and migration instructions.

---

## Exceptions and Audit Requests
- Any exception to "reject rather than partial sanitize" or "zero metadata leakage" requires formal approval and an auditable exception record.  
- Audit requests are limited to internal compliance and security teams; external audit requests must be anonymized or refused with a generic response.

---

## Glossary
- **Deterministic**: Given the same input, configuration, and toolchain, the output is repeatable and identical.  
- **Metadata**: Descriptive or tracking information embedded in files or containers (EXIF, IPTC, XMP, PDF fields, etc.).  
- **Fingerprint**: Traces left by devices or encoders that can be used for correlation or identification.  
- **Sandbox**: Isolated execution environment with no network access, limited resources, and secure temporary storage.

---
