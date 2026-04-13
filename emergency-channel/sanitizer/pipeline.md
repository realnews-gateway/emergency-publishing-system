## Sanitization Pipeline Overview

The Sanitizer Pipeline defines the full, deterministic sequence of operations applied to incoming content to ensure complete metadata removal, fingerprint neutralization, and format normalization. Each stage runs inside an isolated sandbox, produces auditable processing reports, and yields correlation‑resistant outputs suitable for downstream routing and storage.

The pipeline handles text, images, videos, and documents using format‑specific routines while maintaining strict anonymity and safety guarantees.

---

## Pipeline Stages

The Sanitization Pipeline consists of five deterministic stages. Each stage is isolated, reproducible, and designed to eliminate metadata, fingerprints, and format‑specific identifiers while preserving semantic content.

### 1. Intake and Sandbox Initialization
- Accept validated content from the Validation module.
- Initialize a secure, ephemeral sandbox with strict resource quotas and no network access.
- Load format‑specific sanitization routines and deterministic toolchain versions.

### 2. Format Detection and Routing
- Determine true file type using magic‑byte inspection and lightweight structural checks.
- Reject ambiguous, encrypted, or unsupported formats immediately.
- Route content to the appropriate sanitization path: **Text**, **Image**, **Video**, or **Document**.

### 3. Metadata Extraction and Removal
- Scan for EXIF, IPTC, XMP, PDF metadata, embedded thumbnails, revision histories, and hidden streams.
- Remove GPS coordinates, device identifiers, software signatures, encoder tags, and editor traces.
- Normalize container headers and rebuild structural fields using deterministic templates.

### 4. Re‑Encoding and Fingerprint Neutralization
- Re‑encode media with fixed, deterministic encoder parameters to remove latent fingerprints.
- Convert proprietary or opaque formats into safe, open formats when permitted by policy.
- Apply fingerprint neutralization routines to remove encoder noise patterns, watermarks, and deterministic compression artifacts.

### 5. Output Normalization and Packaging
- Produce a consistent output structure across all sanitized content types.
- Apply policy‑driven resizing, recompression, or rewrapping when required for safety.
- Emit a sanitized blob and a structured, versioned sanitization report for downstream consumers.

---

## Format‑Specific Pipelines

Each content type follows a dedicated sanitization pipeline tailored to its metadata structures, fingerprint risks, and encoding characteristics. All pipelines are deterministic and executed inside an isolated sandbox.

### Text Pipeline
**Steps**
- Normalize encoding to UTF‑8.
- Remove hidden Unicode markers (BOM, RLO, LRO, ZWJ, ZWNJ) and directionality controls.
- Strip embedded metadata blocks (front‑matter, editor tags, invisible comments).
- Collapse excessive whitespace and normalize line endings.

**Output**
- Clean UTF‑8 text with no hidden markers or structural identifiers.

### Image Pipeline
**Steps**
- Decode into a raw pixel buffer.
- Remove EXIF, IPTC, XMP, embedded thumbnails, and color profile traces.
- Normalize color profile to sRGB.
- Re‑encode using deterministic JPEG/PNG/WebP settings with fixed quantization and metadata‑free containers.

**Output**
- Metadata‑free image with neutralized encoder fingerprints.

### Video Pipeline
**Steps**
- Extract raw frames and audio tracks.
- Remove container metadata, embedded GPS tracks, and encoder signatures.
- Normalize frame rate, resolution, and color space.
- Re‑encode using deterministic MP4/WebM profiles and fixed muxer settings.

**Output**
- Re‑encoded video with no device or encoder identifiers.

### Document Pipeline
**Steps**
- Convert DOCX/ODT to sanitized PDF/A when permitted.
- Remove embedded metadata, object streams, revision history, macros, and hidden attachments.
- Flatten layers, annotations, and embedded media where required.
- Rebuild document using deterministic PDF/A settings and sanitized object templates.

**Output**
- Sanitized PDF/A with no embedded metadata or hidden structures.

---

## Fallback Paths

Controlled fallback paths preserve deterministic behavior when primary routines fail. Fallbacks are strictly limited and must never compromise anonymity or metadata safety.

### Format Conversion Fallback
Used when the original format cannot be safely sanitized.

**Examples**
- Corrupted JPEG headers → convert to PNG  
- Unsupported DOCX features → convert to PDF/A  
- Non‑standard WebM profiles → convert to MP4 (deterministic profile)

**Rules**
- Conversion must produce a deterministic, metadata‑free format.
- Original format artifacts must be discarded and securely wiped.

### Re‑Encoding Fallback
Used when the primary encoder fails due to corruption or unsupported profiles.

**Examples**
- H.264 profile mismatch → fallback to baseline H.264  
- PNG alpha issues → fallback to sanitized PNG without alpha  
- WebP decoding errors → fallback to JPEG

**Rules**
- Fallback encoders must use deterministic settings.
- Non‑deterministic compression routines are prohibited.

### Structural Repair Fallback
Used when minor structural inconsistencies can be safely corrected.

**Examples**
- Missing EOF markers in PDFs  
- Non‑critical container padding errors  
- Minor timestamp inconsistencies in video containers

**Rules**
- Repairs must not alter semantic content.
- If repair cannot be completed deterministically, reject the content.

### Safe Failure Path
If all fallback paths fail, the pipeline rejects the content.

**Rules**
- Rejection responses are generic and non‑diagnostic.
- No partial sanitization results are returned.
- Unsafe blobs and temporary artifacts are securely wiped from the sandbox.

---

## Resource Controls

The pipeline enforces strict, deterministic resource controls to prevent denial‑of‑service, sandbox escapes, and excessive computation.

### CPU Limits
- Fixed CPU quota per task; long‑running operations are terminated deterministically.
- Re‑encoding routines must complete within predefined time windows.

### Memory Limits
- Sandboxes enforce strict memory ceilings.
- Large media may be downscaled or rejected; no in‑memory caching of originals.

### Disk I/O Limits
- Temporary files stored in ephemeral, isolated filesystems.
- Maximum temporary storage per task is capped.
- All temporary data is securely wiped after sanitization.

### Execution Time Limits
- Each pipeline stage has a maximum execution time; fallbacks respect global time limits.
- Tasks exceeding limits are terminated and marked failed.

### Parallelization Controls
- Concurrency limits for sanitization tasks; high load triggers queueing or rate limiting.
- Tasks may not spawn subprocesses outside the sandbox.

### Safety‑First Termination
- Resource exhaustion triggers safe failure and sandbox destruction.
- No partial outputs are returned.

---

## Security Notes

The Sanitization Pipeline enforces strict security guarantees to ensure no metadata, fingerprints, or identifiers survive sanitization.

### Zero Metadata Retention
- No EXIF, IPTC, XMP, PDF metadata may remain after sanitization.
- Container headers are rebuilt using deterministic templates.
- Hidden thumbnails, GPS tracks, and revision histories are removed.

### Deterministic Re‑Encoding
- All re‑encoding routines use fixed, deterministic parameters.
- Identical inputs produce identical sanitized outputs under the same configuration.
- Non‑deterministic encoders are prohibited unless fully documented and versioned.

### Sandbox Isolation
- All operations occur inside isolated sandboxes with no network access.
- Temporary files are securely wiped after use.

### No Partial Sanitization
- If any metadata or fingerprint cannot be removed, the content is rejected.
- Rejection messages do not reveal internal sanitization logic.

### Minimal Logging
- Only anonymized operational metrics and structured processing reports are logged.
- No raw content, metadata details, or unhashed identifiers appear in logs.
- Logs are purgeable and non‑persistent in hostile deployments.

### Defense Against Correlation Attacks
- Re‑encoding removes encoder signatures and deterministic artifacts.
- Proprietary formats are converted to safe, open formats when possible.
- Fingerprint neutralization is applied consistently across media types.

---

These rules ensure the Sanitization Pipeline provides strong anonymity guarantees, deterministic behavior, and robust defenses against metadata leakage and correlation attacks.
