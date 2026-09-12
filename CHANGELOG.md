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
  <img src="https://img.shields.io/badge/version-0.1.0-2EA44F" alt="Version 0.1.0" />
  <img src="https://img.shields.io/badge/format-Keep%20a%20Changelog-3776AB" alt="Keep a Changelog" />
  <img src="https://img.shields.io/badge/semver-2.0.0-FF6F61" alt="Semantic Versioning" />
</p>

</div>

---

# Changelog — WorkWizard

All notable changes to WorkWizard are documented in this file.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed
- Performance, reliability, and security improvements to the AI transformation pipeline and
  caching layer: faster cache lookups on larger worksheets, reduced lock contention under
  concurrent transforms, more accurate concurrency tuning for multi-exercise worksheets, and an
  updated PDF-rendering dependency to close a known vulnerability
- Fixed a timeout-handling bug that could cut off progress updates mid-transform on
  longer worksheets; long-running transforms now reliably run to completion
- Improved transform reliability for large worksheets: exercises run in bounded
  parallel waves instead of overwhelming the AI provider; progress streams over SSE
- Research documents consolidated under `docs/research/` with a README index
  (pedagogical frameworks report and market validation foundation)
- Pre-pilot survey instrument aligned to `2026-09-01-wtp-aligned`: retired legacy
  answer codes on new submissions; admin WTP metrics use the current €/student/year tiers

### Added
- Pre-pilot survey: referral-intent question (an NPS-style "would you recommend this?"
  proxy) for all four audiences — the clearest organic-growth signal collected so far
- Pre-pilot survey: school administrators now answer a champion-likelihood question that
  is kept separate from the formal pilot-approval decision, so the dashboard's "would use"
  and "pilot intent" rates measure two genuinely different things
- Pre-pilot survey: parents get a willingness-to-pay confidence check (matching the one
  administrators already had), reducing hypothetical-pricing bias
- Pre-pilot survey: a student attention check surfaces a data-quality pass rate on the
  admin dashboard
- Added a verification-focused generation path for exercise types where answer accuracy
  is most critical
- Bare-equation exercises now embed the numbers in a short narrative scene before
  presenting the original equation unchanged

### Changed
- Pre-pilot survey: German copy for teachers now uses the formal "Sie", matching the
  administrator role; the parent root-cause question no longer combines "boring" and
  "pointless" into one option; questions that don't apply are skipped; and the estimated
  completion time was recalibrated to roughly 4–5 minutes
- Production default model: Kimi K2.6 (Instant+tools per exercise); K2.7 family unsupported
- LLM-based quality judging enabled by default

### Fixed
- Worksheet transformation reliability: exercises in larger worksheets no longer time
  out and silently ship as the original, untransformed text under concurrent load —
  request-rate admission control plus a reduction in redundant AI verification calls
  per exercise significantly cut queuing during full-worksheet transformation
- Personalised strategy hints for repetitive-structure exercises (e.g. fill-in-the-blank
  equations) are now checked for exercise-specific grounding before being shown,
  reducing generic, copy-pasteable hint phrasing
- Growth-mindset encouragement phrasing is now consistently present in generated
  strategy hints instead of being dropped in a majority of exercises
- Oversized narrative introductions at the highest grade levels are now automatically
  corrected rather than shipped over length
- Internal quality scoring no longer over-penalizes an exercise's overall score for one
  unrelated formatting nit
- Pre-pilot survey: the concept-demo engagement metric is now measured from real
  scroll behaviour instead of being marked complete for everyone who reached the end,
  so the reported engagement rate reflects what respondents actually did
- Fixed an issue where, under full-worksheet concurrency, an answer-verification failure
  on one exercise could occasionally affect unrelated exercises in the same document
- Documented the verified transformation quality contract: per-exercise source checks,
  safe fallback for invalid output, direct handling for drill-style exercises, and
  conservative optional judge scoring
- Reconciled the pedagogical taxonomy description: 13 active frameworks represented by
  16 routed guidance labels

### Added
- SECURITY.md — high-level security policy
- CONTRIBUTING.md — community contribution guidelines
- ARCHITECTURE.md — public architecture overview
- .gitignore — repository hygiene
- CI workflow — docs validation, link checking, secret scanning, source code leak prevention
- Interest catalog moved into the database, with access controls and indexing
- `pgvector` extension provisioned in initial schema for future DB-level vector search
- Automatic retry with backoff for transient database rate limits
- Pagination for the admin survey-responses endpoint

### Changed
- README.md updated as self-contained public showcase (no private clone instructions)
- PRESS_KIT.md — merged duplicate timeline entries, added social media placeholder
- Corrected backend test count to 1,882 (67 modules); frontend 625 tests across 32 files
- Frontend vitest coverage thresholds increased from 60% to 80%
- Prompt design updated with engagement-first framing, subject-aware hint discipline,
  and age-appropriate humor guidance across all grade bands
- ARCHITECTURE.md — added engagement-first and subject-aware design principles
- Corrected embedding model name to paraphrase-multilingual-MiniLM-L12-v2
- Corrected upload format list to include TXT, GIF, BMP, TIFF
- Qualified semantic caching 40-60% claim as estimated (pre-pilot, no measured data)
- Clarified across all public docs that the MVP is under active development with continuous output quality optimisation
- Added safeguards against regular-expression denial-of-service (ReDoS) on validation inputs
- Hardened internal logging so credential values are never included in debug output

## [0.1.0] — 2026-04-20

### Added
- Initial MVP release (under active development, with continuous output quality optimisation)
- Interest-driven worksheet personalisation for K-12 classrooms
- Upload PDF, DOCX, or photo → AI rewriting → print-ready PDF
- 54 curated interests across 6 categories (games, sports, TV/film, fantasy, superheroes, creative)
- Full DE/EN bilingual output
- 7 grade bands (1-2 through 13) with calibrated pedagogical scaffolding
- 13-framework pedagogical taxonomy, all 13 active in backend (16 routed labels)
- Answer-revelation detection (bilingual regex guard)
- Multi-dimensional quality scoring with retry loop
- Semantic caching (estimated 40-60% fewer API calls, to be verified in pilot)
- RAG enrichment via a curated knowledge base
- WeasyPrint PDF generation with per-grade CSS
- Anonymous in-app interest and market survey (parents, teachers, students, admins)
- K-12 pedagogical frameworks report
- WorkWizard research foundation document

---

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>
