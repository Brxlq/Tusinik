# Project Proposal

> YouTrack task: DIP-2.

## 1. Project title

- **EN:** Development of Aphasia-Friendly Web Page Simplifier platform
- **RU:** Разработка платформы для упрощения веб-страниц, адаптированной для людей с афазией
- **KZ:** Афазиясы бар адамдарға ыңғайлы веб-бет мазмұнын жеңілдету платформасын әзірлеу

Working product name: **Tusinik** (from Kazakh «түсінік», "understanding").

## 2. Team and supervisor

| Name | Group | GitHub | Role | Responsibility |
|---|---|---|---|---|
| Sovet Yerkebulan Talgatuly | SE-2430 | [Brxlq](https://github.com/Brxlq) | Backend Developer | Backend API (FastAPI), content extraction, simplification pipeline and meaning check, data storage, technology decisions, requirements |
| Sadenov Ernar Aidynovich | SE-2430 | [AIeeph](https://github.com/AIeeph) | Frontend Developer | Web client (React), reading view and display settings, text-to-speech and icons in the interface, prototype, diagrams, verification plan |

**Supervisor:** Mirzagalikova Botagoz Vitalievna

Programme: Software Engineering, Astana IT University, group SE-2430.

## 3. Problem statement

Aphasia is an acquired language disorder, most often caused by stroke, that impairs the expression and understanding of language as well as reading and writing [1][2]. Its severity ranges from mild word-finding difficulties to severe deficits in comprehension and expression [2].

Written information can therefore be hard for people with aphasia to use. Interview studies with people with aphasia have identified content and design characteristics of printed text that act as barriers or facilitators to reading [4]. Aphasia-friendly formatting has been reported to improve comprehension of written material, although not everyone prefers it, and decorative pictures such as ClipArt may distract rather than help [5]. Because severity and reading ability differ between individuals [2][4], a single fixed rewrite is unlikely to suit everyone, which is the project's rationale for a configurable level of simplification.

Web accessibility work has traditionally concentrated on the interface for people with sensory and physical impairments. The W3C note on cognitive and learning disabilities provides supplemental guidance for web content, including clear language, but this guidance is not required for WCAG conformance [3].

Aphasia-specific web tools already exist, for example the CommuniKey browser extension, which provides distraction reduction, resizing, read-aloud and AI summaries [7]. In the systems reviewed in `04-existing-systems-comparison.md`, none combines rewriting at an adjustable reading level, a check that meaning is preserved, a documented API and a comprehension-based evaluation.

**Problem in one sentence:** people with aphasia lack a tool that takes an arbitrary web page and turns it into a version they can read, with a controllable level of simplification and without losing the meaning of the original.

## 4. Target users

1. **People with aphasia** (primary users). Severity and reading ability vary widely between individuals [2][4].
2. **Relatives and caregivers**, who often help find and explain information.
3. **Speech-language therapists**, who may use the tool in therapy and to prepare materials.

Secondary beneficiaries: any reader who benefits from plain-language text.

## 5. Aim

To design, implement and evaluate a web platform that converts the content of a web page (or pasted text) into an aphasia-friendly version with configurable simplification, and to demonstrate whether the simplified text is easier to read while keeping the original meaning.

## 6. Objectives

1. Establish the needs of the target users through conversations with 3–5 user representatives (or an agreement with a real client), separating verified findings from assumptions (`02-relevance-evidence.md`).
2. Review the literature on aphasia and reading, text simplification, LLM-based simplification and accessible interface design (at least 12 sources, at least 8 scientific) (`03-literature-review.md`).
3. Compare 3–5 existing systems against 6–10 criteria and identify the gap the project fills (`04-existing-systems-comparison.md`).
4. Specify functional and non-functional requirements with verifiable acceptance criteria (`06-requirements.md`).
5. Choose and justify the key technologies (at least three decisions, 2–3 alternatives each) (`05-technology-comparison.md`).
6. Design the architecture and the interface: context, use case and component diagrams; prototype of key scenarios (`07-architecture.md`, `08-ux-prototype.md`).
7. Implement the platform: extraction of the main content of a page, simplification at a selectable level with a meaning check, a clear reading view, display settings, text-to-speech, optional icons and a documented API.
8. Evaluate the result against a baseline (the original page) using readability indicators, meaning-preservation checks, comprehension and reading time, and a usability evaluation with the target audience or their representatives (`09-verification-plan.md`).

## 7. Expected outcomes

- A working web platform: the user provides a page URL or text and receives a simplified, clearly laid-out version (FR-01–FR-09).
- Three simplification levels (light, medium, strong) (FR-05).
- Input text and interface in Russian, Kazakh and English (FR-06, FR-14).
- Display settings (font size, line spacing, contrast) and history of recent pages, kept in the user's browser; no user accounts in the first version (FR-10, FR-13).
- The original text shown next to the simplified text, so the reader or a supporter can check meaning (FR-09).
- Text-to-speech with adjustable rate, and icons with captions that the user can switch on; icons are off by default, because the literature reports that some pictures can distract readers with aphasia [5] (FR-11, FR-12).
- A documented REST API used by the web client (FR-15).
- An evaluation report: change in readability indicators of the simplified text compared with the original, meaning-preservation results, comprehension and reading time, and usability results.
- Project documentation and a traceable repository (README, requirements, diagrams, tests, decision records).

## 8. Scope

### In scope

- Input: a web page URL or pasted text, in Russian, Kazakh or English.
- Extraction of the main readable content of a page.
- Text simplification at three selectable levels, with an automatic check for meaning preservation.
- A reading view with clear layout, adjustable display settings and the original text available next to the simplified text.
- Text-to-speech and optional icons with captions.
- History of recent pages and settings stored in the browser only.
- A backend API and a web client.
- Evaluation against a baseline.

### Out of scope

- User accounts and server-side user profiles (first version; may be added later if evaluation shows a need).
- Medical diagnosis, therapy or treatment advice. The platform is a reading aid, not a clinical tool.
- Browser extensions and native mobile apps.
- Pages behind a login, and pages whose content is built only by scripts after loading (the user is asked to paste the text instead).
- Automatic personalisation by aphasia type or severity.
- Clinical validation of the simplified text.

## 9. Engineering complexity

| Problem | Why it is non-trivial | Requirements |
|---|---|---|
| Main-content extraction from heterogeneous pages | Page layouts differ; menus, adverts and footers must be removed without losing content | FR-03, FR-04, NFR-06 |
| Controlled simplification with a language model | Output must follow level rules and must not change or invent facts; three languages | FR-05, FR-06, FR-07 |
| Accessible, configurable interface | Must follow accessibility guidance and support different reading abilities | FR-08–FR-14, NFR-03, NFR-04 |
| Service architecture | API, external provider behind an interface, caching, limits and error handling within a time budget | FR-15, FR-18, NFR-01, NFR-02, NFR-07 |
| Privacy and safety | User text goes to an external AI service; pages are fetched from user-supplied URLs | NFR-05, NFR-06, NFR-11 |
| Measurable evaluation | Readability formulas alone do not predict comprehension for this group [6] | `09-verification-plan.md` |

## 10. Assumptions and risks

| # | Assumption / risk | How it will be checked | Status |
|---|---|---|---|
| 1 | Users want a configurable simplification level | Conversations (DIP-3), usability evaluation | Literature reports that preferences differ [5]; conversations not yet conducted |
| 2 | Automatic readability formulas are a reasonable proxy for ease of reading for this audience | Literature review (DIP-4) and evaluation plan (DIP-10) | In doubt: one study of 75 people with aphasia found no significant effect of readability on comprehension [6]; evaluation therefore includes comprehension and reading time |
| 3 | LLM simplification can keep meaning accurate enough | Meaning check (FR-07) and evaluation (DIP-10) | Not yet tested |
| 4 | Access to representatives of the target group is possible within the project timeline | Outreach (DIP-3) | Open |
| 5 | Main-content extraction works on typical pages | Extraction test on the test page set (DIP-10) | Not yet tested |
| 6 | Existing tools do not meet the needs of this audience | Comparison of systems (DIP-5) | Refined: CommuniKey offers display adjustments, read-aloud and AI summaries but was not publicly available in the sources reviewed; the project's contribution is level-controlled rewriting with a meaning check, an API and a comprehension-based evaluation |
| 7 | Simplification quality in Kazakh is comparable to Russian and English | Separate results per language in the evaluation (DIP-10) | Not yet tested; Kazakh is a lower-resource language for language models |

## References

1. National Institute on Deafness and Other Communication Disorders (NIDCD). *Aphasia* (fact sheet). U.S. National Institutes of Health. https://www.nidcd.nih.gov/sites/default/files/Documents/health/voice/Aphasia.pdf
2. StatPearls Publishing. *Aphasia*. https://www.statpearls.com/ArticleLibrary/viewarticle/43129
3. W3C Cognitive and Learning Disabilities Accessibility Task Force. *Making Content Usable for People with Cognitive and Learning Disabilities*. W3C Working Group Note, 29 April 2021. https://www.w3.org/TR/coga-usable/
4. Rose, T. A., Worrall, L. E., Hickson, L. M., & Hoffmann, T. C. (2011). Aphasia friendly written health information: content and design characteristics. *International Journal of Speech-Language Pathology*, 13(4), 335–347. https://doi.org/10.3109/17549507.2011.560396
5. Worrall, L., Rose, T., Howe, T., Brennan, A., Egan, J., Oxenham, D., & McKenna, K. (2005). Access to written information for people with aphasia. *Aphasiology*, 19(10–11), 923–929.
6. Webster, J., Morris, J., Howard, D., & Garraffa, M. (2018). Reading for meaning: what influences paragraph understanding in aphasia? *American Journal of Speech-Language Pathology*, 27, 423–437. https://doi.org/10.1044/2017_AJSLP-16-0213
7. Queensland Aphasia Research Centre (2025). *Bridging the Digital Divide: celebration event summary*. University of Queensland. https://shrs.uq.edu.au/files/22409/DigitalDivide_ProjectSummary.pdf
