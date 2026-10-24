# 06 — Requirements and Acceptance Criteria

**YouTrack task:** DIP-7

Priorities follow MoSCoW: **M** — must have (the first version is not accepted without it), **S** — should have, **C** — could have, **W** — won't have in the first version. Every Must and Should requirement has an acceptance criterion that can be checked by a test or a measurement; the test IDs (T-x, E-x) refer to `09-verification-plan.md`. Sources: L-numbers refer to `03-literature-review.md`, A-numbers to `02-relevance-evidence.md`.

## 1. Users and terms

- **Reader** — a person with aphasia, or a relative/therapist acting with them.
- **Block** — one heading, paragraph or list item of the extracted text; simplification and the meaning check work per block.
- **Level** — simplification level: **1 Light**, **2 Medium**, **3 Strong**.
- **Test page set** — at least 15 public pages (5 per language: news, government service, health information), fixed in `tests/data/pages.csv` before implementation; see `09-verification-plan.md`.

### Simplification levels

| Level | Sentence length (words, maximum per sentence) | Other rules |
|---|---|---|
| 1 Light | 20 | Replace rare or technical words with common ones; keep paragraph structure |
| 2 Medium | 15 | As level 1; one idea per sentence; explain unavoidable terms in brackets |
| 3 Strong | 10 | As level 2; key points first; paragraphs turned into short bullet lists |

For all levels: numbers, dates, names and amounts are kept exactly (FR-07).

## 2. Functional requirements

| ID | Requirement | Priority | Acceptance criterion | Source | Verification |
|---|---|---|---|---|---|
| FR-01 | The reader can submit the URL of a public web page (http/https). | M | A valid URL of a page from the test page set returns a simplified result; an invalid URL (wrong format) shows an error message in the interface language without sending a request to the server. | Proposal | T-API-01, T-UI-01 |
| FR-02 | The reader can paste text instead of a URL (up to 10,000 characters). | M | Pasted text of 1–10,000 characters is simplified; longer text is rejected with a message stating the limit. | Proposal | T-API-02 |
| FR-03 | The system extracts the main content of the page: headings, paragraphs and lists, without navigation, adverts, cookie banners and footers. | M | On the test page set, at least 80 % of pages are rated "complete" by two reviewers (all main paragraphs present, no menu or advert blocks). | L14 | E1 |
| FR-04 | The system handles pages it cannot process (login required, non-HTML, timeout, too little text) with a clear message and an offer to paste text. | M | For each of the four cases a test returns the defined error code and the interface shows the matching message within 12 s. | Proposal (out of scope list) | T-API-03 |
| FR-05 | The reader selects one of three simplification levels; the output follows the rules of the level. | M | On the test page set, at least 90 % of simplified sentences respect the maximum sentence length of the selected level; level 3 output contains a bullet list for every paragraph longer than two sentences. | L2, L8, L11; consultation B1 | E2, T-PIPE-01 |
| FR-06 | Input text in Russian, Kazakh or English is detected automatically; the output is in the same language. The reader can override the detected language. | M | Language is detected correctly for all texts of the test page set; with a manual override the output is in the chosen language. | Proposal | T-PIPE-02 |
| FR-07 | After simplification the system checks each block: every number, date and proper name from the original appears in the output, and the semantic similarity is above a threshold. Blocks that fail are marked with a warning, and the original block is shown. | M | Unit tests with prepared pairs (missing number, changed date, removed name, unchanged text) give the expected pass/fail result in 100 % of cases; on the test page set the share of flagged blocks is reported. | L13, L16 | T-PIPE-03, E2 |
| FR-08 | The reading view shows one block at a time in a clear layout: base font size at least 18 px, line spacing at least 1.5, left-aligned text, no images except optional icons, headings kept. | M | Automated UI test checks computed styles on the reading view; no element from the original page other than text is rendered. | L3, L5, L8 | T-UI-02 |
| FR-09 | The reader can show the original text next to (or under, on narrow screens) the simplified block, per block or for the whole page. | M | Toggle shows and hides the original for one block and for all blocks; works by mouse, touch and keyboard. | Risk of meaning loss; L13 | T-UI-03 |
| FR-10 | Display settings: font size (3 steps), line spacing (2 steps), high-contrast theme. Settings are kept in the browser and applied on the next visit. | S | After changing a setting and reloading the page, the setting is still applied; no request to the server contains the settings. | L4, L5, L17 | T-UI-04 |
| FR-11 | Text-to-speech reads the simplified text aloud with play, pause, stop and speed (0.5×–1.5×); the sentence being read is highlighted. If the browser has no voice for the language, a message says so. | S | In Chrome and Edge, play/pause/stop and speed change work for an English and a Russian text; with no voice available the message appears instead of an error. | L10, L17 | T-UI-05 |
| FR-12 | Icons with captions: the reader can switch on icons for key concepts of a block, taken from a fixed open-licensed icon set; each icon has a text caption. Off by default. | S | With the setting off no icons are shown; with it on, every shown icon has a visible caption and alt text. Effect on reading is measured, not assumed. | L5 | T-UI-06, E3 |
| FR-13 | History: the last 20 simplified pages (title, URL or first words, level, date) are kept in the browser only; the reader can open an item again or clear the history. | S | After 21 simplifications the list contains 20 items; "Clear history" empties it; the server stores no history. | Proposal | T-UI-07 |
| FR-14 | The interface is available in Russian, Kazakh and English; the reader can switch the interface language at any time. | M | Every visible interface string exists in the three translation files (automated check for missing keys); switching language changes all strings without reloading. | Proposal | T-UI-08 |
| FR-15 | The backend offers a REST API documented with OpenAPI: `POST /api/v1/simplify`, `GET /api/v1/languages`, `POST /api/v1/feedback`, `GET /api/v1/health`. The web client uses only this API. | M | OpenAPI schema is available at `/docs`; contract tests cover every endpoint, including error responses in one defined JSON error format. | Proposal; gap in `04-existing-systems-comparison.md` | T-API-04 |
| FR-16 | The API returns readability indicators for the original and the simplified text: number of sentences, average sentence length, average word length, share of long words; for English and Russian also a readability formula score. | S | For a prepared text with known values the API returns the expected indicators (tolerance 1 %). | L9 (formulas alone are not enough) | T-PIPE-04 |
| FR-17 | After reading, the reader can answer "Was this easier to read?" (yes / no / partly). The answer is stored anonymously with level and language for the evaluation. | C | The answer is saved without IP address or text content. | Evaluation | T-API-05 |
| FR-18 | Results for the same public URL, level and language are cached for 24 hours. | C | The second identical request within 24 h is answered from the cache; after 24 h the entry is deleted. | NFR-01 | T-API-06 |
| FR-19 | User accounts and server-side profiles. | W | — | Scope decision | — |
| FR-20 | Export of the simplified text to PDF or print layout. | W | — | Scope decision | — |

## 3. Non-functional requirements

| ID | Category | Requirement | Priority | Acceptance criterion | Verification |
|---|---|---|---|---|---|
| NFR-01 | Performance | Time from request to complete result for main text up to 1,500 words. | M | 90 % of requests in test E4 complete in ≤ 15 s; the interface shows a progress indicator after 1 s. | E4 |
| NFR-02 | Limits | Main text longer than 5,000 words is cut to the first 5,000 words; page download limited to 5 MB and 10 s. | M | Tests with a 6,000-word page and a slow/large page return the documented notice or error. | T-API-07 |
| NFR-03 | Accessibility | The web client meets WCAG 2.2 level AA on the key screens (input, reading view, settings, history) and can be used with keyboard only. | M | axe-core reports 0 serious or critical violations on each key screen; a manual keyboard-only walkthrough of scenario S1 succeeds; a manual check with a screen reader (NVDA) is recorded. | T-A11Y-01..03 |
| NFR-04 | Usability | Plain interface: one primary action per screen, touch targets at least 44 × 44 px, interface texts written in plain language, no time limits. | M | UI test checks target sizes; interface texts reviewed against the COGA patterns [L3]; usability evaluation E3. | T-UI-09, E3 |
| NFR-05 | Privacy | Pasted text is not stored on the server; logs contain no text content or full URLs with query strings; only public URL results are cached (24 h); the interface states that text is processed by an external AI service. | M | Code review checklist and a test that inspects logs and the database after a pasted-text request find no text content; the notice is visible on the input screen. | T-SEC-01 |
| NFR-06 | Security | Secrets only in environment variables; secret scanning in CI; URL fetching blocks localhost and private network addresses (SSRF protection) and follows at most 3 redirects. | M | CI fails on a committed test secret; requests to `http://127.0.0.1`, `http://10.0.0.1` and a redirect chain of 4 are rejected. | T-SEC-02, T-SEC-03 |
| NFR-07 | Reliability | If the language-model provider fails or times out, the system retries once and then returns a clear error; the original extracted text is still shown. | M | With the provider mocked to fail, the API returns the defined error within 20 s and the client shows the original text with a message. | T-API-08 |
| NFR-08 | Maintainability | The language-model provider is behind one interface; core backend modules have at least 70 % line coverage; CI runs all tests on each pull request. | S | Swapping the provider in configuration needs no code change outside the adapter; coverage report ≥ 70 %; CI status visible on pull requests. | T-CI-01 |
| NFR-09 | Portability | The system starts locally with one command (`docker compose up`). | S | On a clean machine with Docker, the documented command starts backend, frontend and database and `/api/v1/health` returns OK. | T-CI-02 |
| NFR-10 | Compatibility | Latest versions of Chrome, Firefox, Edge and Safari; screen width from 360 px without horizontal scrolling. | S | End-to-end scenario S1 passes in Chromium, Firefox and WebKit (Playwright) at 360 px and 1280 px width. | T-UI-10 |
| NFR-11 | Safety of content | The interface states that the platform is a reading aid and not medical advice; for blocks flagged by FR-07 the reader is told to check the original. | M | Notice visible in the footer of the reading view; flagged blocks show the warning (UI test). | T-UI-11 |

## 4. Key user scenarios

| ID | Scenario | Requirements |
|---|---|---|
| S1 | A reader pastes the link to a health information page, chooses level 2 and reads the simplified text, opening the original for one block. | FR-01, FR-03, FR-05, FR-07, FR-08, FR-09 |
| S2 | A relative pastes text from a message in Kazakh, chooses level 3 and lets the reader listen to it at 0.75× speed. | FR-02, FR-05, FR-06, FR-11 |
| S3 | A therapist increases the font size, switches on high contrast and icons, and later reopens a page from history. | FR-10, FR-12, FR-13 |
| S4 | A reader submits a page that needs a login and is asked to paste the text instead. | FR-04 |
| S5 | A developer of another client calls the API and reads the documentation. | FR-15, FR-16 |

## 5. Traceability summary

| Objective (`01-proposal.md`) | Requirements |
|---|---|
| Extraction of main content | FR-01–FR-04, NFR-02, NFR-06 |
| Simplification at a selectable level with a meaning check | FR-05–FR-07, NFR-07 |
| Clear reading view and settings | FR-08–FR-14, NFR-03, NFR-04, NFR-10 |
| Documented API | FR-15, FR-16, NFR-08 |
| Evaluation against a baseline | FR-16, FR-17, `09-verification-plan.md` |
| Privacy and safety | NFR-05, NFR-06, NFR-11 |

## 6. Assumptions behind the requirements

FR-05 (three levels) is supported by the literature and by one consultation with a speech-language therapist (B1 in `02-relevance-evidence.md`), which needs confirmation by further conversations. FR-12 (icons) and the choice of three languages rest on assumptions not yet confirmed by user representatives (rows 10–11). They are kept because the literature supports configurable help [L4, L5, L17]; the usability evaluation E3 checks them. If conversations or E3 contradict them, the requirement is changed and the change is recorded in `11-supervisor-feedback.md` or a new ADR.
