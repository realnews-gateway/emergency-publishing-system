# Contributing Guide

Thank you for your interest in contributing to the Emergency Publishing System.
This project is designed for high-risk environments and requires careful attention to security, documentation quality, and architectural consistency.

Contribution Principles:
Security first
Minimal metadata
Consistent architecture
Clear documentation
Region-aware behavior
Modular and extensible design

How to Contribute:
Step 1: Fork the repository.
Step 2: Create a feature branch.
Examples:
feature/sanitization-pipeline
fix/routing-bug
refactor/storage-layer

Step 3: Follow conventions.
Keep modules isolated and avoid cross-layer coupling.
Avoid metadata leaks such as logs, headers, or fingerprints.
Use stable Markdown formatting and avoid nested code blocks.
Maintain architectural consistency with existing components.
Abstract region-specific logic behind interfaces.

Step 4: Submit a Pull Request.
A valid PR must include:
Summary of changes
Motivation or problem being solved
Security considerations
Testing notes
Architecture impact if any

Step 5: Participate in review.
All PRs require:
Security review
Architecture review
Documentation review when applicable
Reviewers may request changes before approval.

Documentation Requirements:
Use stable Markdown.
Avoid nested code blocks.
Follow the structure in architecture/ and docs/.
Be region-agnostic.
Avoid assumptions about network topology or censorship models.
Avoid diagrams containing sensitive metadata.

Security Considerations:
Every contribution must consider:
Metadata exposure
Transport fingerprinting
Regional safety
Sanitization correctness
Avoid hard-coded endpoints.
Avoid region-specific assumptions.
If unsure, ask before submitting.

Testing Requirements:
Include tests for new functionality.
Avoid tests that rely on region-specific behavior.
Do not include logs or metadata that could reveal sensitive patterns.
Integration tests must not contact real external endpoints.

Communication:
For general questions, use GitHub Issues or Discussions.
For security-related topics, follow the instructions in SECURITY.md.

Thank You:
Your contributions help strengthen a system designed to protect people in high-risk environments.
