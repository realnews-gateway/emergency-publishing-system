## Sanitizer Overview

The Sanitizer module removes metadata, fingerprints, and embedded identifiers from incoming content. It guarantees that no device, location, software, or user trace remains before content enters the processing pipeline.

Sanitization is mandatory for all submissions and runs inside a secure sandbox to prevent leakage or correlation. The module supports text, image, video, and document formats, applying format‑specific routines to guarantee anonymity, determinism, and policy compliance.

---

## Purpose

- **Provide a strong privacy and security boundary** for all inbound content.  
- **Ensure deterministic, reproducible outputs** suitable for routing, storage, and distribution.  
- **Prevent covert channels, fingerprinting, and metadata leakage.**  
- **Fail safely:** ambiguous or unremovable traces cause rejection rather than partial sanitization.

---

## Sanitization Stages

The Sanitizer executes a multi‑stage pipeline inside an isolated sandbox. Each stage is deterministic, auditable, and independently testable.

### 1 Format Detection
- Identify true file type using magic‑byte inspection and lightweight structural checks.  
- Reject ambiguous, encrypted, or unsupported formats.  
- Select the appropriate format‑specific sanitization routine.

### 2 Metadata Removal
- Strip EXIF, IPTC, XMP, PDF metadata, embedded thumbnails, and revision histories.  
- Remove GPS coordinates, device identifiers, software signatures, and encoder fingerprints.  
- Normalize headers and container fields to eliminate hidden markers.

### 3 Reencoding and Fingerprint Neutralization
- Reencode images, videos, and documents with deterministic settings to remove latent fingerprints.  
- Convert proprietary or opaque formats into safe, open formats when permitted.  
- Apply deterministic encoding parameters to prevent correlation attacks.

### 4 Content Normalization
- Produce consistent output structure across sanitized files.  
- Resize, recompress, or rewrap media when required by policy.  
- Emit a sanitized blob and a structured processing report for downstream consumers.

---

## Supported Formats

Sanitizer supports a controlled set of formats to ensure reliable metadata removal and fingerprint neutralization. Unsupported or ambiguous formats are rejected.

### Text
**Supported:** TXT (UTF‑8), Markdown, plain JSON.  
Notes: All text is normalized to UTF‑8; hidden Unicode markers and directionality controls are removed.

### Images
**Supported:** JPEG (reencoded), PNG (reencoded), WebP (static).  
Notes: All EXIF, XMP, IPTC metadata removed; images may be recompressed to eliminate latent fingerprints.

### Video
**Supported:** MP4 (H.264/AAC), WebM (VP9/Opus) where conversion is permitted.  
Notes: Container metadata, thumbnails, and encoder fingerprints are stripped; reencoding applied as needed.

### Documents
**Supported:** PDF (sanitized PDF/A), DOCX and ODT converted to PDF/A when permitted.  
Notes: Object streams, revision history, embedded macros, and application metadata are removed.

### Unsupported Formats
**Rejected:** RAW camera formats, encrypted PDFs, DRM protected documents, executables, archives and scripts, proprietary containers that cannot be safely inspected.  
Rationale: These formats either resist deterministic sanitization or present unacceptable risk for metadata leakage.

---

## Data Contracts

Sanitizer exchanges strict, versioned data contracts with upstream validation and downstream processing. Contracts are stable and designed to avoid leaking sensitive details.

### SanitizerInput
Represents validated content passed from the Validation module.

```json
{
  "version": "string",
  "normalized_type": "text | image | video | document",
  "safe_blob": "binary",
  "metadata_flags": ["string"],
  "submission_id": "string",
  "received_at": "ISO8601 string"
}
```

### SanitizationReport
Internal report generated during sanitization.

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

### SanitizedContent
Final sanitized output ready for downstream processing.

```json
{
  "version": "string",
  "content_type": "text | image | video | document",
  "sanitized_blob": "binary",
  "sanitization_report": { "ref": "SanitizationReport" },
  "size_bytes": 0,
  "checksum": "anonymized_hash"
}
```

### RejectionRecord
Represents content that failed sanitization.

```json
{
  "version": "string",
  "submission_id": "string",
  "reason": "string",
  "anonymized_hash": "string",
  "notes": "string",
  "timestamp": "ISO8601 string"
}
```

---

## Error Handling

Sanitizer is designed to fail safely and deterministically. Any ambiguity, corruption, or unremovable metadata results in rejection.

### Format Errors
- Triggered when file type is unsupported, ambiguous, or encrypted.  
- Handling: reject without attempting risky transformations; do not disclose detected format to clients.

### Metadata Removal Errors
- Triggered when metadata cannot be fully removed (proprietary blocks, embedded GPS tracks).  
- Handling: reject with a generic unsafe response; log anonymized indicators.

### Reencoding Failures
- Triggered when media cannot be safely reencoded (corrupted frames, unsupported codec profiles).  
- Handling: attempt deterministic fallback reencoding; reject if all fallbacks fail.

### Fingerprint Neutralization Errors
- Triggered when latent fingerprints or watermarks cannot be reliably removed.  
- Handling: reject to prevent correlation attacks; flag for internal monitoring.

### Size and Resource Errors
- Triggered when content exceeds safe processing limits.  
- Handling: reject with a generic size-related response; do not reveal internal thresholds.

### Adversarial Inputs
- Triggered for steganographic payloads, crafted crash vectors, or repeated malformed submissions.  
- Handling: reject, apply rate limiting, and log anonymized adversarial indicators.

---

## Security Notes

Sanitizer enforces strict guarantees to prevent metadata leakage and correlation.

### Zero Metadata Leakage
- Remove all EXIF, XMP, IPTC, PDF metadata, thumbnails, and revision histories.  
- No original headers or container metadata may remain in sanitized outputs.

### Deterministic Output
- Use deterministic encoding parameters.  
- Identical inputs must yield identical sanitized outputs under the same configuration.  
- Non‑deterministic encoders are prohibited unless their behavior is fully documented and versioned.

### Secure Sandbox Execution
- Run all routines in isolated sandboxes with no external network access.  
- Wipe temporary files immediately after use.  
- Enforce strict resource quotas and timeouts.

### No Partial Sanitization
- If any metadata or fingerprint cannot be removed, reject the submission.  
- Partial or best‑effort sanitization that leaves traces is not permitted.

### Minimal Logging
- Log only anonymized operational metrics and structured processing reports.  
- Do not include raw content, metadata details, or unhashed identifiers in logs.  
- Ensure logs are purgeable and non‑persistent in hostile deployments.

### Defense Against Correlation Attacks
- Reencoding must remove encoder signatures and deterministic artifacts.  
- Convert proprietary formats to safe, open formats when possible.  
- Apply fingerprint neutralization consistently across all media types.

---

## Observability and Metrics

Instrument sanitization with metrics and traces to monitor health and detect adversarial patterns.

Recommended metrics:
- Processing latency distribution and percentiles.  
- Throughput and queue depth.  
- Rejection rate and breakdown by reason.  
- Counts of reencodes, format conversions, and metadata removals.  
- Adversarial detection counts and false positive rates.

Expose traces and processing reports for cross-correlation with Router and Storage telemetry while preserving anonymity.

---

## Testing and Validation

- Unit tests for each pipeline stage with deterministic fixtures.  
- Maintain an adversarial test corpus for XSS, steganography, malformed containers, and codec spoofing.  
- Run fuzzing and regression tests in CI.  
- Validate that identical inputs produce identical outputs across environments and documented toolchain versions.

---

## Governance and Versioning

- All sanitization rules, regexes, and policy thresholds must be versioned and documented in rules.md.  
- Processing toolchain versions (encoders, muxers, libraries) must be recorded in processing reports.  
- Changes to sanitization behavior require review, regression testing, and a documented migration path.

---

## Summary

The Sanitizer module provides a deterministic, auditable, and secure pipeline that removes metadata and fingerprints from inbound content. It enforces strict privacy guarantees, prevents correlation attacks, and produces stable outputs ready for downstream processing. Rejection is preferred over partial sanitization to preserve anonymity and system integrity.
