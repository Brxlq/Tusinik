# 05 — Technology Comparison

**YouTrack task:** DIP-6

---

## 1. Purpose

This document compares alternatives for the key technical decisions of the platform and justifies each choice. Every decision is evaluated against the drivers below, which come from the proposal (`01-proposal.md`), the literature review (`03-literature-review.md`) and the requirements (`06-requirements.md`).

## 2. Decision drivers

| ID | Driver | Requirements |
|---|---|---|
| D1 | Accept a web page URL or pasted text and extract the readable main content (without menus, ads, footers) | FR-01, FR-02, FR-03, FR-04 |
| D2 | Rewrite text to a selected simplification level in Russian, Kazakh or English while preserving meaning | FR-05, FR-06, FR-07 |
| D3 | Measure output: readability indicators, length, and support for evaluation of comprehension and reading time | FR-16, FR-17 |
| D4 | Documented, testable backend API that the web client (and later other clients) can use | FR-15, NFR-08 |
| D5 | Response time: 90 % of requests for up to 1,500 words finish within 15 s | NFR-01, NFR-02 |
| D6 | Privacy and security: no storage of pasted text, safe fetching of user-supplied URLs, no secrets in the repository | NFR-05, NFR-06 |
| D7 | Accessible, multilingual web client with reading view, settings, text-to-speech and history in the browser | FR-08–FR-14, NFR-03, NFR-04 |
| D8 | Feasible for a two-person team within the diploma timeline; matches team skills | — |

Team skills note (D8): the team has practical experience with JavaScript/Node.js and MongoDB (Postera project), Swift/Firebase (Echelon project), and coursework in OOP, Web, software design and algorithms. Python is chosen for the backend because of its text-processing ecosystem; this is a conscious trade-off, recorded as a risk in Section 10.

Scoring scale in tables: **++** strong fit, **+** acceptable, **–** weak fit.

---

## 3. Decision 1 — Backend platform (ADR-0001)

| Criterion | FastAPI (Python) | Django + Django REST Framework (Python) | Node.js + Express (JavaScript) |
|---|---|---|---|
| Access to text-processing and NLP libraries (D1, D2, D3) | ++ same language as extraction/readability libraries | ++ same | – fewer mature libraries for readability metrics and content extraction |
| Automatic API documentation (D4) | ++ OpenAPI/Swagger generated from type hints | + via add-on packages | + via add-on packages |
| Request/response validation (D4) | ++ built in (Pydantic models) | + serializers | + needs extra library |
| Async I/O for calling external services such as an LLM API and fetching pages (D5) | ++ native async | + supported, less central | ++ native async |
| Amount of built-in features not needed (admin, templates) (D8) | ++ minimal | – heavy for an API-only service | ++ minimal |
| Team experience (D8) | – new for the team | – new for the team | ++ used in Postera |
| Testing (D4) | ++ pytest + test client | ++ built-in test tools | + Jest/Mocha |

**Decision:** FastAPI.
**Justification:** the core of the system is text processing (D1–D3), and the strongest libraries for that are in Python. FastAPI gives an automatically documented, validated API (D4) and async calls to external services (D5) with little boilerplate (D8). Node.js is closer to the team's experience, but would require weaker or self-written replacements for extraction and readability tooling.
**Rejected:** Django (more than an API-only service needs); Node.js/Express (weaker text-processing ecosystem).

---

## 4. Decision 2 — Main-content extraction from web pages (ADR-0002)

| Criterion | Trafilatura (Python) | Mozilla Readability (JavaScript, Firefox Reader View algorithm) | Custom rules with BeautifulSoup |
|---|---|---|---|
| Removes boilerplate (navigation, ads, footers) (D1) | ++ designed for main-text extraction [L14] | ++ designed for reader mode | – every site needs hand-written rules |
| Keeps structure: headings, paragraphs, lists (D1) | + configurable output formats | ++ returns cleaned HTML | + only what rules preserve |
| Same runtime as backend (D8) | ++ Python | – needs a Node.js process or a port | ++ Python |
| Maintenance effort (D8) | ++ library | + library, extra service | – high |
| Control over edge cases | + options and fallbacks | + | ++ full control |

**Decision:** Trafilatura as the primary extractor; BeautifulSoup for post-processing (cleaning the extracted HTML, splitting into blocks for simplification).
**Justification:** solves D1 in the backend's own language without a second runtime (D8). BeautifulSoup is kept for small, targeted fixes rather than as the main extractor.
**Check:** extraction quality is measured on the test page set (test E1 in `09-verification-plan.md`). If a page type fails, Readability is evaluated as a fallback.

---

## 5. Decision 3 — Text simplification approach (ADR-0003)

| Criterion | Rule-based / lexical substitution (word lists, sentence splitting) | Hosted large-language-model API | Self-hosted open-weight model |
|---|---|---|---|
| Rewrites sentence structure, not only words (D2) | – limited to word replacement and splitting | ++ [L12, L15] | + depends on model size |
| Control of simplification level (D2) | + fixed rules per level | ++ instructions per level | + |
| Russian, Kazakh and English (D2) | – separate word lists per language | + all three; Kazakh quality must be measured | – small models are weaker in Kazakh |
| Risk of changing or inventing meaning (D2) | ++ low | – real risk; needs a check step [L13, L16] | – same risk |
| Infrastructure and hardware (D8) | ++ none | ++ none (pay per use) | – needs a GPU server |
| Latency (D5) | ++ very fast | + seconds per page section | depends on hardware |
| Privacy (D6) | ++ text stays on server | – text is sent to a third party | ++ text stays on server |
| Cost | ++ free | – per-request cost | – hardware cost |

**Decision:** hosted LLM API, wrapped behind a provider-independent interface in the backend (so the provider or a self-hosted model can be swapped without changing the rest of the system), plus deterministic rules as a safety layer (sentence length limits per level, keeping numbers, dates and names unchanged).
**Justification:** D2 requires rewriting text at a chosen level in three languages, which rule-based methods cannot do well; self-hosting is not feasible for the team's resources (D8).
**Mitigations for the weak points:**
- Meaning preservation: an automatic check after simplification (FR-07: numbers, dates and names of the original must appear in the output; a semantic-similarity score between original and output), the original shown next to the simplified text (FR-09), and human rating of meaning preservation in the evaluation (E2).
- Privacy (D6): no storage of pasted text; the interface states that text is processed by an external service (NFR-05); API keys only in environment variables (`config/.env.example` holds placeholders).
- Cost and latency (D5): cache results for public URLs for 24 hours (FR-18); limit text length per request (NFR-02).
**Provider selection:** the specific provider and model are not fixed. They are chosen by the comparison P1 in `09-verification-plan.md` on the same test texts (meaning-check pass rate, level compliance, quality in Kazakh, latency, cost, data-use terms).

---

## 6. Decision 4 — Data storage (ADR-0004)

There are no user accounts in the first version (see `01-proposal.md`). Server-side data is limited to: cached simplification results for public URLs (with expiry), anonymous ratings ("was this easier to read?"), and records of evaluation runs (metrics per text, level, language and provider). Settings and history live in the user's browser (FR-10, FR-13).

| Criterion | PostgreSQL | MongoDB | SQLite |
|---|---|---|---|
| Structured data: evaluation runs, ratings linked to requests (D3) | ++ relational schema and constraints | + flexible documents, weaker constraints | + relational |
| Semi-structured data (extracted page blocks, metrics) | + JSONB columns | ++ native | – limited |
| Concurrent use by a web service | ++ | ++ | – single-writer limits |
| Reporting for evaluation (aggregations by level, language, provider) (D3) | ++ SQL | + aggregation pipeline | + SQL |
| Expiry of cached entries (D6) | + scheduled delete by timestamp | ++ TTL indexes | + scheduled delete |
| Team experience (D8) | + coursework | ++ used in Postera | + |
| Runs in Docker for local development | ++ | ++ | ++ no server needed |

**Decision:** PostgreSQL.
**Justification:** the evaluation data is relational (runs, texts, metrics, ratings) and the evaluation needs aggregate queries by level, language and provider (D3). JSONB covers the semi-structured parts; cache expiry is a simple timestamp-based delete. MongoDB is closer to team experience and has TTL indexes, but offers fewer guarantees for the evaluation data. SQLite is used in automated tests.

---

## 7. Decision 5 — Interaction between client and backend (ADR-0005)

| Criterion | Synchronous REST (JSON) | REST + background jobs (request returns a job ID, client polls for the result) | Streaming (Server-Sent Events / WebSocket) |
|---|---|---|---|
| Simplicity to build and test (D4, D8) | ++ | + needs a task queue | – more complex client and server |
| Long operations: fetch page + extract + simplify several blocks (D5) | – risk of timeouts on long pages | ++ no timeouts | ++ partial results appear progressively |
| Documented API contract (D4) | ++ OpenAPI | ++ OpenAPI | + partly outside OpenAPI |
| Perceived speed for the user | + | + | ++ |

**Decision:** REST (JSON) as the API style. For the first version: synchronous requests with a text-length limit (NFR-02) and a progress indicator in the client. If test E4 shows that NFR-01 is not met, long requests move to background jobs (job ID + status endpoint).
**Justification:** simplest to build, document and test (D4, D8), with a clear upgrade path for D5. Streaming is postponed because it adds complexity before the response-time problem is measured.

---

## 8. Decision 6 — Frontend framework (ADR-0006)

| Criterion | React (JavaScript/TypeScript) | Vue (JavaScript/TypeScript) | Server-rendered templates (Jinja2 in FastAPI) |
|---|---|---|---|
| Interactive features in the browser: settings, side-by-side original, text-to-speech controls, history in browser storage (D7) | ++ component state | ++ component state | – needs separate hand-written JavaScript anyway |
| Accessibility tooling (D7) | ++ eslint-plugin-jsx-a11y, axe-core in tests | + eslint-plugin-vuejs-accessibility, axe-core | + axe-core on rendered pages |
| Interface in three languages (FR-14) | ++ mature i18n libraries | ++ mature i18n libraries | + gettext-style translations |
| Separation from the API (FR-15: the client uses the same public API) | ++ | ++ | – tends to bypass the API |
| Team experience (D8) | + JavaScript (Postera) | + JavaScript (Postera) | + Python, new for the team |
| Size of community and learning material | ++ | + | + |

**Decision:** React with TypeScript, built with Vite.
**Justification:** the reading view is a set of interactive blocks (simplified text, original, icons, read-aloud) that fit a component model (D7); the client must use the same documented API as other clients (FR-15); the team's JavaScript experience transfers. Vue is an equally valid technical option; React was chosen for the larger community and learning material (D8). Server templates were rejected because most features need client-side code anyway.

---

## 9. Supporting tools (not compared in depth)

| Purpose | Tool | Reason |
|---|---|---|
| Readability indicators | textstat (Python) for English and Russian formulas; own simple indicators for all three languages (average sentence length, average word length, share of long words) | textstat does not provide formulas for Kazakh; formulas are one of several measures, not the only one [L9] |
| Language detection | a language-identification library (e.g. lingua-py), with manual override in the interface | FR-06 |
| Containerisation | Docker / Docker Compose | Same environment for both team members and for the committee (NFR-09) |
| Tests | pytest (backend), Vitest and Playwright with axe-core (frontend) | NFR-03, NFR-08 |
| Continuous integration | GitHub Actions | Runs tests and a secret scan on each pull request (NFR-06, NFR-08) |
| Text-to-speech | Browser Web Speech API | No server cost; availability of a Kazakh voice depends on the user's browser and operating system, so FR-11 requires a clear message when no voice exists |

## 10. Risks of the chosen stack

| Risk | Impact | Mitigation |
|---|---|---|
| Python/FastAPI is new for the team | Slower start | Small first endpoints with tests; official FastAPI tutorial |
| LLM output changes meaning or adds content | Wrong information for vulnerable users | Automatic meaning check (FR-07); original next to simplified text (FR-09); human rating (E2) |
| Third-party LLM processes user text | Privacy | No storage of pasted text; notice to users; no content in logs (NFR-05); provider data-use terms are a selection criterion (P1) |
| Lower simplification quality in Kazakh | Unequal service across languages | Results reported per language (E2, E3); provider selection includes Kazakh |
| No Kazakh voice in some browsers | Text-to-speech unavailable for Kazakh texts | Clear message (FR-11); server-side speech considered for a later version |
| LLM provider cost or availability changes | Service stops working | Provider-independent interface; caching; clear error (NFR-07) |
| Content extraction fails on some sites | Missing or broken text | Test page set (E1); fallback extractor; user can paste text (FR-04) |

## 11. Architecture decision records

| ADR | Decision |
|---|---|
| [0001](decisions/0001-backend-fastapi.md) | Backend platform: FastAPI |
| [0002](decisions/0002-content-extraction-trafilatura.md) | Main-content extraction: Trafilatura |
| [0003](decisions/0003-simplification-hosted-llm.md) | Simplification: hosted LLM behind a provider interface, with a rule-based safety layer |
| [0004](decisions/0004-storage-postgresql.md) | Storage: PostgreSQL, no user accounts |
| [0005](decisions/0005-api-synchronous-rest.md) | Client–backend interaction: synchronous REST with an upgrade path |
| [0006](decisions/0006-frontend-react.md) | Frontend: React with TypeScript |

## 12. Sources for technology facts

- FastAPI documentation: https://fastapi.tiangolo.com
- Django REST Framework: https://www.django-rest-framework.org
- Trafilatura documentation: https://trafilatura.readthedocs.io
- Mozilla Readability: https://github.com/mozilla/readability
- Beautiful Soup documentation: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- PostgreSQL JSON types: https://www.postgresql.org/docs/current/datatype-json.html
- textstat: https://pypi.org/project/textstat/
- React: https://react.dev
- Vue: https://vuejs.org
- eslint-plugin-jsx-a11y: https://github.com/jsx-eslint/eslint-plugin-jsx-a11y
- axe-core: https://github.com/dequelabs/axe-core
- Web Speech API (MDN): https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API

Facts about tools are taken from their official documentation at a general level; versions and pricing are intentionally not stated and are checked at implementation time.
