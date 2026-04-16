### Minimal Handshake Example

**Purpose**
Validate a shaped ClientHello and a minimal obfuscated handshake locally or in CI as a deterministic smoke test for review and funding materials.

---

### Prerequisites
- Local test entrypoint (e.g., `127.0.0.1:4433`) or equivalent simulated environment.
- Basic tooling: `openssl` or a test harness capable of sending a custom ClientHello.
- Reference profile names documented in `tls-fingerprint.md` (e.g., `chrome-115`).

---

### Run Steps (minimum)
1. **Choose profile and SNI** — e.g., `chrome-115` and `example-mimic.com`.
2. **Send probe** — use `openssl s_client` or an equivalent script to send a shaped ClientHello.
3. **Inspect result** — determine acceptance or rejection from the probe output.

**Example command**
```bash
PROFILE=chrome-115
SNI=example-mimic.com
TARGET=127.0.0.1:4433

echo | openssl s_client -servername "$SNI" -connect "$TARGET" -alpn h2,http/1.1 -brief -timeout 5
```

---

### Expected outputs (CI-friendly)
- **PASS** — handshake accepted; output includes `Verify return code: 0 (ok)` or a custom success marker.
- **WARN** — handshake established but certificate/ALPN/fingerprint mismatches require manual review.
- **FAIL** — handshake rejected or protocol error; CI should mark the job failed.

Ensure logs explicitly include `PASS` / `WARN` / `FAIL` for automated parsing.

---

### Quick troubleshooting
- **Certificate errors** — verify certificate chain and alignment with `real-site-mimicry` guidance.
- **Fingerprint mismatch** — confirm ClientHello parameters match the chosen profile.
- **Timeouts or resets** — check whether delay/padding strategies are causing middlebox drops.
- **SNI inconsistencies** — ensure SNI aligns with certificate/template or allowed front domain.

---

### CI note (optional)
Not required for funding submission; if automated, add a lightweight job that runs the single probe and fails only on explicit `FAIL` markers.

---

### Reviewer note
This is a minimal, reproducible demonstration intended to show feasibility. Full test harnesses, deterministic generators, and additional profiles can be added after funding.
