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
- Answer-key sections are stripped from the source worksheet before processing
- The computed answer is never included in any model input
- Answer-revelation detection is a hard check in the per-exercise quality gate
- A failing exercise gets at most one repair attempt on the per-exercise pipeline; if it still fails, the source exercise is used
- On the agent engine, every task is checked in code and a task that never passes is printed as in the source; the model receives a verdict on whether the answer is unchanged, never the computed value
- Both engines pass their output through the same answer guard

## Survey Data

- Anonymous: no IP address or user-agent is stored, and no third-party fonts or trackers are loaded
- Server-side validation of every answer against the current instrument version
- Contact details are stored only after the respondent ticks a separate consent box for each purpose; students cannot leave contact details
- Erasure of one respondent's data on request, covering answers, lead status, and contact messages
- 18-month retention window, enforced by a scheduled job
- Administration routes require an admin token; destructive operations (full reset, respondent erasure) require a separate token
- Static survey pages are served with a Content-Security-Policy that disallows inline scripts

## Rate Limiting

- Per-IP rate limiting on transform, upload, PDF, and interest routes
- Request ceilings to prevent worker exhaustion (~280 s non-streaming; longer budget for SSE streaming on large worksheets)

## CORS

- Explicit allowlist — no wildcard in production

## Container Security

- Non-root containers
- Digest-pinned base images (not `:latest`)

## CI/CD Security

- SAST (bandit) and dependency audits (pip-audit, npm audit) on every CI run
- No secrets in build logs

## Known Limitations (v0.1.0)

- No end-user authentication — pilot will run under teacher-supervised access once the MVP is test-ready
- No audit log
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
