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

### Added
- Offline evaluation harness: recorded DE/EN worksheets are run through the production
  pipeline and scored deterministically (exercise count, source preservation, answer
  leaks, word budget, success rate, latency), with regression checks against a baseline
- Per-exercise word budget scaled by grade band, enforced by the quality gate
- Worksheet theme: one companion character per interest for the whole worksheet, with
  settings rotating per exercise
- Separate strategy-compile step for answer-critical numeric exercise types
- Bare-equation exercises embed the numbers in a short narrative scene before
  presenting the original equation unchanged
- Survey contact form on the intro and thanks screens
- Pre-pilot survey: optional results-summary opt-in for every adult respondent;
  one-sheet class trial offer for teachers
- Pre-pilot survey: referral-intent question (an NPS-style "would you recommend this?"
  proxy) for parents, teachers, and administrators on the Full path
- Pre-pilot survey: school administrators answer a champion-likelihood question that
  is kept separate from the formal pilot-approval decision
- Pre-pilot survey: willingness-to-pay confidence check for every adult role
- Pre-pilot survey: a student attention check surfaces a data-quality pass rate on the
  admin dashboard
- Interest catalog stored in the database, with access controls and indexing
- `pgvector` extension provisioned in the initial schema for future DB-level vector search
- Automatic retry with backoff for transient database rate limits
- Pagination for the admin survey-responses endpoint
- SECURITY.md, CONTRIBUTING.md, ARCHITECTURE.md, and .gitignore in this repository
- CI workflow for this repository: docs validation, link checking, secret scanning,
  source code leak prevention

### Changed
- Per-exercise quality pipeline: one deterministic gate, one structured judge call with
  yes/no verdicts per dimension, and at most one repair; output that fails the gate is
  replaced by the source exercise
- No model request carries tools or the computed answer; repair feedback is redacted
  of the answer before it reaches the model
- Engagement comes from relevance to the interest (native objects, roles, and units)
  rather than jokes, twists, or extra characters
- Worksheet preprocessing keeps lettered items, repeated instructions, tables, and
  header lines intact; answer-key sections are removed before transformation
- Production default model: Kimi K2.6 (instant mode per exercise); K2.7 family unsupported
- LLM-based quality judging enabled by default
- Large worksheets transform in bounded parallel waves; progress streams over SSE
- Pre-pilot survey instrument `2026-09-26-owned-list`; submissions from earlier
  instrument versions are rejected
- Pre-pilot survey: German copy for teachers uses the formal "Sie", matching the
  administrator role; the parent root-cause question separates "boring" and
  "pointless"; questions that don't apply are skipped; estimated completion time is
  4–8 minutes depending on role
- Research documents moved under `docs/research/` with a README index
  (pedagogical frameworks report and market validation foundation)
- Frontend Vitest coverage threshold raised to 80%
- Updated the PDF-rendering dependency to close a known vulnerability

### Removed
- Transformation response cache: every exercise is generated fresh and passes the
  quality gate

### Fixed
- Exercises in larger worksheets no longer time out and ship as the original,
  untransformed text under concurrent load
- Provider billing errors fail fast with an actionable message instead of retrying
- Downloading the PDF no longer resets the wizard to the first step
- Switching between German and English re-renders every wizard step
- PDF rendering crashes on list markers and complex layouts
- Timeout handling that could cut off progress updates on longer worksheets
- Under full-worksheet concurrency, an answer-verification failure on one exercise
  could affect unrelated exercises in the same document
- Personalised strategy hints for repetitive-structure exercises are checked for
  exercise-specific grounding before being shown
- Pre-pilot survey: the concept-demo engagement metric is measured from real scroll
  behaviour
- Safeguards against regular-expression denial-of-service (ReDoS) on validation inputs
- Credential values are excluded from debug logging

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
- Semantic response caching
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
