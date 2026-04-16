
# Camouflage Layer

This layer defines the protocol-level and traffic-shape techniques that make Empus access indistinguishable from legitimate Internet traffic. It covers TLS/QUIC fingerprint shaping, SNI strategies, handshake obfuscation, real-site behavior templates, and traffic normalization used to resist DPI, active probing, and protocol classification.

---

## Purpose and scope

**Purpose:** enable covert, robust access by ensuring client↔server flows resemble benign services while remaining compatible with Empus entrypoints and session initialization.

**Scope:** design and implementation guidance for:
- TLS/QUIC ClientHello shaping and fingerprint selection  
- SNI rotation and domain selection policies  
- Handshake obfuscation primitives and validation vectors  
- Real-site mimicry templates (certificates, HTTP/3 behavior, timing)  
- Traffic pattern normalization (packet sizes, pacing, stream behavior)  
- Local test vectors and smoke tests for CI

This directory contains high-level strategy plus concrete artifacts for implementation, testing, and review.

---

## Key responsibilities

- **Fingerprint shaping:** provide selectable TLS/QUIC fingerprints and safe mapping rules to client profiles.  
- **Handshake obfuscation:** define obfuscation transforms, replay protection, and verification vectors.  
- **SNI management:** define rotation, randomness bounds, and domain selection constraints.  
- **Real-site mimicry:** maintain a small, auditable template library for certificate and HTTP/QUIC behavior.  
- **Traffic normalization:** specify packet/stream size distributions and timing profiles to match target services.  
- **Testability:** supply minimal reproducible examples and smoke tests for CI and local validation.  
- **Auditability:** document risks, allowed deviations, and logging constraints.

---

## Files (simplified layout)

```
camouflage/
├── README.md
├── camouflage.md                # High-level strategy and design principles
├── tls-fingerprint.md           # Fingerprint sets, selection rules, risks
├── sni-randomization.md         # SNI rotation policy and parameters
├── handshake-obfuscation.md     # Obfuscation transforms and test vectors
├── real-site-mimicry.md         # Templates for certs, HTTP/QUIC behavior
├── examples/
│   └── minimal-handshake.md     # Minimal reproducible handshake example
└── tests/
    └── smoke-handshake.md       # CI smoke test: handshake success/fallback
```

Each file contains a short summary at the top and a clear “what to change” section for implementers.

---

## Minimal run example (quick verification)

**Purpose:** locally verify a shaped ClientHello and a successful obfuscated handshake.

**Steps (high level):**
1. Select a `client-profile` (e.g., `chrome-115`) from `tls-fingerprint.md`.  
2. Apply SNI from `sni-randomization.md` (example domain).  
3. Run the minimal handshake script in `examples/minimal-handshake.md`.  

**Example snippet (pseudo):**
```bash
# choose profile and target
PROFILE=chrome-115
SNI=example-mimic.com
# run minimal handshake (script provided in examples/)
./examples/run_minimal_handshake.sh --profile $PROFILE --sni $SNI --target 127.0.0.1:4433
```

The script emits a pass/fail summary and a short trace suitable for CI smoke tests.

---

## Tests and CI

- **smoke-handshake.md**: single-step test that validates:
  - shaped ClientHello accepted by a test entrypoint  
  - handshake obfuscation round-trip correctness  
  - fallback activation when primary path blocked

CI should run smoke tests on PRs that modify camouflage artifacts. Tests must be deterministic and fast.

---

## Security and operational notes

- **No raw payload logging:** only hashes/references allowed for debugging.  
- **Key handling:** private keys used for mimicry must be managed per ops policy; never commit keys.  
- **Risk disclosure:** document which fingerprints/templates are high-risk and require rotation.  
- **Compatibility:** all camouflage transforms must be validated against `session-init` to avoid breaking key exchange.  
- **Auditability:** include a short changelog in each file when templates or fingerprints change.

---
