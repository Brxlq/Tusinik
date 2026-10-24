# 08 — UX Prototype of Key Scenarios

**YouTrack task:** DIP-9

## 1. Where the prototype is

- Clickable prototype: [`../prototype/index.html`](../prototype/index.html). Download the repository (or the tagged version) and open the file in a browser; no server or installation is needed.
- Screenshots of every screen: [`../prototype/screens/`](../prototype/screens/).

The prototype is static HTML, CSS and JavaScript. It does not call the backend: the sample page ("Flu vaccine" from the fictional *Riverside Health Centre*, `riverside-health.example`) and its simplified versions are written by hand to show the intended behaviour. The sample text is invented and is not medical advice. Read-aloud uses the browser's real Web Speech API when available.

## 2. Screens

| Screen | Screenshot | What it shows | Requirements |
|---|---|---|---|
| Input | [01-input.png](../prototype/screens/01-input.png) | URL or pasted text, three levels with plain descriptions, one primary button, privacy and "not medical advice" notice | FR-01, FR-02, FR-05, NFR-04, NFR-05, NFR-11 |
| Error: page needs login | [02-error.png](../prototype/screens/02-error.png) | Plain-language message and a direct way to paste text instead | FR-04 |
| Processing | (shown for about 1.5 s after "Make it easier") | Progress steps: open page, find main text, simplify and check meaning | NFR-01 |
| Reading view, level 2 | [03-reading-level2.png](../prototype/screens/03-reading-level2.png) | One block per card, large text, toolbar with listen/stop/speed, show original, icons, level switch; readability indicators; "Was this easier to read?" | FR-05, FR-07, FR-08, FR-11, FR-16, FR-17 |
| Reading view, level 3 with original and icons | [04-reading-level3-original-icons.png](../prototype/screens/04-reading-level3-original-icons.png) | Bullet points at level 3; original next to each block; icons with captions; a block flagged by the meaning check with a warning | FR-05, FR-07, FR-09, FR-12 |
| Settings | [05-settings.png](../prototype/screens/05-settings.png) | Text size, line spacing, high contrast, icons on/off; saved only in the browser | FR-10, FR-12 |
| History | [06-history.png](../prototype/screens/06-history.png) | Recent pages with level and date; open again; clear history | FR-13 |
| Reading view on a phone (375 px) | [07-reading-mobile.png](../prototype/screens/07-reading-mobile.png) | Same content without horizontal scrolling; original appears under the block | NFR-10 |

The interface language can be switched between RU, KZ and EN on every screen (FR-14).

## 3. Key scenarios in the prototype

| Scenario (`06-requirements.md`) | How to walk through it |
|---|---|
| S1 — simplify a page by URL, open the original | Input → keep the example link → level "Medium" → **Make it easier** → in the reading view press **Original** on a block |
| S2 — listen to a simplified text at lower speed | Reading view → choose speed 0.75× → **Listen** (the sentence being read is highlighted) |
| S3 — adjust display and reopen from history | **Settings** → larger text, high contrast, icons on → **History** → **Open** |
| S4 — page that needs a login | Input → type a link containing "login" → **Make it easier** → error message → **Paste text** |

## 4. Design decisions and their basis

| Decision | Basis |
|---|---|
| Base font 20 px, line spacing 1.6, left-aligned text, one block per card | FR-08; aphasia-friendly formatting [L4, L5, L8]; W3C COGA clear-layout patterns [L3] |
| Plain words in the interface ("How simple?", "Make it easier") and one primary action per screen | NFR-04; COGA [L3] |
| Original text always one click away, and shown automatically for flagged blocks | FR-07, FR-09; meaning-loss risk [L13, L16] |
| Icons off by default, always with a text caption | FR-12; pictures may distract [L5] |
| Read-aloud with adjustable speed and sentence highlighting | FR-11; combined modality preferred [L10]; speed control matters [L17] |
| Buttons at least 44 × 44 px, visible keyboard focus, skip link | NFR-03, NFR-04 |
| Settings and history only in the browser, stated on screen | NFR-05; no accounts in the first version |

## 5. Limitations and next steps

- The prototype has not yet been shown to people with aphasia, relatives or therapists. It will be used as the material for the conversations in `02-relevance-evidence.md` and for the first round of usability evaluation (E3 in `09-verification-plan.md`).
- Automated accessibility checks (axe-core) and a screen-reader walkthrough are planned for the implemented React client (T-A11Y-01..03), not for this static prototype.
- The final icon set has not been chosen; the prototype uses simple line icons drawn for the demo.
