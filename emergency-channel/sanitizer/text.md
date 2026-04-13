# Emergency Channel — Text Sanitizer

## 1. Purpose

The Text Sanitizer ensures that all incoming textual content is safe, valid, and compliant with the Emergency Channel’s publishing rules.  
It removes harmful elements, normalizes formatting, extracts minimal metadata required by downstream systems, and prepares content for routing, storage, and distribution.

The sanitizer must operate deterministically, produce predictable transformations, and avoid ambiguity or lossy heuristics that could change meaning.

---

## 2. Responsibilities

### 2.1 Content Safety

- Remove or neutralize embedded scripts and executable code fragments.  
- Strip dangerous HTML elements and attributes that can trigger execution.  
- Neutralize payloads used for XSS, SQL/command injection, or other injection classes.  
- Detect and block obfuscated or encoded payloads intended to bypass filters.

### 2.2 Encoding and Normalization

- Convert all input to UTF-8.  
- Normalize line endings and whitespace.  
- Normalize punctuation and canonicalize common character variants.  
- Remove invisible control characters and BOM markers.

### 2.3 Structural Normalization

- Preserve structural markers (headings, lists, blockquotes) while removing unsupported or unsafe markup.  
- Flatten or normalize nested formatting to a stable, deterministic representation.  
- Convert allowed lightweight markup (basic Markdown subset) into a canonical internal representation.

### 2.4 Policy Enforcement

- Enforce maximum content length and per-field limits.  
- Validate allowed MIME types and reject mixed or ambiguous formats.  
- Apply language-specific rules and content policies where required.  
- Enforce domain rules (for example: allowed HTML subset, permitted links, or banned domains).

### 2.5 Metadata Extraction

- Extract minimal metadata useful for routing and display: title, summary, language, and structural markers.  
- Provide deterministic extraction results (same input → same metadata).  
- Avoid extracting or retaining PII unless explicitly required and authorized.

---

## 3. Architecture

The Text Sanitizer is a deterministic, modular pipeline. Each stage performs a focused transformation; outputs are immutable and passed to the next stage.

### 3.1 Processing Stages

1. **Input Normalization**  
   - Detect and convert character encodings to UTF-8.  
   - Normalize line endings and remove BOM.  
   - Reject inputs with unsupported encodings or binary/mixed content.

2. **Lexical Cleaning**  
   - Remove control characters and non-printable codepoints.  
   - Normalize whitespace and collapse excessive runs.  
   - Replace problematic Unicode sequences with safe equivalents.

3. **Markup and Structural Sanitization**  
   - Parse allowed markup (basic Markdown, safe HTML subset) into a canonical AST.  
   - Remove disallowed tags and attributes (for example: iframe, embed, script, style, inline event handlers).  
   - Flatten or normalize nested constructs to a deterministic form.

4. **Content Filtering**  
   - Detect and remove malicious payloads (XSS vectors, injection patterns, obfuscated scripts).  
   - Validate and sanitize URLs and link targets; optionally rewrite or remove disallowed links.  
   - Apply domain and policy checks (banned words, prohibited domains, content length).

5. **Policy Enforcement and Truncation**  
   - Enforce length and quota limits; apply deterministic truncation rules when needed.  
   - Validate language and charset constraints.  
   - Reject content that cannot be safely sanitized.

6. **Metadata Extraction and Annotation**  
   - Extract title, summary, language, and structural markers.  
   - Annotate sanitized output with deterministic metadata for downstream consumers.

7. **Output Generation**  
   - Emit sanitized text in a canonical, stable encoding and format.  
   - Include a processing report describing applied transformations and any policy violations.

Each stage is independently testable and instrumented for observability.

---

### 3.2 Deterministic Output

Sanitizer output must be identical for identical input:

- No randomness or non-deterministic ordering.  
- No environment-dependent behavior (fonts, locale, time).  
- No time-dependent metadata embedded in outputs.  
- No heuristic rewriting that can vary between runs.

Determinism guarantees reproducible downstream behavior and simplifies debugging.

---

### 3.3 Error Handling

Errors are classified and handled deterministically:

- **Recoverable errors**  
  - Invalid characters or minor encoding issues → corrected and logged.  
  - Unsupported but convertible markup → converted to canonical form.  
  - Missing metadata → best-effort extraction.

- **Unrecoverable errors**  
  - Embedded payloads that cannot be safely removed.  
  - Mixed binary/text formats or corrupted encodings.  
  - Policy violations that mandate rejection (e.g., banned content).  
  - These cause rejection with a structured rejection reason.

All errors and transformation steps are recorded in a processing report for auditing and analytics.

---

### 3.4 Performance Considerations

The Text Sanitizer is optimized for:

- Low latency and minimal added processing time.  
- Linear-time processing relative to input size.  
- Minimal memory overhead; streaming where possible.  
- High throughput and graceful degradation under load.

Instrumentation must expose processing time, rejection rates, and common failure modes.

---

## 4. Security Considerations

The Text Sanitizer is a primary defense against content-borne attacks. Treat all text as untrusted.

### 4.1 Threat Model

Protect against:

- Script injection and XSS vectors.  
- Markup-based attacks (malicious attributes, data URLs).  
- Unicode attacks (homoglyphs, bidirectional override).  
- Obfuscated or encoded payloads intended to bypass filters.  
- Oversized or resource-exhausting payloads.  
- Phishing links and malicious redirects.

### 4.2 Allowed and Disallowed Content

**Allowed:**  
- Plain text and UTF-8 content.  
- Basic Markdown and a restricted, safe subset of HTML.  
- Deterministic, canonicalized outputs within policy limits.

**Disallowed:**  
- Executable scripts and inline event handlers (for example: attributes like onclick=).  
- Iframes, embeds, and external script references.  
- Binary or mixed-format payloads masquerading as text.  
- Obfuscated or encoded payloads that cannot be safely decoded and validated.

Disallowed content is removed when safe to do so; otherwise the submission is rejected.

---

### 4.3 Logging and Auditing

Log entries include:

- Rejected submissions and rejection reasons.  
- Sanitization actions (tags/attributes removed, truncation applied).  
- Detected malicious patterns and suspicious encodings.  
- Metadata extraction results and any anomalies.

Logs are anonymized and structured to support analytics, incident response, and compliance reviews.

---

## 5. Observability and Metrics

Instrument the sanitizer to emit metrics such as:

- Processing latency distribution.  
- Throughput (items/sec).  
- Rejection rate and rejection reasons breakdown.  
- Counts of specific sanitization actions (tags removed, links rewritten).  
- Error rates by category.

Expose traces and processing reports to enable cross-correlation with Router and Storage telemetry.

---

## 6. Testing and Validation

- Provide unit tests for each pipeline stage with deterministic fixtures.  
- Maintain a corpus of adversarial test cases (XSS, Unicode attacks, obfuscated payloads).  
- Run fuzzing and regression tests as part of CI.  
- Validate that identical inputs always produce identical outputs across environments.

---

## 7. Summary

The Text Sanitizer guarantees that textual content entering the Emergency Channel is:

- Safe and free of executable payloads.  
- Deterministic and reproducible.  
- Policy-compliant and properly annotated.  
- Instrumented for observability and auditable for security.

It is a core component of the Emergency Channel’s content safety and reliability model.
