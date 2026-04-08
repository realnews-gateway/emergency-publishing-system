# Contributing Guide

Thank you for your interest in contributing to the Emergency Publishing System.  
This project is designed for high-risk environments and requires careful attention to security, documentation quality, and architectural consistency.

---

## Contribution Principles

- **Security first**  
- **Minimal metadata**  
- **Consistent architecture**  
- **Clear documentation**  
- **Region-aware behavior**  
- **Modular and extensible design**

All contributions must follow these principles.

---

## How to Contribute

### 1. Fork the repository
Create your own fork before making any changes.

### 2. Create a feature branch
Use descriptive names such as:

- feature/sanitization-pipeline  
- fix/routing-bug  
- refactor/storage-layer  

### 3. Follow code and documentation conventions

- Keep modules isolated and avoid cross-layer coupling.  
- Avoid metadata leaks (logs, headers, fingerprints).  
- Use stable Markdown formatting and avoid nested code blocks.  
- Maintain architectural consistency with existing components.  
- Keep region-specific logic abstracted behind interfaces.

### 4. Submit a Pull Request

A valid PR must include:

- Summary of changes  
- Motivation or problem being solved  
- Security considerations  
- Testing notes  
- Architecture impact (if any)

### 5. Participate in review

All PRs require:

- Security review  
- Architecture review  
- Documentation review (when applicable)

Reviewers may request changes before approval.

---

## Documentation Contributions

Documentation must:

- Use stable, portable Markdown  
- Avoid nested code blocks  
- Follow the structure in `architecture/` and `docs/`  
- Be region-agnostic  
- Avoid assumptions about network topology or censorship models  
- Avoid diagrams containing sensitive metadata

---

## Security Considerations

Every contribution must consider:

- Metadata exposure  
- Transport fingerprinting  
- Regional safety  
- Sanitization correctness  
- Avoiding hard-coded endpoints  
- Avoiding region-specific assumptions  

If unsure, ask before submitting.

---

## Testing Requirements

- Include tests for new functionality.  
- Avoid tests that rely on region-specific behavior.  
- Do not include logs or metadata that could reveal sensitive patterns.  
- Integration tests must not contact real external endpoints.

---

## Communication

For general questions, use:

- GitHub Issues (non-security topics)  
- GitHub Discussions  

For security-related topics, follow the instructions in `SECURITY.md`.

---

## Thank You

Your contributions help strengthen a system designed to protect people in high-risk environments.
