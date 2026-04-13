# Emergency Channel — Video Sanitizer

## 1. Purpose

The Video Sanitizer ensures that all incoming video content is safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes harmful metadata, validates container and codec integrity, enforces duration and resolution limits, and prepares videos for downstream processing such as storage and distribution.

The sanitizer must operate deterministically and avoid altering the semantic meaning of the video unless required for safety or policy compliance.

---

## 2. Responsibilities

### 2.1 Security Filtering

- Detect and block embedded executable payloads and attachments.  
- Remove malicious or unsupported metadata fields.  
- Validate container structure and codec signatures.  
- Reject disguised, malformed, or tampered video files.

### 2.2 Format Normalization

- Convert unsupported containers to a safe canonical container (MP4 with H.264/AAC) when permitted.  
- Normalize frame rate, color space, and pixel format.  
- Remove unsupported audio tracks, subtitle tracks, and attachments unless explicitly allowed.  
- Ensure deterministic encoding parameters for reproducible outputs.

### 2.3 Policy Enforcement

- Enforce maximum duration and per-file time limits.  
- Enforce maximum resolution, bitrate, and file size.  
- Validate aspect ratio constraints and orientation metadata.  
- Reject corrupted, truncated, or partially downloaded videos.

### 2.4 Metadata Sanitization

- Remove GPS, device identifiers, timestamps, and other EXIF-like metadata.  
- Remove embedded thumbnails, preview frames, and attachments.  
- Strip subtitle tracks and chapters unless explicitly permitted.  
- Normalize or remove non-deterministic metadata fields.

Metadata removal protects user privacy and reduces fingerprinting and covert-channel risks.

---

## 3. Architecture

The Video Sanitizer is implemented as a deterministic, multi‑stage pipeline.  
Each stage performs a focused transformation; outputs are immutable and passed to the next stage.

### 3.1 Processing Stages

1. **Container Validation**  
   - Verify container type (MP4, MKV, MOV, etc.).  
   - Validate header integrity, atom/box structure, and index tables.  
   - Reject malformed or ambiguous containers.

2. **Codec Verification**  
   - Validate video codec (H.264 preferred) and profile compatibility.  
   - Validate audio codec (AAC preferred) and channel layout.  
   - Reject unsupported or proprietary codecs unless conversion is allowed.

3. **Metadata Removal**  
   - Strip GPS, device identifiers, timestamps, and encoder fingerprints.  
   - Remove embedded thumbnails, preview frames, and attachments.  
   - Remove or normalize subtitle and chapter metadata unless allowed.

4. **Security Filtering**  
   - Detect embedded scripts, attachments, or non-media payloads.  
   - Identify steganographic anomalies using heuristic detectors and statistical checks.  
   - Reject videos with suspicious binary patterns or hidden streams.

5. **Policy Enforcement**  
   - Enforce duration, resolution, bitrate, and file size limits.  
   - Validate aspect ratio and orientation.  
   - Reject corrupted, truncated, or partially downloaded files.

6. **Format Normalization**  
   - Recontainerize or transcode to canonical formats (MP4 H.264/AAC) when permitted.  
   - Normalize frame rate (e.g., 24/30/60 fps buckets) and color space (e.g., BT.709).  
   - Normalize pixel format and remove non-deterministic encoder metadata.

7. **Output Encoding**  
   - Re-encode using safe, deterministic encoder settings.  
   - Use fixed GOP, bitrate control mode, and deterministic muxing order.  
   - Produce a sanitized, reproducible output video and a processing report.

Each stage is isolated, testable, and instrumented for observability.

---

### 3.2 Deterministic Output

Sanitizer output must be identical for identical input when the same configuration is used:

- No encoder randomness or non-deterministic ordering.  
- No environment-dependent encoder behavior.  
- No time-dependent metadata embedded in outputs.  
- No heuristic transformations that vary between runs.

Determinism simplifies debugging, caching, and downstream verification.

---

### 3.3 Error Handling

Errors are classified and handled deterministically:

- **Recoverable errors**  
  - Minor container inconsistencies corrected during recontainerization.  
  - Unsupported but convertible codecs → transcode and log.  
  - Missing or malformed metadata → normalize and log.

- **Unrecoverable errors**  
  - Embedded executables or attachments that cannot be safely removed.  
  - Severe corruption or missing critical index structures.  
  - Unbounded duration or resource-exhausting properties.  
  - These result in rejection with a structured rejection reason.

All errors and transformation steps are recorded in a processing report for auditing and analytics.

---

### 3.4 Performance Considerations

The sanitizer is optimized for:

- Streaming-based decoding and re-encoding to minimize memory footprint.  
- Parallelized worker pools for CPU-bound transcode tasks.  
- Linear-time processing relative to input size where possible.  
- Graceful degradation under load (backpressure, queueing, and rate limits).

Instrumentation must expose processing latency, queue depth, rejection rates, and common failure modes.

---

## 4. Security Considerations

The Video Sanitizer is a critical security boundary. Treat all video inputs as hostile.

### 4.1 Threat Model

Protect against:

- Embedded executable payloads and attachments.  
- Script injection via metadata fields.  
- Steganographic channels hidden in video frames or audio streams.  
- Oversized or ultra-high-resolution videos intended to exhaust resources.  
- Corrupted or malformed container structures designed to exploit parsers.  
- Codec spoofing or mixed-codec attacks.  
- Encrypted or obfuscated payloads embedded in frames or streams.

### 4.2 Allowed and Disallowed Content

**Allowed:**  
- MP4 (H.264/AAC) and other containers that can be safely converted.  
- Videos within duration, resolution, and bitrate limits.  
- Deterministic, re-encoded outputs that meet policy.

**Disallowed:**  
- Proprietary or unsupported codecs (unless explicitly enabled and vetted).  
- Files with ambiguous or mismatched MIME types.  
- Videos containing embedded non-media payloads, attachments, or encrypted payloads.  
- Ultra-high-resolution or unbounded bitrate content that violates resource policies.  
- Containers with hidden or alternate streams that cannot be safely inspected.

Disallowed content is removed when safe to do so; otherwise the submission is rejected.

---

### 4.3 Logging and Auditing

The sanitizer logs:

- Rejected videos and structured rejection reasons.  
- Format conversions and transcode events.  
- Metadata removal and normalization events.  
- Policy violations and threshold breaches.  
- Steganography detection alerts and suspicious binary patterns.

Logs are anonymized and structured for analytics, incident response, and compliance.

---

## 5. Observability and Metrics

Instrument the sanitizer to emit metrics such as:

- Processing latency distribution and percentiles.  
- Throughput (videos processed per second).  
- Rejection rate and breakdown by reason.  
- Counts of transcodes, container-only rewraps, and metadata-only sanitizations.  
- Resource usage (CPU, GPU, memory) per task class.  
- Steganography detection counts and false-positive rates.

Expose traces and processing reports to enable cross-correlation with Router and Storage telemetry.

---

## 6. Testing and Validation

- Provide unit tests for each pipeline stage with deterministic fixtures.  
- Maintain a corpus of adversarial test cases (malformed containers, codec spoofing, steganography samples).  
- Run fuzzing and regression tests as part of CI.  
- Validate that identical inputs produce identical outputs across environments and encoder versions (or document version-dependent behavior).

---

## 7. Integration and Deployment Notes

- Use hardware acceleration (GPU/ASIC) where available, but ensure deterministic encoder settings and document any non-deterministic behavior introduced by hardware.  
- Isolate transcode workers in sandboxed environments.  
- Apply strict resource quotas and timeouts per task.  
- Version encoder and muxer toolchains and record versions in processing reports.

---

## 8. Summary

The Video Sanitizer guarantees that video content entering the Emergency Channel is:

- Safe and free of non-media payloads.  
- Deterministic and reproducible when processed with the same configuration.  
- Policy-compliant with enforced duration, resolution, and bitrate limits.  
- Stripped of sensitive metadata and covert channels.  
- Encoded into a stable, auditable format ready for storage and distribution.

It is a foundational component of the Emergency Channel’s content safety, privacy, and reliability model.
