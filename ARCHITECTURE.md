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
  <img src="https://img.shields.io/badge/backend-FastAPI-009688?logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/frontend-React%2019-61DAFB?logo=react&logoColor=black" alt="React 19" />
  <img src="https://img.shields.io/badge/AI-Kimi%20K2.6-7C3AED" alt="Kimi K2.6" />
  <img src="https://img.shields.io/badge/data-Supabase-3ECF8E?logo=supabase&logoColor=white" alt="Supabase" />
</p>

</div>

---

This document provides a high-level architecture overview of WorkWizard.
Implementation details, internal data models, and infrastructure specifics
are not disclosed. The MVP is under active development, with output quality
being continuously optimised.

## System at a Glance

```text
      ┌─────────────┐              ┌─────────────┐              ┌─────────────┐
      │  Frontend   │─────────────▶│   Backend   │─────────────▶│  Supabase   │
      │  React 19   │              │   FastAPI   │              │  Postgres   │
      │  Vite / TS  │              │   uvicorn   │              └─────────────┘
      └─────────────┘              └──────┬──────┘
                                           │
                          ┌────────────────┼────────────────┐
                    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
                    │  Mistral  │    │  Kimi K2  │    │  OpenAI   │
                    │    OCR    │    │ Transform │    │ GPT Image │
                    └───────────┘    └───────────┘    └───────────┘
```

## Pipeline

```text
Upload  ──▶  Personalise  ──▶  Transform  ──▶  Review  ──▶  Download
PDF/DOCX     up to 5           AI rewriting    quality      accessible
or photo     interests         per exercise    scored       PDF
```

| Step | What happens |
|------|--------------|
| **Upload** | OCR extracts structured text from PDF, DOCX, or image |
| **Personalise** | Student picks up to 5 of 54 curated bilingual interests |
| **Transform** | K2.6 rewrites each exercise inside the interest context; bare equations become embedded mini word-problems with strategy hints |
| **Review** | Original and transformed shown side by side with quality scores |
| **Download** | Per-grade styled PDF for offline student work |

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, TypeScript 5.x, Vite, Tailwind CSS |
| Backend | Python 3.12, FastAPI, Pydantic v2, structlog |
| AI | Kimi K2.6 (transform and judge, instant mode), Mistral OCR, OpenAI GPT Image 1.5 Mini (PDF illustrations) |
| Data | Supabase Postgres (pgvector provisioned; retrieval is deterministic key lookup) |
| Testing | pytest, Vitest, Playwright |
| Infrastructure | Docker Compose, nginx, GitHub Actions, Vercel, Railway |

## Key Design Principles

- **Engagement-first** — engagement is the core product goal and comes from relevance to the interest, not decoration; pedagogy is the responsible frame
- **Print-first** — the model runs once; the student works offline on paper
- **Answer protection** — a hard constraint; answers are never revealed in output
- **Bilingual by design** — DE/EN throughout, never mixed in output
- **Grade-aware** — 7 grade bands with calibrated scaffolding and a grade-scaled word budget
- **Subject-aware** — hint discipline and evaluation criteria adapt to subject domain
- **Pedagogically grounded** — all 13 frameworks active (16 routed labels)
- **Quality-scored** — a deterministic per-exercise gate (source preservation, answer leaks), one LLM-as-judge call (K2.6 instant mode), at most one repair, and fallback to the source exercise
- **Risk-adjusted generation** — for answer-critical numeric exercise types, the solution strategy is compiled in a separate step before the narrative is written; other exercise types use a single call with the same answer-protection checks. The computed answer is never included in any model input
- **Bounded parallelism** — large worksheets transform in admission-controlled waves so provider limits are respected; production UI streams progress over SSE

## What Is Not Disclosed

The following are proprietary and not documented in this public repository:

- Source code (backend and frontend)
- Prompt structures and orchestration logic
- Internal data models and database schema
- Deployment URLs and infrastructure addresses
- ADRs and internal documentation
- Configuration files and environment variable details

For licensing inquiries: **madeinjurgistan@gmail.com**

---

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>
