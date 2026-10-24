# Comparison of Existing Systems

> YouTrack task: DIP-5.

## 1. Systems compared

| ID | System | What it is | Sources |
|---|---|---|---|
| S1 | Microsoft Immersive Reader | Reading tool built into Microsoft products; changes how text is displayed. | Microsoft Support (Teams, education pages); university help pages |
| S2 | Rewordify | Free website that replaces hard words and phrases with easier ones; accepts pasted text or a URL. | rewordify.com help pages; educator reviews |
| S3 | CommuniKey (University of Queensland, QARC) | Browser extension co-designed with people with aphasia, with eight reading-support features including AI summaries. | UQ "Bridging the Digital Divide" project summary (Nov 2025); ANZSOC 2026 abstract |
| S4 | HAPPI (University of Sunderland) | Early web-based text simplification system for people with aphasia, based on lexical (synonym) simplification with WordNet. | Devlin & Unthank (2006), "Helping aphasic people process online information", ASSETS '06 (L19) |
| S5 | General-purpose LLM chat assistants (category) | Chat assistants that can rewrite text on request. Treated as a category; behaviour varies by product. | L12, L15 for simplification quality |

## 2. Criteria

| ID | Criterion | Why it matters |
|---|---|---|
| C1 | Designed for people with aphasia | Fit with the target users |
| C2 | Rewrites the text itself (not only the display) | The project's core function |
| C3 | Adjustable simplification level | Severity varies between users [L2, L8] |
| C4 | Works on web pages | The project's input |
| C5 | Display adjustments and distraction reduction | Layout affects reading [L8] |
| C6 | Read-aloud / text-to-speech | Combined modality preferred by many [L10] |
| C7 | Checks or evidence on meaning preservation | Meaning must survive simplification [L13, L16] |
| C8 | Availability | Whether people can use it today |
| C9 | Evidence from users with aphasia | Whether it has been tested with the target group |

## 3. Comparison

| Criterion | S1 Immersive Reader | S2 Rewordify | S3 CommuniKey | S4 HAPPI | S5 LLM chat assistants |
|---|---|---|---|---|---|
| C1 Designed for aphasia | No (general reading support) | No (vocabulary and English learning) | Yes | Yes | No |
| C2 Rewrites the text | No: changes how text appears, not the text | Yes: word and phrase replacement | Partly: AI summaries; rewriting at a chosen reading level not described | Yes: lexical (synonym) simplification | Yes, on request |
| C3 Adjustable level | Not applicable | Yes: six levels (Easiest to Hardest) based on word frequency | Summaries are customisable; reading-level control not described | Not described | Yes, by prompting; user must know how to ask |
| C4 Works on web pages | Yes (web pages, PDFs, Office apps) | Yes: URL or pasted text | Yes (browser extension) | Web-based; aimed at online content such as news | Pasted text always; URL reading varies by product |
| C5 Display adjustments | Yes: size, spacing, font, colours, line focus | Limited: highlighting modes | Yes: Simple View, resize text, hide images, hide text | Not described | No dedicated reading settings |
| C6 Read-aloud | Yes, with word highlighting | Per-word audio only | Yes: Read Along, customisable | Not described | Varies by product |
| C7 Meaning-preservation check | None found | None; its help page says it cannot rewrite to a target grade level | Not described | Not described | None built in; quality studied in general texts [L12, L15] |
| C8 Availability | Built into Microsoft products | Free website | Not yet publicly available in the sources reviewed (project summary, Nov 2025: "soon") | Research prototype; availability not found | Widely available; free and paid tiers vary |
| C9 Evidence with people with aphasia | None found | None found | Yes: usability testing (8 people with aphasia, 5 family members, 6 professionals) and a two-week field trial (16 participants with aphasia in the abstract) | Not found in the source reviewed | None found for this group |

## 4. Findings

1. **CommuniKey is the closest existing work.** It is aphasia-specific, co-designed with people with aphasia, and combines distraction reduction, resizing, read-aloud and AI summaries. Participants in usability testing found Summarise and customisable text-to-speech helpful, and Summarise was the most used feature in the preliminary field-trial data. As of the sources reviewed it is not publicly available, and its documentation describes summaries rather than simplification to a chosen reading level.
2. **General reading tools** (Immersive Reader) adjust presentation well but do not rewrite text, so complex wording remains.
3. **Word-replacement tools** (Rewordify, HAPPI) offer lexical simplification but not sentence-level restructuring; Rewordify states that it cannot rewrite text to a target grade level.
4. **General-purpose LLM assistants** can rewrite text at a requested level [L12, L15], but they are not designed for people with aphasia, require the user to write prompts, and offer no aphasia-oriented reading view or meaning check.
5. **No system reviewed combines all of the following:** extraction of a web page's main content, rewriting at an adjustable reading level, an aphasia-oriented reading view, a documented API, and an evaluation that measures comprehension. This is a statement about the systems reviewed, not about every tool that exists.

## 5. Gap the project targets

The project does not claim to be the first aphasia web tool; CommuniKey shows that the problem is recognised and addressed. The intended contribution is narrower:

- **Level-controlled rewriting** of page content (with an explicit check on meaning) rather than summaries or word replacement only.
- **A documented API and backend** that other clients can use.
- **An evaluation plan** that measures comprehension accuracy and reading time, not only readability scores [L9].

Risk: overlap with CommuniKey's summarisation. The project's differentiation must be shown in the evaluation (`09-verification-plan.md`, DIP-10), and the final report should position the platform as complementary.

## 6. How the gap becomes requirements

| Gap | Requirement in `06-requirements.md` |
|---|---|
| Rewriting at an adjustable level | FR-05 |
| Explicit meaning check | FR-07, FR-09 |
| Works on arbitrary pages, not only in one product | FR-01, FR-03 |
| Aphasia-oriented reading view with settings and read-aloud | FR-08, FR-10, FR-11 |
| Documented API | FR-15 |
| Evaluation of comprehension, not only formulas | `09-verification-plan.md`, E3 |

## 7. References for this comparison

1. Microsoft. *Use Immersive Reader in Microsoft Teams.* https://support.microsoft.com/en-US/education/use-immersive-reader-in-microsoft-teams
2. Rewordify. *Rewordifying level (help).* https://rewordify.com/helprewordifyinglevel.php
3. Queensland Aphasia Research Centre, University of Queensland. *Bridging the Digital Divide: celebration event summary* (24 Nov 2025). https://shrs.uq.edu.au/files/22409/DigitalDivide_ProjectSummary.pdf
4. Wallace, S. J., et al. (2026). Usability, feasibility, and preliminary efficacy of CommuniKey: a browser extension for web accessibility in post-stroke aphasia. Oral presentation, ANZSOC 2026. https://anzsoc-2026.p.asnevents.com.au/days/2026-08-19/abstract/138899
5. Devlin, S., & Unthank, G. (2006). *Helping aphasic people process online information.* Proc. ASSETS '06, 225–226. https://doi.org/10.1145/1168987.1169027
6. Kew et al. (2023); Qiang et al. (2025) as in `03-literature-review.md` (L12, L15).
