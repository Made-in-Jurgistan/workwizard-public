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

This document gives a high-level overview of WorkWizard's architecture.
Implementation details, internal data models and infrastructure specifics
are not disclosed. WorkWizard is an MVP (version 0.1.0), live on Vercel and
Railway and not yet piloted in a school.

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
                    │  Mistral  │    │ Kimi K2.6 │    │  OpenAI   │
                    │    OCR    │    │ Transform │    │   Image   │
                    └───────────┘    └───────────┘    └───────────┘
```

| Provider | Role |
|----------|------|
| Mistral OCR | Reads uploaded PDFs and photos; MinerU is a self-hosted alternative |
| Kimi K2.6 (Moonshot AI) | Rewrites the worksheet's tasks in agent sessions |
| OpenAI GPT Image 1.5 Mini | Optional illustrations in the PDF |
| Supabase | Survey answers, confirmation emails (Auth) and uploaded worksheets (private storage) |

## Pipeline

```text
Upload  ──▶  Personalise  ──▶  Transform  ──▶  Review  ──▶  Download
PDF/DOCX     up to 5           agent           original     accessible
or photo     interests         sessions        side by side PDF
```

| Step | What happens |
|------|--------------|
| **Upload** | Text is extracted from a PDF, Word file, photo or text file; the grade band is detected and confirmed by the teacher |
| **Personalise** | The student picks up to 5 of 54 curated bilingual interests |
| **Transform** | Worksheets that print their answers, and worksheets of subjects that gain nothing from a story, are turned away before any model call. The worksheet is split into tasks without changing its text. Kimi K2.6 agent sessions rewrite groups of tasks around the chosen interests and check each task in code; a task that never passes keeps its original text. A final answer guard scans the whole worksheet |
| **Review** | Original and personalised versions are shown side by side with the detected subject, grade band and the frameworks applied |
| **Download** | A PDF laid out for the grade band, for the student to complete on paper |

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, TypeScript 5, Vite 7, Tailwind CSS 4 |
| Backend | Python 3.12, FastAPI, Pydantic v2 |
| AI | Kimi K2.6 (Moonshot AI, 256K context), Mistral OCR, OpenAI GPT Image 1.5 Mini (PDF illustrations) |
| PDF | WeasyPrint |
| Data | Supabase Postgres, Auth and Storage for the survey; interest catalogue and story kits are bundled with the backend |
| Testing | pytest (7,200+ tests), Vitest (740+ tests), Playwright (3 specs) |
| Infrastructure | Docker, GitHub Actions, Vercel, Railway |

## Coverage

| Dimension | Scope |
|-----------|-------|
| Worksheets | Any worksheet: work in class, homework, test preparation |
| Grade bands | 7: grades 1–2, 3–4, 5–6, 7–8, 9–10, 11–12 and 13 |
| Interests | 54 in 6 groups of 9, each offered at every grade; up to 5 per student |
| Subjects | 14 domains, 37 subdomains. Mathematics, science, language arts, social studies (geography, economics), world languages, technology, *Sachunterricht* and mixed sheets are personalised. History, civics, physical education, arts, music, and religion and ethics are turned away; in grades 1–4 history and civics count as *Sachunterricht* |
| Languages | German and English |
| Pedagogy | 8 framework labels: cognitive load, zone of proximal development, scaffolding, self-determination autonomy support, narrative transportation, worked examples, prior knowledge, metacognition |

## Key Design Principles

- **Engagement through relevance** — engagement comes from the student's own interest carrying the task, not from decoration; pedagogy is the responsible frame
- **Print-first** — the computer is used to make the worksheet; the student works on paper
- **Answer protection** — a hard constraint: the model that rewrites a task never receives its answer, code verifies that every answer is unchanged, and no output that reveals an answer is returned
- **Acceptance in code** — every task passes deterministic checks (original task intact, answer unchanged, no leak, length per grade band, age-appropriate topics) before it is accepted; a task that never passes is printed as in the original
- **Source text kept** — task text, numbers, blanks and formulas stay exactly as printed; the story is added around them
- **Bilingual by design** — German and English throughout, never mixed in one worksheet
- **Grade-aware** — 7 grade bands with their own sentence and word limits, hint rules and PDF layout
- **Subject-aware** — subjects that gain nothing from a story are turned away instead of personalised
- **Pedagogically grounded** — 8 frameworks, recorded per task: four for the story context, four for the strategy hints
- **Bounded spend** — every worksheet runs within a fixed budget of model calls and time; progress is streamed to the browser
- **Measured** — an evaluation harness runs a fixed set of worksheets through the real pipeline and scores the results, including how consistent repeated runs are

## What Is Not Disclosed

The following are proprietary and not documented in this public repository:

- Source code (backend and frontend)
- Prompt structures and orchestration logic
- Internal data models and database schema
- Deployment URLs and infrastructure addresses
- Internal documentation and design records
- Configuration files and environment variable details

For licensing inquiries: **madeinjurgistan@gmail.com**

---

<div align="center">

<sub>Copyright © 2025–2026 Made in Jurgistan. All rights reserved.</sub>

<br />

<img src="assets/made-in-jurgistan.svg" alt="Made in Jurgistan" width="120" />

</div>
