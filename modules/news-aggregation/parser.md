
# Parser: Content Normalization and Metadata Extraction

The parser converts raw HTML, XML, or JSON content into a clean, structured internal format.  
It extracts metadata, removes noise, normalizes timestamps, and prepares articles for deduplication and classification.  
The parser is designed to handle heterogeneous content from diverse sources, including malformed HTML, inconsistent metadata, and region-specific formatting.  
Importantly, the parser preserves original source attribution and author information to maintain copyright integrity.  

---

## Objectives

The parser provides:

- Robust HTML/XML parsing  
- Metadata extraction (title, timestamp, author, tags, source attribution)  
- Clean text extraction from article bodies  
- Removal of tracking parameters and boilerplate  
- Language detection and normalization  
- Timestamp normalization across time zones  
- Structured output for downstream modules  

These capabilities ensure consistent and high-quality content for the aggregation pipeline while preserving copyright and author attribution.  

---

## Parsing Pipeline

The parsing pipeline consists of four stages:

### 1. Content Type Detection
- Detects HTML, XML, JSON, or RSS formats  
- Identifies encoding (UTF-8, GBK, Shift-JIS, etc.)  
- Handles malformed or mixed-encoding content  

### 2. Metadata Extraction
- Extracts `<title>`, `<meta>`, `<h1>`, and OpenGraph fields  
- Parses timestamps from multiple formats  
- Extracts author names when available  
- Extracts tags or categories from RSS or HTML  
- Preserves original source attribution and copyright metadata  

### 3. Body Extraction and Cleaning
- Removes navigation bars, ads, comments, and scripts  
- Extracts main article text using DOM heuristics  
- Removes tracking parameters from URLs  
- Normalizes whitespace and punctuation  

### 4. Normalization and Structuring
- Converts timestamps to UTC  
- Normalizes language codes  
- Converts HTML entities  
- Produces a structured internal article format  

---

## Structured Article Format

The parser outputs a normalized article object:

```
article:
  id: Unique identifier
  source_id: Source registry ID
  url: Canonical URL
  title: Cleaned title text
  author: Extracted or inferred author (preserved for copyright attribution)
  timestamp: UTC timestamp
  language: ISO language code
  tags: List of extracted tags
  body: Cleaned article text
  raw_html: Optional raw content for debugging
  copyright: Original source attribution metadata
```

This format is consumed by deduplication and classification modules, ensuring that copyright and author information remain intact.  

---

## Metadata Extraction Details

The parser extracts metadata from:

### HTML
- `<title>`  
- `<meta name="description">`  
- `<meta property="og:title">`  
- `<meta property="article:published_time">`  
- `<time>` elements  
- Author and copyright fields (preserved)  

### RSS/XML
- `<item><title>`  
- `<item><pubDate>`  
- `<item><category>`  
- `<item><description>`  
- Source attribution fields (preserved)  

### JSON APIs
- `title`  
- `published_at`  
- `tags`  
- `author`  
- `copyright` / source attribution (preserved)  

Fallback strategies ensure metadata is extracted even when fields are missing or malformed.  

---

## Body Extraction

The parser uses multiple heuristics:

### DOM-Based Extraction
- Identifies main content blocks  
- Removes sidebars, ads, comments, and scripts  

### Text Density Analysis
- Detects high-density text regions  
- Filters out boilerplate  

### Tag-Based Filtering
- Removes `<script>`, `<style>`, `<nav>`, `<footer>`, `<aside>`  
- Preserves `<p>`, `<h1>`, `<h2>`, `<blockquote>`, `<img alt>`  

### URL Cleaning
- Removes tracking parameters (`utm_*`, `fbclid`, etc.)  
- Normalizes canonical URLs  

---

## Language Detection

The parser determines language using:

- Source metadata  
- HTML `lang` attributes  
- Statistical language detection  
- Unicode range heuristics  

Language is normalized to ISO 639-1 codes.  

---

## Timestamp Normalization

The parser:

- Extracts timestamps from multiple formats  
- Converts local timestamps to UTC  
- Handles ambiguous or missing time zones  
- Falls back to fetch time when necessary  

This ensures consistent ordering across sources.  

---

## Error Handling

The parser handles errors gracefully:

### Recoverable Errors
- Malformed HTML → fallback parser  
- Missing metadata → infer from content  
- Encoding errors → re-decode with fallback encodings  

### Unrecoverable Errors
- Empty content  
- Non-article pages  
- Repeated parsing failures  

Unrecoverable errors lower the source reliability score.  

---

## Integration with Other Modules

The parser integrates with:

- **fetcher.md** — Receives raw HTML/XML/JSON content  
- **deduplication.md** — Provides normalized text for similarity detection  
- **classification.md** — Provides structured metadata and body text  
- **sources.md** — Uses source metadata for parsing hints  
- **region-config.md** — Mandatory file ensuring reliable parsing of foreign sources blocked in specific countries  

---

## Summary

The parser converts raw content into a clean, structured article format suitable for deduplication and classification.  
By handling diverse formats, malformed content, and inconsistent metadata, and integrating **region-config.md** for blocked foreign sources, it ensures reliable and high-quality input for the news aggregation pipeline.  
Crucially, the parser preserves original source attribution and author information, maintaining copyright integrity while normalizing content.
