# Security Policy

The Emergency Publishing System is designed for hostile environments with strong adversaries.  
Security is a core requirement for all development, deployment, and review activities.

---

## Reporting a Vulnerability

If you discover a security issue, please report it privately.

Guidelines:
- Do not open a public issue  
- Do not disclose details publicly  
- Provide a clear description of the issue  
- Include reproduction steps if possible  
- Anonymous reports are welcome  

When reporting, avoid including sensitive logs, metadata, or region-identifying information.

---

## Responsible Disclosure

We follow responsible disclosure principles:
- Issues are reviewed promptly  
- Severity determines prioritization  
- Fixes are developed privately  
- Public disclosure occurs only after mitigation  
- Sensitive details may be withheld for user safety  

---

## Scope

Security issues may include:
- Metadata leakage  
- Transport fingerprinting  
- Sanitization bypass  
- Routing correlation  
- Storage vulnerabilities  
- Region-specific risks  
- Dependency-related risks  
- Architectural inconsistencies that weaken safety  

---

## Out of Scope

The following are generally out of scope:
- Issues caused by compromised user devices  
- Network-level attacks outside our control  
- Third-party service outages  
- Attacks requiring unrealistic adversary capabilities  

---

## Security Expectations for Contributors

All contributions must consider:
- Metadata minimization  
- Avoiding fingerprintable behavior  
- Region-agnostic design  
- Correct sanitization and validation  
- No hard-coded endpoints  
- No debug logs in production paths  
- No assumptions about censorship models  

If unsure whether a change introduces risk, ask before submitting.

---

## Security Review Process

Every PR undergoes:
- Security review  
- Architecture review  
- Documentation review (if applicable)  

Reviewers may request changes to ensure safety and consistency.

---

## Commitment

We are committed to:
- Protecting users in high-risk regions  
- Minimizing metadata  
- Ensuring safe communication  
- Maintaining a secure architecture  
- Responding quickly to credible reports  

Security is a continuous process, and we appreciate all responsible contributions.
