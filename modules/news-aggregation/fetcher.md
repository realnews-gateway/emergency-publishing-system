
# Fetcher: Content Retrieval

The fetcher is responsible for retrieving content from all registered news sources.  
It runs on servers located in free‑network regions (e.g. Japan, Singapore), ensuring reliable ingestion of foreign sources that may be blocked in certain countries.  
The fetcher implements retries, mirror rotation, and region‑aware source selection defined by **region-config.md**.

---

## Objectives

The fetcher provides:

- Reliable content retrieval from diverse sources  
- Automatic retries with exponential backoff  
- Mirror rotation for failing sources  
- Region‑aware fetching strategies defined by **region-config.md**  
- Minimal metadata leakage  
- Integration with parser and deduplication modules  

---

## Fetching Pipeline

### 1. Source Selection
- Selects a source from the region‑config.md set  
- Prioritizes high‑reliability sources  
- Falls back to mirrors if the primary source fails  

### 2. Request Execution
- Sends HTTP(S) requests with minimal headers  
- Avoids identifiable patterns  
- Applies rate limiting and jitter  
- Handles redirects and content negotiation  

### 3. Response Handling
- Validates content type  
- Extracts raw HTML or XML  
- Passes content to the parser module  

---

## Request Behavior

### Headers
- User‑Agent: Platform‑specific (from client‑profiles)  
- Accept: text/html, application/xml  
- Accept‑Language: Region‑specific  
- No tracking headers, no custom identifiers  

### Timing
- Randomized jitter  
- Retry backoff: 1s → 2s → 4s → 8s  

### Connection Behavior
- Reuses connections when possible  
- Rotates TLS fingerprints based on platform  

---

## Error Handling

- **Network Errors**: timeout → retry; reset → retry with mirror; DNS failure → switch to mirror/CDN  
- **HTTP Errors**: 429 → backoff; 503 → retry with mirror; 404 → mark source unstable  
- **Parsing Errors**: pass to parser; unrecoverable → mark source low reliability  

---

## Mirror Rotation

When a source fails:  
1. Retry with same transport  
2. Retry with mirror #1  
3. Retry with mirror #2  
4. Switch to CDN‑backed mirror  
5. Temporarily disable source  

Disabled sources are periodically rechecked.

---

## Integration with Other Modules

The fetcher integrates with:

- **sources.md** — Provides URLs, mirrors, and region‑aware source sets  
- **parser.md** — Receives raw HTML/XML for normalization  
- **deduplication.md** — Receives structured content for duplicate detection  
- **classification.md** — Receives normalized content for topic classification  
- **region-config.md** — Mandatory file defining which foreign sources are blocked in specific countries  

---

## Summary

The fetcher provides reliable content retrieval using retries, mirror rotation, and region‑aware source selection.  
Running in free‑network regions (Japan, Singapore), it directly fetches foreign sources identified by **region-config.md** as blocked in certain countries, ensuring continuous ingestion of high‑value content.
