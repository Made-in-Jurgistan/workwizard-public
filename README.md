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
  <strong>Interest-driven worksheet personalisation for K-12 classrooms</strong>
</p>

<p>
  Any worksheet, rewritten inside a context the student chooses. Learning objectives stay intact,<br />
  answer leakage is checked, and the output is print-ready for teacher review.
</p>

<p>
  <a href="https://github.com/Made-in-Jurgistan/workwizard-public/actions/workflows/ci.yml"><img src="https://github.com/Made-in-Jurgistan/workwizard-public/actions/workflows/ci.yml/badge.svg" alt="CI" /></a>
  <a href="https://python.org"><img src="https://img.shields.io/badge/python-3.12%2B-3776AB?logo=python&logoColor=white" alt="Python 3.12+" /></a>
  <a href="https://react.dev"><img src="https://img.shields.io/badge/react-19-61DAFB?logo=react&logoColor=black" alt="React 19" /></a>
  <a href="https://fastapi.tiangolo.com"><img src="https://img.shields.io/badge/FastAPI-0.115%2B-009688?logo=fastapi&logoColor=white" alt="FastAPI" /></a>
  <a href="LICENSE.md"><img src="https://img.shields.io/badge/license-proprietary-B91C1C" alt="License: Proprietary" /></a>
</p>

<p>
  Survey: <a href="https://workwizard-demo.vercel.app/survey">workwizard-demo.vercel.app/survey</a>
</p>

</div>

<br />

<div align="center">

| Upload | Select interests | Personalised output |
| :---: | :---: | :---: |
| <img src="assets/screen1.png" alt="Upload a worksheet as text, file, or photo" width="270" /> | <img src="assets/screen2.png" alt="Choose up to five interests from a bilingual catalog" width="270" /> | <img src="assets/screen3.png" alt="Transformed exercise with narrative, problem, and strategy" width="270" /> |
| PDF, DOCX, or photo | 54 options, no account needed | Answers locked, goals intact |

</div>

<br />

## 📖 Contents

<table>
<tr>
<td valign="top">

- [✨ Why WorkWizard](#-why-workwizard)
- [⚙️ How it works](#️-how-it-works)
- [🧩 Features](#-features)
- [🏗️ Architecture](#️-architecture)

</td>
<td valign="top">

- [🎓 Pedagogical foundation](#-pedagogical-foundation)
- [🔌 API](#-api)
- [🔒 Security](#-security)
- [📚 Documentation](#-documentation)

</td>
<td valign="top">

- [🤝 Contributing](#-contributing)
- [🔄 Changelog](#-changelog)
- [⚖️ License](#️-license)

</td>
</tr>
</table>

> **About this repository:** This is the public showcase for WorkWizard. It contains project documentation, brand assets, and research papers — not source code. The codebase, ADRs, and operational data live in a private repository.

## ✨ Why WorkWizard

Practice is where learning consolidates, and it is exactly where students disengage. A teacher cannot hand-write a Minecraft version of a geometry sheet for one student and a Marvel version for another, so every student receives the same generic exercises.

WorkWizard removes that constraint. It ingests a teacher's existing worksheet and rewrites each exercise inside the student's chosen interest, treating the source objectives and answer protection as testable constraints. The result is a print-ready PDF for teacher review: the model runs once, and the student works offline on paper. Whether the approach improves engagement or attainment is a question for pilot evaluation to answer.

Built for the German K-12 system with full DE/EN bilingual output, grade-aware adaptation across seven grade bands, and research-backed pedagogical guidance driving prompt construction and quality scoring.

> **Status:** MVP `v0.1.0`, pre-pilot and under active development, with output quality being continuously optimised. Frontend on Vercel, backend on Railway.

## ⚙️ How it works

```text
Upload  ──▶  Personalise  ──▶  Transform  ──▶  Review  ──▶  Download
PDF/DOCX     up to 5           AI rewriting    quality      accessible
or photo     interests         per exercise    scored       PDF
```

| Step | What happens |
| --- | --- |
| **Upload** | Mistral OCR extracts structured text from PDF, DOCX, or image. MinerU is available as a self-hosted GDPR fallback. |
| **Personalise** | Student picks up to 5 of 54 curated interests across 6 categories, all bilingual. |
| **Transform** | Kimi K2 rewrites each exercise in the chosen context, calibrated by grade band, subject, and detected content type. |
| **Review** | Original and transformed shown side by side with multi-dimensional quality scores and answer-revelation checks. |
| **Download** | WeasyPrint renders a per-grade styled PDF; accessibility criteria are tested and conformance is not claimed without an audit. |

## 🧩 Features

| Capability | Detail |
| --- | --- |
| **Interest personalisation** | 54 interests across 6 categories (games, sports, TV and film, fantasy, superheroes, creative), bilingual EN/DE |
| **Grade-aware pedagogy** | 7 grade bands (1-2 through 13) with calibrated scaffolding, Bloom's levels, and motivation emphasis |
| **Answer protection** | Bilingual regex guard rejects any output that leaks a solution; enforced as a hard constraint |
| **Quality scoring** | Heuristic multi-dimensional scorer plus optional structured LLM evaluation; retry loop at a configurable threshold. Latency and quality are measured in CI and pilot runs. |
| **Semantic caching** | Estimated 40-60% fewer API calls via embedding deduplication and borderline-match validation (to be verified in pilot) |
| **RAG enrichment** | pgvector knowledge base with quality scoring and cultural safety checks |
| **Narrative diversity** | Anti-repetition engine ensuring varied narrative contexts across exercises |
| **Model routing** | Automatic thinking vs. instant mode selection based on content type and difficulty |
| **Accessible output** | WeasyPrint and Jinja2 templates, with applicable [WCAG 2.2](https://www.w3.org/TR/WCAG22/) AA criteria as a test target and per-grade CSS; no conformance claim without an audit |
| **Print-first design** | Screen time is subtracted rather than stacked; the model runs once, the student works offline |

## 🏗️ Architecture

```text
                        ┌──────────────────────┐
                        │     nginx proxy      │
                        │  :5600  ·  :8000     │
                        └──────────┬───────────┘
             ┌─────────────────────┼─────────────────────┐
             │                     │                     │
      ┌──────▼──────┐       ┌──────▼──────┐       ┌──────▼──────┐
      │  Frontend   │       │   Backend   │       │  Supabase   │
      │  React 19   │       │   FastAPI   │       │  Postgres   │
      │  Vite / TS  │       │   uvicorn   │       │  pgvector   │
      └─────────────┘       └──────┬──────┘       └─────────────┘
                                   │
                  ┌────────────────┼────────────────┐
            ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
            │  Mistral  │    │  Kimi K2  │    │ Semantic  │
            │    OCR    │    │ Transform │    │   Cache   │
            └───────────┘    └───────────┘    └───────────┘
```

<details>
<summary><strong>Backend pipeline stages</strong></summary>
<br />

| Stage | Role |
| --- | --- |
| Extraction | Document to structured text (Mistral OCR, MinerU fallback) |
| Language detection | Zero-dependency EN/DE heuristic |
| Grade detection | Readability, vocabulary, math-complexity signals |
| RAG enrichment | Interest entities, characters, and settings from pgvector knowledge base |
| Prompt building | Constraint-first zero-shot prompt with pedagogical blocks |
| Transformation | Kimi K2 (256K context), parallel per-exercise dispatch |
| Quality gate | Answer-revelation detection, dimensional scoring, LLM judge, retry loop |
| Caching | Semantic similarity cache (memory or Redis backend) |
| PDF generation | WeasyPrint and Jinja2 with per-grade CSS |

</details>

## 🎓 Pedagogical foundation

<details>
<summary><strong>Expand: 9 active frameworks within a 13-framework taxonomy</strong></summary>
<br />

The repository documents a 13-framework pedagogical taxonomy in its research and product documentation, but **nine frameworks are active in the running backend, not all 13**. The backend routes those nine pedagogy labels to each content type's primary and secondary guidance. Eight additional, conditional prompt blocks operationalize selected mechanisms such as self-explanation, desirable difficulties, interleaving, worked examples, UDL, spaced review, attribution framing, and value connection. The table below lists the full taxonomy for design context; it should not be read as proof that all 13 frameworks are independently implemented.

| Framework | Citation | Role |
| --- | --- | --- |
| Cognitive Load Theory | Sweller (1988, 2024) | Working-memory chunk limits per age |
| Zone of Proximal Development | Vygotsky (1978) | Scaffolding calibration |
| Bloom's Taxonomy | Anderson & Krathwohl (2001) | Cognitive level targeting |
| Self-Determination Theory | Ryan & Deci (2000, 2017) | Intrinsic motivation via interest alignment |
| Flow Theory | Csikszentmihalyi (1990) | Challenge-skill balance |
| Growth Mindset | Dweck (2006) | Process praise over outcome praise |
| Discovery Learning | Bruner | Guided inquiry, Socratic prompts |
| Dual Coding Theory | Paivio (1971), Mayer (2009) | Visual hierarchy fading with age |
| Narrative Transportation | Green & Brock (2000) | Interest-driven immersion |
| Worked Examples Effect | Sweller (2011) | Full to faded to minimal |
| Prior Knowledge Activation | Ausubel | Everyday-to-academic bridging |
| Metacognition | Flavell, Schraw & Dennison | Predict-plan-check cues |
| Cognitive Activation | Burge, Lenkeit & Sizmur (2015) | Reasoning beyond recall |

Seven pedagogical blocks are injected into every system prompt (Feynman self-explanation, difficulty framing, UDL choice, value connector, attribution framing, worksheet structure, and worked example), with a conditional spaced-review block in the user prompt for students with session history.

Evidence notes and source links are maintained in the [K-12 frameworks report](k12-pedagogical-frameworks.md). These frameworks guide design; they are not evidence that WorkWizard itself improves outcomes.

</details>

### Tech stack

| Layer | Technology |
| --- | --- |
| Frontend | React 19, TypeScript 5.8, Vite 7.2, i18n DE/EN |
| Backend | Python 3.12, FastAPI 0.115+, Pydantic v2, structlog |
| AI | Kimi K2.7-code (Moonshot, 256K context), Mistral OCR |
| Embeddings | paraphrase-multilingual-MiniLM-L12-v2 (384-dim, multilingual) |
| Data | Supabase Postgres with pgvector |
| Testing | pytest (1,881 tests, 60 modules), Vitest and Playwright (21 files) |
| Infrastructure | Docker Compose, nginx, GitHub Actions, Vercel, Railway |

## 🔌 API

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/health` | Service and dependency status |
| `POST` | `/api/upload` | Upload PDF, DOCX, or image (max 10 MB) |
| `GET` | `/api/interests/catalog` | Interest catalog with metadata |
| `POST` | `/api/ai-transformation/transform` | Transform exercises |
| `POST` | `/api/ai-transformation/transform/stream` | Transform with SSE progress streaming |
| `POST` | `/api/pdf/generate` | Generate styled PDF |
| `GET` | `/api/pdf/download/{cache_key}` | Download a generated PDF |
| `POST` | `/api/survey/submit` | Submit an anonymous survey response |

Admin-token gated routes cover survey administration and cache invalidation. Full interactive reference at `/docs`.

## 🔒 Security

- Credentials exclusively via environment variables; no secrets in source
- Per-IP rate limiting on transform, upload, PDF, and interest routes
- Explicit CORS allowlist, no wildcard in production
- Non-root containers and digest-pinned base images
- Pydantic v2 validation at every request boundary
- SAST (bandit) on every CI run
- Request ceilings to prevent worker exhaustion
- No end-user authentication in v0.1.0 — pilot will run under teacher-supervised access once the MVP is test-ready

See [SECURITY.md](SECURITY.md) for the full security policy.

## 📚 Documentation

- [Architecture overview](ARCHITECTURE.md) — high-level system design and tech stack
- [Security policy](SECURITY.md) — security posture and vulnerability reporting
- [Contributing guidelines](CONTRIBUTING.md) — bug reports, feature suggestions, interest and market survey
- [Changelog](CHANGELOG.md) — notable changes per release
- [K–12 pedagogical, psychological, and engagement frameworks](k12-pedagogical-frameworks.md) — 13-framework taxonomy, citations, and design context
- [WorkWizard Research Foundation](workwizard-research-foundation.md) — research basis for the product
- [Press Kit](PRESS_KIT.md) — product boilerplate, fact sheet, screenshots, brand assets, and press FAQ

## 🤝 Contributing

This is a proprietary project and external code contributions are not accepted at this time. Bug reports and feature suggestions are welcome through [GitHub Issues](https://github.com/Made-in-Jurgistan/workwizard-public/issues), and visitors can share feedback through the in-app interest and market survey at [workwizard-demo.vercel.app/survey](https://workwizard-demo.vercel.app/survey).

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 🔄 Changelog

See [CHANGELOG.md](CHANGELOG.md) for notable changes per release.

## ⚖️ License

This software is proprietary. All rights are reserved by the copyright holder. Unauthorised copying, modification, distribution, or commercial use of any part of this repository is prohibited. For licensing inquiries, contact **madeinjurgistan@gmail.com**.

The full terms are in [LICENSE.md](LICENSE.md).

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>