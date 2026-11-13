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
- Secrets are redacted from logs and error responses
- No secrets, API keys or tokens in this public repository

## Input Validation

- Pydantic v2 validation at every request boundary
- File upload: max 10 MB; extension and file-type allowlist (PDF, DOCX, TXT, PNG, JPG/JPEG, WEBP, GIF, BMP, TIFF), with file-signature checks for every type except TXT
- Interest selection: 1 to 5 from the curated catalog; every interest is offered at every grade
- Language: German or English only
- Worksheet length and the model calls spent on one worksheet are bounded

## Prompt Injection

- Untrusted worksheet text is sanitised before it reaches a model: hidden characters and look-alike letters are normalised, instruction-override phrases are flagged and removed, and markup that could pose as instructions is neutralised
- The sanitiser is defence in depth beside the deterministic answer checks

## Answer Protection (Hard Constraint)

- A printed answer-key section is removed from the worksheet before processing
- A worksheet that prints its answers outside an answer key is rejected before any model call
- The model that rewrites a task never receives its answer
- Every task is checked in code; the model learns only whether an answer is unchanged, never the value, and a task whose answer changed or cannot be verified is not accepted
- A task that never passes its checks keeps its original text
- A bilingual answer guard scans the whole assembled worksheet; an output that reveals an answer is not returned

## Paid Image Generation

- Illustrations are generated only for cues that the server itself signed in the personalised worksheet; signatures expire after 24 hours
- An edited or unsigned cue renders without an image, and the PDF is still produced
- The number of images per PDF is capped

## Survey Data

- Signed sessions: the server issues every session and signs a token that every survey call carries, so no one can write to another respondent's session; each session submits once
- Server-side validation of every answer against the survey's questions and answer codes; answers from an outdated version of the survey are rejected and the respondent is asked to reload
- No IP address or user agent is stored with any answer, and no third-party fonts or trackers are loaded. To catch bulk submissions, completions are counted per day under a one-way, daily-salted hash of the network address; the count is never linked to answers and is deleted after 7 days
- An email is stored only for a purpose the adult ticks (launch news and results, a preview, a demo, the pilot list, an offer to help), and counts only after its owner confirms it through the link in one confirmation email; opening the link alone confirms nothing. Each purpose can be withdrawn on its own. Students never reach a contact field
- Uploaded worksheets: at most three files of 10 MB, file type read from the file's content, the uploader confirms the rights and that the sheet holds no personal data; files are stored privately, never published and never processed by the public API
- Erasure of one respondent's data on request: their contact details only, or all their answers, with their files
- 18-month retention window, enforced by a scheduled job
- Administration routes require an admin token; destructive operations (full reset, respondent erasure) require a separate token
- Survey pages are served with a Content-Security-Policy that disallows inline scripts

## Rate Limiting

- Per-IP rate limiting on upload, transform, PDF, interest and survey routes, and a cap on concurrent streamed transforms per client
- Request ceilings to prevent worker exhaustion (~280 s non-streaming; a longer budget for streamed progress on large worksheets)

## CORS and Security Headers

- Explicit allowlist; a wildcard origin is rejected at startup
- Security headers on every response, including a restrictive Content-Security-Policy, `X-Frame-Options: DENY` and HSTS in production

## Database

- Row-level security on every table
- The browser has no database access; all data access goes through the backend API
- Survey tables are reachable only from the server

## Container Security

- Non-root containers
- Digest-pinned base images (not `:latest`)

## CI/CD Security

- SAST (bandit) and dependency audits (pip-audit, npm audit) on every CI run
- GitHub Actions pinned to commit SHAs
- No secrets in build logs

## Known Limitations (v0.1.0)

- No end-user authentication — the pilot will run with teachers supervising access
- No audit log
- A documented data-protection review is required before the school pilot

## Vulnerability Reporting

Report security issues to **madeinjurgistan@gmail.com**.

Do not open public GitHub issues for security vulnerabilities.

---

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>
