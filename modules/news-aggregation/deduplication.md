# Deduplication and Content Clustering

The deduplication subsystem identifies duplicate or near‑duplicate articles across heterogeneous news sources.  
Because many sources syndicate, mirror, or rewrite the same content, deduplication is essential for reducing noise, improving classification accuracy, and ensuring that downstream modules receive clean, non‑redundant data in the Empus system.

---

## Objectives

The deduplication subsystem provides:

- Exact duplicate detection using hashing  
- Near‑duplicate detection using similarity metrics  
- Clustering of related articles  
- Canonical article selection  
- Integration with classification and publisher modules  

---

## Deduplication Pipeline

### 1. Exact Duplicate Detection
- Uses normalized article text  
- Computes stable content hashes (e.g., SHA‑256)  
- Detects identical or trivially modified content  

### 2. Near‑Duplicate Detection
- Uses similarity metrics (Jaccard, cosine similarity, MinHash, embeddings)  
- Detects rewritten or partially modified articles  
- Handles syndicated and mirrored content  

### 3. Clustering and Canonicalization
- Groups related articles into clusters  
- Selects a canonical representative based on source reliability and metadata completeness  
- Assigns cluster IDs for downstream processing  

---

## Handling Multi‑Language Content

- Articles grouped by language within clusters  
- Each language group receives its own canonical article  
- Cross‑language similarity optional and disabled by default  

---

## Performance Considerations

- **Scalability**: LSH reduces comparisons from O(n²) to O(n log n)  
- **Efficiency**: Compact MinHash signatures, constant‑time hashing  
- **Incremental Updates**: New articles compared only against recent clusters  

---

## Integration with Other Modules

The deduplication subsystem integrates with:

- **parser.md** — Receives normalized article text  
- **classification.md** — Receives canonical articles for topic classification  
- **publisher/** — Receives deduplicated content for distribution  
- **emergency-channel/** — Receives only canonical, verified articles  
- **sources.md** — Uses source reliability to prioritize canonical selection  
- **region-config.md** — Mandatory file that guides deduplication logic, ensuring that when handling duplicate content, high‑value foreign sources blocked in specific countries are prioritized and preserved  

---

## Summary

The deduplication subsystem ensures that the Empus news aggregation pipeline produces clean, non‑redundant content.  
By combining hashing, similarity detection, and clustering, it identifies both exact and near‑duplicate articles and selects canonical representatives for downstream processing.
