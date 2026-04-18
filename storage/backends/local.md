
# Local Storage

Local storage provides simple, fast persistence on individual nodes.  
It is primarily used for testing, prototyping, and small‑scale deployments.

---

## Characteristics
- Low latency read/write operations  
- Easy setup and maintenance  
- Limited durability (single‑node risk)  
- Not suitable for censorship‑resistant or large‑scale environments  

---

## Integration
- Acts as fallback when distributed backends are unavailable  
- Provides baseline persistence for development environments  
- Works with **router.md** for backend selection
