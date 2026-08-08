<div align="center">

<a href="https://github.com/Made-in-Jurgistan/workwizard-public">
  <img src="assets/logo.png" alt="WorkWizard wizard logo" width="96" />
</a>

<br />

<img src="assets/WW.png" alt="WorkWizard" width="320" />

<br />

<p align="right">
  <img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />
</p>

<p>
  <img src="https://img.shields.io/badge/answer%20protection-hard%20constraint-DC2626" alt="Answer Protection: Hard Constraint" />
  <img src="https://img.shields.io/badge/SAST-bandit-3776AB?logo=python&logoColor=white" alt="SAST: bandit" />
  <img src="https://img.shields.io/badge/CORS-allowlist%20only-2EA44F" alt="CORS: Allowlist Only" />
</p>

</div>

---

# Security Policy — WorkWizard

This document describes WorkWizard's security posture at a high level.
Implementation details are not disclosed.

## Secrets Management

- All credentials exclusively via environment variables — never in source
- No secrets, API keys, or tokens in this public repository

## Input Validation

- Pydantic v2 validation at every request boundary
- File upload: max 10 MB, extension allowlist (PDF, DOCX, TXT, PNG, JPG, WEBP, GIF, BMP, TIFF)
- Interest selection: max 5 from curated catalog
- Language field: enum-restricted to `de` / `en`

## Answer Protection (Hard Constraint)

- Bilingual regex guard rejects any output that leaks a solution
- Source answers stripped from transformed output before processing
- Quality gate includes answer-revelation detection as a separate dimension
- Failed outputs enter retry loop; persistent failures return a safe fallback

## Rate Limiting

- Per-IP rate limiting on transform, upload, PDF, and interest routes
- Request ceilings to prevent worker exhaustion

## CORS

- Explicit allowlist — no wildcard in production

## Container Security

- Non-root containers
- Digest-pinned base images (not `:latest`)

## CI/CD Security

- SAST (bandit) on every CI run
- No secrets in build logs

## Known Limitations (v0.1.0)

- No end-user authentication — pilot will run under teacher-supervised access once the MVP is test-ready
- No audit log (planned for future release)
- MVP under active development — output quality is being continuously optimised

## Vulnerability Reporting

Report security issues to **madeinjurgistan@gmail.com**.

Do not open public GitHub issues for security vulnerabilities.

---

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>
