
# Classification and Source Scoring

The classification subsystem assigns topic labels, categories, and metadata to normalized articles.  
It also evaluates source reliability and content quality, producing scores that influence downstream publishing and emergency workflows in the Empus system.

---

## Objectives

The classification subsystem provides:

- Topic classification (politics, world, economy, tech, etc.)  
- Multi-label tagging  
- Language-aware classification  
- Source reliability scoring  
- Content quality scoring  
- Cluster-level classification for deduplicated articles  

---

## Classification Pipeline

### 1. Preprocessing
- Receives canonical articles from deduplication  
- Normalizes text for classification  
- Removes boilerplate and noise  
- Detects language and applies language-specific models  

### 2. Topic Classification
- Assigns one or more topic labels  
- Supports rule-based, statistical, or ML-based classifiers  
- Multi-label classification supported  

### 3. Tag Extraction
- Extracts keywords from titles and body text  
- Normalizes tags across languages  
- Uses TF-IDF, RAKE, embeddings, or NER  

### 4. Source and Content Scoring
- Computes reliability and quality scores  
- Adjusts based on historical performance  
- Produces rankings for downstream modules  

---

## Topics

Standard topic set includes:

- Politics  
- World  
- Economy  
- Technology  
- Society  
- Health  
- Environment  
- Culture  
- Science  
- Breaking News  

---

## Source Reliability Scoring

Reliability is based on:

- Availability (fetch success, mirror performance)  
- Content quality (parsing success, metadata completeness, duplicate ratio)  
- Historical stability (outages, update consistency)  

Scores range from 0.0 to 1.0 and influence prioritization and emergency publishing decisions.

---

## Content Quality Scoring

Articles are scored by:

- Body length  
- Metadata completeness  
- Parsing confidence  
- Language detection confidence  
- Duplicate cluster size  

High-quality articles are prioritized for publishing.

---

## Cluster-Level Classification

- Canonical article receives full classification  
- Cluster-level tags aggregated  
- Topic labels propagated to all members  
- Source reliability influences ranking  

---

## Integration with Other Modules

The classification subsystem integrates with:

- **parser.md** — Receives normalized text and metadata  
- **deduplication.md** — Provides canonical articles for classification  
- **publisher/** — Uses topics and tags for routing and prioritization  
- **emergency-channel/** — Identifies urgent or high-impact content  
- **sources.md** — Updates source reliability scores  
- **region-config/** — Ensures classification respects country-specific blocked content handling  

---

## Summary

The classification subsystem assigns topics, extracts tags, and computes reliability and quality scores for aggregated news content.  
By combining rule-based, statistical, and embedding-based methods, it produces structured metadata that drives publishing, prioritization, and emergency workflows across Empus.
