# 09 — Verification and Evaluation Plan

**YouTrack task:** DIP-10

Two questions are answered separately:

1. **Verification** — does the system meet its requirements (`06-requirements.md`)? Answered by automated tests and measurements (sections 3–5).
2. **Evaluation** — is the simplified text actually easier to read for the target users, and does it keep the meaning, compared with the original page? Answered by experiments E1–E4 and the usability study (sections 6–8).

## 1. Baseline

| Baseline | Used in | Why |
|---|---|---|
| **B0 — original text** of the page (main content as extracted, no simplification) | E2, E3 | The reader's current situation; every gain is measured against it |
| **B1 — single prompt**: the same language model asked once "simplify this text" without level rules, safety layer or meaning check | E2 | Shows what the pipeline adds compared with what a reader could do in a chat assistant (system S5 in `04-existing-systems-comparison.md`) |

## 2. Test data

| Data set | Content | Prepared by | Location |
|---|---|---|---|
| **Test page set** | 15 public pages: 5 Russian, 5 Kazakh, 5 English; in each language 2 news, 2 government service, 1 health information. Saved as HTML snapshots on the date of selection so results are repeatable | Both members, agreed before implementation | `tests/data/pages.csv` + `tests/data/snapshots/` |
| **Meaning-check pairs** | ≥ 30 hand-made (original, simplified) pairs per language with known defects: missing number, changed date, removed name, changed negation, unchanged text | Backend developer | `tests/data/meaning_pairs.jsonl` |
| **Comprehension texts** | 6 short texts (≈150–250 words) from the test page set, each with 4 multiple-choice questions on main ideas and details; questions checked by the supervisor | Both members | `tests/data/comprehension/` |

Snapshots are used for automated tests; only public pages are used, and no personal data is stored.

## 3. Automated tests (verification)

| ID | What is tested | Requirement | Tool | Pass condition |
|---|---|---|---|---|
| T-API-01 | Valid and invalid URL handling | FR-01 | pytest + FastAPI test client | Expected status codes and error codes |
| T-API-02 | Pasted text limits (0, 1, 10,000, 10,001 characters) | FR-02 | pytest | Accept / reject as specified |
| T-API-03 | Login page, non-HTML file, timeout, too little text | FR-04 | pytest with mocked HTTP | Defined error code for each case |
| T-API-04 | Contract tests for every endpoint against the OpenAPI schema | FR-15 | pytest + schema validation | All responses match the schema, including errors |
| T-API-05 | Feedback stored without IP or text | FR-17 | pytest | Stored record has only level, language, answer, time |
| T-API-06 | Cache hit within 24 h, expiry after 24 h | FR-18 | pytest with frozen time | Second call served from cache; expired entry deleted |
| T-API-07 | 6,000-word page; 6 MB page; slow page | NFR-02 | pytest with mocked HTTP | Truncation notice; size and time errors |
| T-API-08 | Provider failure and timeout | NFR-07 | pytest with mocked provider | One retry, then defined error within 20 s; original text returned |
| T-PIPE-01 | Level rules: sentence length and level-3 lists | FR-05 | pytest on recorded provider outputs | ≥ 90 % of sentences within the limit |
| T-PIPE-02 | Language detection and override | FR-06 | pytest | All test texts detected correctly |
| T-PIPE-03 | Meaning checker on the meaning-check pairs | FR-07 | pytest | 100 % expected pass/fail on prepared pairs |
| T-PIPE-04 | Readability indicators on a text with known values | FR-16 | pytest | Within 1 % of expected values |
| T-UI-01 | URL validation in the client | FR-01 | Vitest | Invalid URL shows message, no request sent |
| T-UI-02 | Reading view styles (font ≥ 18 px, line spacing ≥ 1.5) | FR-08 | Playwright | Computed styles match |
| T-UI-03 | Show/hide original per block and for all blocks, by keyboard | FR-09 | Playwright | Toggle works with mouse and keyboard |
| T-UI-04 | Settings kept after reload, not sent to server | FR-10 | Playwright | Setting applied after reload; no request contains settings |
| T-UI-05 | Read-aloud controls; message when no voice | FR-11 | Playwright (Chromium) + manual check in Chrome and Edge | Controls work; message shown when voice list is empty |
| T-UI-06 | Icons off by default; captions and alt text when on | FR-12 | Playwright | As specified |
| T-UI-07 | History limit 20 and clear | FR-13 | Playwright | As specified |
| T-UI-08 | Missing translation keys | FR-14 | Script in CI | 0 missing keys in RU, KZ, EN |
| T-UI-09 | Touch-target size ≥ 44 × 44 px | NFR-04 | Playwright | All interactive elements pass |
| T-UI-10 | Scenario S1 in Chromium, Firefox, WebKit at 360 px and 1280 px | NFR-10 | Playwright | Passes; no horizontal scroll |
| T-UI-11 | Disclaimer and warning on flagged blocks | NFR-11 | Playwright | Visible |
| T-SEC-01 | No pasted text in logs or database after a request | NFR-05 | pytest (inspects log capture and database) | No match for the submitted text |
| T-SEC-02 | SSRF: localhost, private ranges, redirect chain of 4 | NFR-06 | pytest | All rejected |
| T-SEC-03 | Secret scanning | NFR-06 | Secret scanner in GitHub Actions | CI fails on a planted test secret in a test branch |
| T-A11Y-01 | Automatic accessibility check of key screens | NFR-03 | axe-core with Playwright | 0 serious or critical violations |
| T-A11Y-02 | Keyboard-only walkthrough of S1–S3 | NFR-03 | Manual, checklist | All steps possible, focus always visible |
| T-A11Y-03 | Screen-reader walkthrough of S1 | NFR-03 | Manual, NVDA + Chrome | Headings, buttons and warnings announced correctly |
| T-CI-01 | Tests and coverage on every pull request | NFR-08 | GitHub Actions | Green; coverage ≥ 70 % for core backend modules |
| T-CI-02 | One-command start on a clean machine | NFR-09 | Manual, documented | `/api/v1/health` returns OK |

## 4. Provider selection (P1)

Before the simplification pipeline is finalised, 2–3 language-model providers are compared on the test page set at all three levels. Criteria and weights:

| Criterion | Measure | Weight |
|---|---|---|
| Meaning preservation | Share of blocks passing the meaning check (FR-07) | 30 % |
| Level compliance | Share of sentences within the level limit (FR-05) | 20 % |
| Quality in Kazakh | Human rating of 20 Kazakh blocks (1–5 scale, two raters) | 20 % |
| Latency | 90th percentile time per 1,500 words | 15 % |
| Cost and data-use terms | Cost per 1,000 pages; whether submitted data may be used for training | 15 % |

The result and the chosen provider are recorded as an ADR.

## 5. Performance measurement (E4)

- **What:** end-to-end server time of `POST /api/v1/simplify` for texts of 300, 800 and 1,500 words, 20 requests each, cache disabled.
- **Conditions:** backend in Docker on one machine (CPU, RAM and network recorded with the results); the selected provider; the same time of day for all runs.
- **Metric:** median and 90th percentile; target (NFR-01) is 90th percentile ≤ 15 s for 1,500 words.
- **If the target is not met:** move to background jobs (ADR-0005) and repeat E4.

## 6. Extraction quality (E1)

- **What:** main-content extraction (FR-03) on the 15 pages of the test page set.
- **Method:** two team members independently rate each page as *complete* (all main paragraphs present, no navigation or advert blocks), *partial* or *failed*; disagreements are discussed and recorded.
- **Metric:** share of *complete* pages; target ≥ 80 %. Agreement between raters is reported.

## 7. Simplification quality and meaning (E2)

- **Compared:** B0 (original), B1 (single prompt), and Tusinik levels 1, 2 and 3, on all blocks of the test page set.
- **Automatic indicators:** average sentence length, average word length, share of long words (all three languages); Flesch-type score for English and Russian; text length.
- **Meaning:** share of blocks passing the automatic check (FR-07); plus human rating of a random sample of 60 blocks (20 per language) by two raters on two 1–5 scales: *meaning kept* and *easier to read*, without knowing which system produced the text.
- **Expected result:** Tusinik levels reduce sentence length and long-word share compared with B0, and keep meaning at least as often as B1 (fewer flagged blocks and higher *meaning kept* rating).
- **Note:** readability indicators are reported but are **not** treated as proof of easier comprehension, because one study found no significant effect of readability on comprehension in aphasia [L9]. Comprehension is measured in E3.

## 8. Comprehension and usability with users (E3)

- **Participants:** 3–5 people with aphasia (with a relative or therapist present) and/or 3–5 representatives (speech-language therapists, relatives). Recruitment as in `02-relevance-evidence.md`; participation voluntary and anonymous; ethics requirements confirmed with the supervisor first.
- **Design (within-subject):** each participant reads 4 of the 6 comprehension texts — 2 in the original (B0) and 2 simplified at the level they choose — in a balanced order, so each text appears in both conditions across participants. Texts are in the participant's preferred language.
- **Measures:**
  - comprehension accuracy: share of correct answers to 4 questions per text (main ideas and details separately, following [L9]);
  - reading time per text (measured by the interface or a stopwatch);
  - preference: "Which version was easier?" and the in-app rating (FR-17);
  - usability: tasks S1–S3 completed without help (yes/no), problems observed; short questionnaire with pictorial 5-point scale; for representatives also the System Usability Scale (SUS).
  - icons: whether participants switch icons on and whether they report them as helpful or distracting (FR-12, [L5]).
- **Conditions:** same device (laptop with 15" screen) and browser; quiet room or video call with screen sharing; no time pressure; supporter allowed to read the questions aloud.
- **Analysis:** with so few participants, results are reported descriptively per participant (accuracy and time in B0 vs simplified) and not generalised; observed usability problems become YouTrack tasks.
- **If no participants with aphasia can be recruited in time:** the study runs with representatives only, and this limitation is stated in the final report.

## 9. Results log

| Date | Experiment / test | Result | Action taken |
|---|---|---|---|
| — | No measurements yet (planning stage) | — | — |

## 10. Schedule (planned)

| Period | Activity |
|---|---|
| Before implementation of the pipeline | Fix test page set; prepare meaning-check pairs; run P1 |
| During implementation | Automated tests T-* in CI on every pull request |
| After the first working version | E1, E2, E4 |
| Before the final pre-defense | E3 with users or representatives; results added to section 9 |
