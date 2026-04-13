# Emergency Channel — Image Sanitizer

## 1. Purpose

The Image Sanitizer ensures that all incoming image content is safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes harmful metadata, validates formats, enforces size and resolution limits, and prepares images for downstream processing such as storage and distribution.

The sanitizer must operate deterministically and avoid altering the visual meaning of the image unless required for safety.

---

## 2. Responsibilities

### 2.1 Security Filtering

- Strip all embedded scripts or executable payloads  
- Remove malicious or unsupported metadata fields  
- Validate MIME type and reject disguised formats  
- Detect and block steganographic payloads when possible  

### 2.2 Format Normalization

- Convert unsupported formats to safe, standardized formats (e.g., PNG or JPEG)  
- Normalize color profiles  
- Remove alpha channels if not allowed  
- Ensure consistent encoding  

### 2.3 Policy Enforcement

- Enforce maximum file size  
- Enforce maximum resolution  
- Validate aspect ratio constraints  
- Reject corrupted or partially downloaded images  

### 2.4 Metadata Sanitization

- Remove EXIF data (GPS, device info, timestamps)  
- Remove ICC profiles unless required  
- Remove thumbnails and embedded previews  
- Remove application-specific metadata blocks  

Metadata removal protects user privacy and reduces fingerprinting risks.

---

## 3. Architecture

The Image Sanitizer is implemented as a deterministic, multi‑stage pipeline.  
Each stage performs a specific transformation, and the output of one stage becomes the input of the next.

### 3.1 Processing Stages

The sanitizer pipeline consists of:

1. **Input Validation**  
   - Verify MIME type  
   - Validate file header (magic bytes)  
   - Reject disguised or malformed images  

2. **Metadata Removal**  
   - Strip EXIF, GPS, device identifiers  
   - Remove ICC profiles unless required  
   - Remove embedded thumbnails and previews  

3. **Format Normalization**  
   - Convert unsupported formats to PNG or JPEG  
   - Normalize color profiles  
   - Remove alpha channels if disallowed  

4. **Security Filtering**  
   - Remove embedded scripts or payloads  
   - Detect steganographic anomalies (heuristic)  
   - Reject images with suspicious binary patterns  

5. **Policy Enforcement**  
   - Enforce file size limits  
   - Enforce resolution and aspect ratio constraints  
   - Reject corrupted or partially downloaded images  

6. **Output Encoding**  
   - Re‑encode image using safe, deterministic settings  
   - Ensure consistent compression parameters  
   - Produce a sanitized, stable output image  

Each stage is isolated and independently testable.

---

### 3.2 Deterministic Output

The sanitizer must always produce the same output for the same input:

- No random compression parameters  
- No environment‑dependent color profile behavior  
- No time‑dependent metadata  
- No heuristic‑based transformations that vary between runs  

Determinism ensures predictable downstream processing and reproducible results.

---

### 3.3 Error Handling

Errors are categorized into:

- **Recoverable errors**  
  - Minor metadata corruption  
  - Unsupported but convertible formats  
  - Missing or invalid EXIF blocks  
  - These are corrected automatically  

- **Unrecoverable errors**  
  - Malicious payloads  
  - Severe corruption  
  - Oversized or unbounded images  
  - These result in rejection  

All errors are logged for analytics and debugging.

---

### 3.4 Performance Considerations

The sanitizer is optimized for:

- Low memory usage  
- Streaming‑based processing  
- Linear‑time decoding and re‑encoding  
- High throughput under load  

It must handle large volumes of images without blocking the pipeline.

---

## 4. Security Considerations

The Image Sanitizer is a critical security boundary.  
It must defend the Emergency Channel against malformed, malicious, or intentionally crafted image inputs.

### 4.1 Threat Model

The sanitizer protects against:

- Embedded executable payloads  
- Script injection via metadata  
- Steganographic channels used for covert communication  
- Oversized images intended to exhaust memory  
- Corrupted or malformed headers  
- MIME type spoofing  
- Pixel-level anomalies indicating tampering  

All incoming images are treated as untrusted.

---

### 4.2 Allowed and Disallowed Content

**Allowed:**

- PNG, JPEG, GIF (static), WebP (static)  
- Deterministic re‑encoded formats  
- Images within size and resolution limits  
- Safe color profiles  

**Disallowed:**

- Animated formats (GIF/WebP) unless explicitly permitted  
- SVG or vector formats containing scripts  
- HEIC/HEIF or proprietary formats  
- Images with embedded audio/video streams  
- Files with mixed or ambiguous MIME types  
- Images containing executable or encrypted payloads  

Disallowed content is removed or the entire submission is rejected.

---

### 4.3 Logging and Auditing

The sanitizer logs:

- Rejected images  
- Metadata removal events  
- Format conversions  
- Policy violations  
- Suspicious binary patterns  
- Steganography detection alerts  

Logs are anonymized and used for analytics, debugging, and threat monitoring.

---

## 5. Summary

The Image Sanitizer ensures that all image content entering the Emergency Channel is:

- Safe  
- Clean  
- Deterministic  
- Policy‑compliant  
- Free of harmful metadata  
- Ready for storage and distribution  

It is a foundational component of the system’s security and reliability.
