# Emergency Channel — Document Sanitizer

## 1. Purpose

The Document Sanitizer ensures that all incoming document files (PDF, DOCX, PPTX, spreadsheets, archives, etc.) are safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes active content, strips metadata, validates structure, enforces size limits, and converts documents into safe, standardized formats for downstream processing.

The sanitizer must operate deterministically and must never execute embedded scripts or macros.

---

## 2. Responsibilities

### 2.1 Security Filtering

- Remove or block embedded scripts, macros, and executable payloads  
- Detect malicious or obfuscated content  
- Validate container integrity (ZIP‑based formats like DOCX/PPTX)  
- Reject encrypted or password‑protected documents  

### 2.2 Format Normalization

- Convert documents to safe, standardized formats (e.g., PDF/A or plain text)  
- Normalize fonts and embedded resources  
- Remove unsupported or proprietary extensions  
- Ensure deterministic rendering  

### 2.3 Policy Enforcement

- Enforce maximum file size  
- Enforce page count limits  
- Validate allowed MIME types  
- Reject corrupted or partially downloaded files  

### 2.4 Metadata Sanitization

- Remove author, device, and software identifiers  
- Remove timestamps and revision history  
- Remove embedded thumbnails or previews  
- Strip application‑specific metadata blocks  

Metadata removal protects user privacy and reduces fingerprinting risks.

---

## 3. Architecture

The Document Sanitizer is implemented as a deterministic, multi‑stage pipeline.  
Each stage performs a specific transformation, and the output of one stage becomes the input of the next.

### 3.1 Processing Stages

The sanitizer pipeline consists of:

1. **Container Validation**  
   - Verify container type (PDF, DOCX, PPTX, ODT, etc.)  
   - Validate ZIP‑based structures for Office formats  
   - Reject malformed or ambiguous containers  

2. **Active Content Removal**  
   - Strip macros (VBA, VBS, JS)  
   - Remove embedded scripts or automation triggers  
   - Remove active forms, JavaScript, and interactive elements  
   - Block embedded executables or OLE objects  

3. **Metadata Removal**  
   - Remove author, device, and software identifiers  
   - Remove timestamps, revision history, and comments  
   - Remove embedded thumbnails and previews  
   - Strip application‑specific metadata blocks  

4. **Security Filtering**  
   - Detect obfuscated or encrypted payloads  
   - Identify suspicious binary patterns  
   - Reject password‑protected or encrypted documents  
   - Reject documents with hidden or alternate streams  

5. **Policy Enforcement**  
   - Enforce file size limits  
   - Enforce page count limits  
   - Validate allowed MIME types  
   - Reject corrupted or partially downloaded files  

6. **Format Normalization**  
   - Convert to PDF/A or plain text when possible  
   - Normalize fonts and embedded resources  
   - Remove unsupported or proprietary extensions  
   - Ensure deterministic rendering  

7. **Output Generation**  
   - Produce a sanitized, stable output document  
   - Ensure deterministic layout and encoding  
   - Guarantee reproducible results across environments  

Each stage is isolated and independently testable.

---

### 3.2 Deterministic Output

The sanitizer must always produce the same output for the same input:

- No environment‑dependent rendering  
- No random font substitution  
- No time‑dependent metadata  
- No heuristic‑based rewriting that varies between runs  

Determinism ensures predictable downstream processing and reproducible results.

---

### 3.3 Error Handling

Errors are categorized into:

- **Recoverable errors**  
  - Minor metadata corruption  
  - Missing or invalid ZIP entries  
  - Unsupported but convertible formats  
  - These are corrected automatically  

- **Unrecoverable errors**  
  - Encrypted or password‑protected documents  
  - Embedded executables or macros that cannot be safely removed  
  - Severe corruption or malformed structures  
  - These result in rejection  

All errors are logged for analytics and debugging.

---

### 3.4 Performance Considerations

The sanitizer is optimized for:

- Streaming‑based parsing  
- Low memory usage  
- Linear‑time processing  
- High throughput under load  

It must handle large volumes of documents without blocking the pipeline.

---

## 4. Security Considerations

The Document Sanitizer is one of the most critical security boundaries in the Emergency Channel.  
Documents are the most common vector for embedded malware, macros, and hidden payloads, and must be treated as fully untrusted.

### 4.1 Threat Model

The sanitizer protects against:

- Embedded macros (VBA, JS, VBS)  
- OLE objects containing executables  
- Encrypted or password‑protected payloads  
- Obfuscated or compressed malicious streams  
- PDF JavaScript, forms, and interactive elements  
- Malformed ZIP containers (DOCX/PPTX)  
- Hidden alternate streams or attachments  
- Steganographic channels inside embedded images  

All document inputs are considered hostile by default.

---

### 4.2 Allowed and Disallowed Content

**Allowed:**

- PDF/A  
- Plain text  
- Sanitized DOCX/PPTX converted to PDF/A  
- Documents within size and page count limits  
- Deterministic, re‑encoded outputs  

**Disallowed:**

- Password‑protected or encrypted documents  
- Documents containing macros or scripts  
- Embedded executables or OLE objects  
- Proprietary or unsupported formats  
- Files with ambiguous or mismatched MIME types  
- Documents with hidden or alternate streams  
- Archives (ZIP/RAR/7z) unless explicitly permitted  

Disallowed content is removed or the entire submission is rejected.

---

### 4.3 Logging and Auditing

The sanitizer logs:

- Rejected documents  
- Macro removal events  
- Metadata stripping  
- Format conversions  
- Policy violations  
- Suspicious binary patterns  
- Hidden stream detection alerts  

Logs are anonymized and used for analytics, debugging, and threat monitoring.

---

## 5. Summary

The Document Sanitizer ensures that all document content entering the Emergency Channel is:

- Safe  
- Free of active content  
- Deterministic  
- Policy‑compliant  
- Stripped of metadata  
- Converted into a stable, reproducible format  
- Ready for storage and distribution  

It is a foundational component of the system’s security, privacy, and reliability.
