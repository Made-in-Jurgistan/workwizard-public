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
- K2.6 per-exercise specialist pipeline: isolated strategy compiler (instant+tools) +
  engaging writer (instant)
- Embedded narrative golden for bare-equation drills (`math_missing_factor`):
  numbers live in a mini word-problem scene, then the original equation
- Static specialist system prompts (cache-friendly; dynamics in user message only)
- Cache generation contract includes specialist-pipeline flags

### Changed
- Production default model: Kimi K2.6 (Instant+tools per exercise); `kimi-k2.7-code` opt-in
- Prompt architecture v4.4 — embedded narrative for missing-factor drills; specialist prompt layout
- `math_missing_factor` exercises now use engagement-first mini word-problems (not bare equations)
- Specialist compiler defaults to Instant mode (~server temp 0.6), not Thinking
- LLM-as-judge enabled by default (K2.6 Instant mode)

### Fixed
- Per-exercise answer-verification tool loop budget raised from 3 to 5 rounds
  — the 3-round budget could be exhausted by the verification tools alone,
  triggering an avoidable AI-service error that, under full worksheet
  concurrency, could trip the shared circuit breaker and fail unrelated
  exercises in the same document
- Startup now warns operators if production-mode debug logging is
  accidentally left enabled, which otherwise floods logs and risks hitting
  the hosting platform's log-rate ceiling
- Documented the verified transformation quality contract: per-exercise source checks,
  safe fallback for invalid output, direct handling for drill-style exercises, and
  conservative optional judge scoring
- Reconciled the pedagogical taxonomy description: 13 active frameworks represented by
  16 routed guidance labels
- Frontend type contract alignment: upload response `contentWarning` field correctly
  nullable in shared types and test fixtures
- Backend code style: import ordering and unused-import cleanup across test modules
- Strategy anchor validation: inline-label format now detected correctly; non-numeric
  anchor check no longer misfires on pure math equations with no non-numeric tokens

### Added
- SECURITY.md — high-level security policy
- CONTRIBUTING.md — community contribution guidelines
- ARCHITECTURE.md — public architecture overview
- .gitignore — repository hygiene
- CI workflow — docs validation, link checking, secret scanning, source code leak prevention
- `interest_categories` + `interests` catalog tables in DB migrations (RLS, indexes, FK)
- `pgvector` extension provisioned in initial schema for future DB-level vector search
- HTTP 429 rate-limit retry with exponential backoff in Supabase data layer
- Pagination (limit/offset) for `list_survey_responses()` admin endpoint
- `GradeDetectionSummary.to_dict()` for API responses and logging
- Re-exported `subject_narrative_suffix` from `core.grades` package

### Changed
- README.md updated as self-contained public showcase (no private clone instructions)
- PRESS_KIT.md — merged duplicate timeline entries, added social media placeholder
- Corrected backend test count to 1,882 (67 modules); frontend 625 tests across 32 files
- Frontend vitest coverage thresholds increased from 60% to 80%
- Prompt architecture v3.0 → v4.0: engagement-first framing, subject-aware hint
  discipline, age-appropriate humor mandates across all grade bands, ~25% token reduction
- ARCHITECTURE.md — added engagement-first and subject-aware design principles
- Corrected embedding model name to paraphrase-multilingual-MiniLM-L12-v2
- Corrected upload format list to include TXT, GIF, BMP, TIFF
- Qualified semantic caching 40-60% claim as estimated (pre-pilot, no measured data)
- Clarified across all public docs that the MVP is under active development with continuous output quality optimisation
- Grade 13 enrichment key corrected from `g1112` to `g912` (matches DB column `prompt_context_g912`)
- `K27_MODEL_PREFIX` constant deduplicated — single source in `core/constants.py`
- `SELECT *` replaced with explicit column lists in survey, quality, and flagged queries
- Regex patterns moved to module-level compiled constants (ReDoS input size cap added)
- `DataClient.__repr__` masks `service_key` and `anon_key` to prevent secret leakage
- `SubjectGradeModifier` simplified — removed unused `silliness_tier_override` and `narrative_complexity_delta` fields
- Bloom verb instruction tables — removed dead kindergarten (`"K"`) keys (no K grade band in MVP)

### Removed
- 7 unused `SubjectDomain` values (GEOGRAPHY, ECONOMICS, COMPUTER_SCIENCE, ETHICS_PHILOSOPHY, RELIGIOUS_EDUCATION, LATIN_CLASSICAL, PSYCHOLOGY) and 21 related `SubjectSubdomain` values
- Dead `OrchestrationError` exception class (never raised in production)
- Dead `EnrichmentFailed` exception class (never raised — enrichment degrades gracefully)
- Dead `EnrichmentFailed` handlers in `api/routes/transform.py`

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
- RAG enrichment via pgvector knowledge base
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
