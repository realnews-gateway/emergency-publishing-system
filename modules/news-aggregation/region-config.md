
# Region Configuration

The region configuration defines which foreign sources are blocked in specific countries.  
It is a mandatory file that guides the fetcher and deduplication subsystems, ensuring that high‑value foreign sources under censorship are prioritized and preserved.  
This document references **source IDs** from sources.md instead of duplicating their attributes.

---

## Objectives

The region configuration provides:

- Explicit lists of foreign sources blocked in each country (by source_id)  
- Guidance for the fetcher to select region‑aware source sets  
- Rules for deduplication to prioritize censored high‑value sources  
- Integration with parser, classification, and publisher modules  

---

## Directory Structure

Region configuration is maintained as a single file **region-config.md**, with structured sections for each country.  
Example layout:

```
region-config.md
  ├── china
  ├── iran
  ├── russia
  ├── custom-regions
```

---

## Example Configuration

```
china:
  blocked_sources:
    - "global_reuters"
    - "global_bbc"
    - "global_nytimes"
  notes: "High-value foreign sources frequently censored in CN"

iran:
  blocked_sources:
    - "regional_dw"
    - "regional_aljazeera"
    - "regional_bbc_persian"
  notes: "Foreign Persian-language sources blocked in IR"

russia:
  blocked_sources:
    - "regional_meduza"
    - "regional_bbc_russian"
    - "regional_rferl"
  notes: "Independent Russian-language sources blocked in RU"
```

---

## Integration with Other Modules

The region configuration integrates with:

- **fetcher.md** — Guides region‑aware source selection  
- **parser.md** — Ensures reliable parsing of censored sources  
- **deduplication.md** — Prioritizes censored high‑value sources during duplicate handling  
- **classification.md** — Provides region context for topic classification  
- **sources.md** — Supplies source IDs and attributes; region-config.md only references IDs  

---

## Summary

The region configuration is a mandatory file that defines which foreign sources are blocked in specific countries.  
By referencing source IDs from **sources.md**, it avoids duplication of attributes while guiding fetcher, parser, deduplication, and classification modules to ensure censored high‑value sources are reliably ingested and preserved.
