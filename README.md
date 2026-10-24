# Tusinik — Aphasia-Friendly Web Page Simplifier Platform

Diploma project, Software Engineering, Astana IT University (group SE-2430).

Tusinik is a web platform that takes a web page link or pasted text and produces a version that is easier to read for people with aphasia. The reader chooses one of three simplification levels; the platform keeps numbers, dates and names, checks that the meaning is preserved, and shows the result in a clear reading view with the original one click away, adjustable display settings, read-aloud and optional icons with captions. Texts and interface are supported in Russian, Kazakh and English.

- **EN:** Development of Aphasia-Friendly Web Page Simplifier platform
- **RU:** Разработка платформы для упрощения веб-страниц, адаптированной для людей с афазией
- **KZ:** Афазиясы бар адамдарға ыңғайлы веб-бет мазмұнын жеңілдету платформасын әзірлеу

## 1. Problem statement

Aphasia is an acquired language disorder, most often caused by stroke, that impairs understanding of language, including reading. Long, term-heavy web pages are hard to read for many people with aphasia, and even material written for them is often above a readable level. Web accessibility guidance has traditionally focused on visual, motor and hearing impairments; guidance for cognitive and language difficulties is supplemental. Existing tools either change only how text looks, replace single words, or summarise; none of those reviewed combines rewriting at an adjustable level, a meaning check, a documented API and a comprehension-based evaluation. Details and sources: [`docs/01-proposal.md`](docs/01-proposal.md), [`docs/03-literature-review.md`](docs/03-literature-review.md), [`docs/04-existing-systems-comparison.md`](docs/04-existing-systems-comparison.md).

## 2. Target users

- People with aphasia (primary users)
- Their relatives and caregivers
- Speech-language therapists

## 3. Expected outcomes

- A web platform that simplifies a page (URL) or pasted text at three levels, in Russian, Kazakh or English.
- Automatic meaning check, with the original text shown next to the simplified text.
- Reading view with display settings, read-aloud with adjustable speed, optional icons with captions, and history kept in the browser.
- A documented REST API (OpenAPI).
- An evaluation against the original page: readability indicators, meaning preservation, comprehension and reading time, usability.

## 4. Team members and responsibilities

| Name | Group | GitHub | Role | Responsibility |
|---|---|---|---|---|
| Sovet Yerkebulan Talgatuly | SE-2430 | [Brxlq](https://github.com/Brxlq) | Backend Developer | Backend API (FastAPI), content extraction, simplification pipeline and meaning check, storage; relevance evidence, comparison of systems, technology decisions, requirements, supervisor feedback |
| Sadenov Ernar Aidynovich | SE-2430 | [AIeeph](https://github.com/AIeeph) | Frontend Developer | Web client (React), reading view, settings, read-aloud and icons; proposal, literature review, diagrams, prototype, verification plan, repository check |

## 5. Supervisor

Mirzagalikova Botagoz Vitalievna

## 6. Scope

**In scope:** URL or pasted text input (RU/KZ/EN); main-content extraction; simplification at three levels with a meaning check; reading view with original text, display settings, read-aloud and optional icons; history and settings stored in the browser; REST API and web client; evaluation against a baseline.

**Out of scope (first version):** user accounts; medical or therapy advice; browser extensions and mobile apps; pages behind a login (the user pastes the text instead); automatic personalisation by aphasia type; clinical validation.

Full scope: [`docs/01-proposal.md`](docs/01-proposal.md#8-scope). Requirements: [`docs/06-requirements.md`](docs/06-requirements.md).

## 7. Current status

Analysis and design stage (first pre-defense, tag `prob-period-1`). Completed: proposal, evidence of relevance (literature and 1 of the planned 3–5 conversations: a written consultation with a speech-language therapist), literature review (19 sources, 12 peer-reviewed), comparison of 5 existing systems, technology decisions with ADRs, requirements with acceptance criteria, architecture diagrams, clickable prototype, verification and evaluation plan. No application code yet. Known gaps and next steps: [`docs/10-project-management.md`](docs/10-project-management.md).

## 8. Selected technologies

| Area | Choice | Decision record |
|---|---|---|
| Backend | Python, FastAPI | [ADR-0001](docs/decisions/0001-backend-fastapi.md) |
| Content extraction | Trafilatura + BeautifulSoup | [ADR-0002](docs/decisions/0002-content-extraction-trafilatura.md) |
| Simplification | Hosted language-model API behind a provider interface (provider chosen by comparison P1), rule-based safety layer, meaning check | [ADR-0003](docs/decisions/0003-simplification-hosted-llm.md) |
| Storage | PostgreSQL (SQLite in tests); no user accounts | [ADR-0004](docs/decisions/0004-storage-postgresql.md) |
| Client–backend interaction | Synchronous REST/JSON, OpenAPI | [ADR-0005](docs/decisions/0005-api-synchronous-rest.md) |
| Frontend | React + TypeScript (Vite), Web Speech API | [ADR-0006](docs/decisions/0006-frontend-react.md) |
| Quality | pytest, Vitest, Playwright, axe-core, textstat, GitHub Actions, Docker Compose | [`docs/05-technology-comparison.md`](docs/05-technology-comparison.md) |

## 9. Links

- YouTrack project: https://tusinik.youtrack.cloud/projects/DIP
- Documentation: [`docs/`](docs/)
- Diagrams (sources and exports): [`diagrams/`](diagrams/README.md)
- Clickable prototype: [`prototype/index.html`](prototype/index.html) (download and open in a browser); screenshots in [`prototype/screens/`](prototype/screens/); description in [`docs/08-ux-prototype.md`](docs/08-ux-prototype.md)

## 10. Documentation map

| Pre-defense component | File | YouTrack |
|---|---|---|
| Project proposal and scope | [docs/01-proposal.md](docs/01-proposal.md) | DIP-2 |
| Evidence of relevance | [docs/02-relevance-evidence.md](docs/02-relevance-evidence.md) | DIP-3 |
| Literature review | [docs/03-literature-review.md](docs/03-literature-review.md) | DIP-4 |
| Comparison of existing systems | [docs/04-existing-systems-comparison.md](docs/04-existing-systems-comparison.md) | DIP-5 |
| Technology comparison | [docs/05-technology-comparison.md](docs/05-technology-comparison.md), [docs/decisions/](docs/decisions/) | DIP-6 |
| Requirements and acceptance criteria | [docs/06-requirements.md](docs/06-requirements.md) | DIP-7 |
| Architecture and diagrams | [docs/07-architecture.md](docs/07-architecture.md), [diagrams/](diagrams/) | DIP-8 |
| Prototype | [docs/08-ux-prototype.md](docs/08-ux-prototype.md), [prototype/](prototype/) | DIP-9 |
| Verification and evaluation plan | [docs/09-verification-plan.md](docs/09-verification-plan.md) | DIP-10 |
| Project management, tasks, risks | [docs/10-project-management.md](docs/10-project-management.md) | DIP-12 |
| Supervisor feedback | [docs/11-supervisor-feedback.md](docs/11-supervisor-feedback.md) | DIP-11 |

## 11. Repository structure

```text
src/        Application code (backend/ and frontend/ placeholders until implementation)
docs/       Project documentation; decisions/ holds architecture decision records
diagrams/   Diagram sources (src/, PlantUML) and exports (exports/, SVG and PNG)
prototype/  Clickable HTML prototype and screenshots
tests/      Automated tests and test data (to be added with the code)
config/     Configuration examples (.env.example, placeholders only)
.github/    Pull request template and workflows
```

## 12. Workflow

Each work item has one YouTrack task. Branches are named `DIP-<id>-short-description`, and commit messages use the format `DIP-<id>: description`. Changes go through pull requests that reference the task and describe how the work was checked. The other team member reviews and merges; `main` is the reviewed baseline.

## 13. Setup, run and test instructions

There is no runnable application code yet. To view the prototype, open `prototype/index.html` in a browser. Setup, run and test instructions (`docker compose up`, `pytest`, frontend tests) will be added with the first code milestone (M2 in `docs/10-project-management.md`).

## 14. Submission tags

Each submission is marked with a tag that is never moved or replaced. First probationary-period submission: [`prob-period-1`](https://github.com/Brxlq/Tusinik/tree/prob-period-1).
