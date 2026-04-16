# Smoke Handshake Test

**Purpose**  
Provide a deterministic smoke test to validate that a shaped ClientHello and obfuscated handshake are accepted by a test entrypoint. This test is designed for CI pipelines and quick local verification.

---

## Test Scope
- **Handshake acceptance** — shaped ClientHello must be accepted by the entrypoint.  
- **Obfuscation correctness** — obfuscation transforms must complete a round-trip without error.  
- **Fallback behavior** — if primary path is blocked, fallback profile must activate.  
- **Error normalization** — invalid clients must receive browser-like error responses.

---

## Test Procedure
1. **Setup**: start a local test entrypoint on `127.0.0.1:4433` with camouflage transforms enabled.  
2. **Client probe**: send a shaped ClientHello using a reference profile (e.g., `chrome-115`).  
3. **Handshake validation**: confirm ServerHello and certificate chain match mimicry templates.  
4. **Obfuscation check**: ensure padding, delay, and fragmentation are applied as defined in `handshake-obfuscation.md`.  
5. **Fallback test**: simulate blocked primary SNI and verify fallback profile activates.  
6. **Error test**: send malformed ClientHello and confirm normalized error response.

---

## Expected Output
- **PASS** — handshake accepted, obfuscation transforms applied, fallback works, error normalized.  
- **FAIL** — handshake rejected, transforms missing, fallback not triggered, or error response anomalous.  

Logs must include explicit `PASS` / `FAIL` markers for CI parsing.

---

## CI Integration
- Run this test automatically on PRs that modify camouflage artifacts.  
- Keep runtime under 30 seconds.  
- Fail the pipeline only on explicit `FAIL` markers.  
- Store handshake traces for offline review.

---

## Notes
This smoke test is minimal and deterministic. It is not intended to cover all edge cases; extended integration tests can be added after funding.
