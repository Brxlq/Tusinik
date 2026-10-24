# Evidence of Relevance

> YouTrack task: DIP-3.

## 1. Purpose

To show, with evidence, that the problem is real, who has it, and what is still an assumption. Verified findings and assumptions are kept separate on purpose.

## 2. Part A: evidence from the literature

| # | Finding | Source | Strength of evidence |
|---|---|---|---|
| A1 | Aphasia impairs the expression and understanding of language, including reading and writing; stroke is its most common cause. | L1, L2 | Fact sheet and reference work |
| A2 | Content and design characteristics of written text act as barriers or facilitators to reading for adults with aphasia (40 adults interviewed, varied severity and reading ability). | L8 | Peer-reviewed, qualitative, printed health information |
| A3 | Aphasia-friendly formatting appears to improve comprehension of written material, but not everyone prefers it. | L4, L5 | Peer-reviewed, printed materials |
| A4 | Decorative pictures (e.g. ClipArt, internet images) may distract rather than help. | L5 | Peer-reviewed synthesis |
| A5 | Most participants in one study preferred combined written and auditory presentation. | L10 | Peer-reviewed, single study |
| A6 | Text length affected reading time but not comprehension accuracy; readability and propositional density did not significantly affect comprehension (75 people with aphasia vs 87 controls). | L9 | Peer-reviewed, paragraph-level texts |
| A7 | Word-level properties (frequency, class, length) influence how people with aphasia process sentences. | L11 | Peer-reviewed, laboratory task |
| A8 | Accessibility guidance has traditionally focused on interfaces for sensory and physical impairments; guidance for cognitive and learning disabilities is supplemental and not required for WCAG conformance. | L3 | W3C Working Group Note |
| A9 | In focus groups with people with aphasia, family members and health professionals, online written information was described as overwhelming; text-to-speech helped only when its rate could be changed; customisable software was important. | L17 | Project summary from an external research group (not our interviews) |
| A10 | In co-design workshops, priorities for a web tool were reducing distractions, help with reading and understanding information, help with word finding, and customisable settings. A resulting extension was reported usable and acceptable in a field trial with 16 people with aphasia. | L17, L18 | External project summary and conference abstract |

**What Part A does not show:** the older studies (A2–A7) concern printed materials and short texts; A9 and A10 come from one external research group and are not our own interviews. Part A does not show that users want an adjustable *simplification level* specifically. These points are covered in Part B.

## 3. Part B: user conversations

### 3.1 Target: 1 conversation

| Group | Why | Status |
|---|---|---|
| Speech-language therapists / aphasia specialists | Know how patients read and what helps in practice | 1 conducted (B1, written) |


### 3.2 Ethics and consent

- Participation is voluntary and anonymous; no names are stored unless the person agrees.
- Do not ask about medical history. Ask about experience of reading information online.
- For people with aphasia, keep questions short, allow a supporter to help, and use choice questions (e.g. yes/no, pointing at options).

### 3.3 Outreach message (Russian, ready to send)

> Здравствуйте! Мы студенты AITU, делаем дипломный проект — сайт, который упрощает текст веб-страниц для людей с афазией. Можно задать 3 коротких вопроса?

### 3.4 Interview guide (3 questions)

1. What difficulties do your patients encounter when reading texts on the internet?
2. What helps them understand the text better?
3. Are different degrees of text simplification necessary?


(Russian version: 1. С какими трудностями ваши пациенты сталкиваются при чтении текстов в интернете? 2. Что помогает им лучше понимать текст? 3. Нужны ли разные степени упрощения текста? )

### 3.5 Interview log (only real conversations are recorded)

One conversation had taken place by 23 October 2026. It was a short written consultation (three questions from section 3.4, adapted) in a messenger; the participant agreed to answer and was told the answers are for a diploma project. No name is recorded.

| # | Date | Group (no names unless agreed) | Format | Key answers | Requirement or design implication | Confirms / contradicts which assumption |
|---|---|---|---|---|---|---|
| B1 | 2026-10-23 | Speech-language therapist (logoped) | Written, messenger, 3 questions | (1) They often find it difficult to understand long sentences, complex words, and large amounts of text. Sometimes a person reads the words but cannot grasp the overall meaning. (2) Short sentences, simple words, large print, and good contrast. For some, having the text read aloud and including images is also helpful. (3) Yes, because aphasia manifests differently in every person. It is best to provide several levels of simplification so that everyone can choose the one that suits them. | Supports FR-05 (three levels, sentence-length limits, key points first at level 3), FR-08 and FR-10 (large font, contrast), FR-11 (text-to-speech as an option), FR-12 (pictures optional, not default) | Confirms rows 2, 7 and 9; partly confirms row 5 |
| 2 | | | | | | |
| 3 | | | | | | |

**Limitations of B1:** one professional, written answers to three short questions, no follow-up questions; it is the view of a specialist, not of people with aphasia themselves.

## 4. Verified findings vs assumptions

| # | Statement | Status | Basis |
|---|---|---|---|
| 1 | Aphasia affects reading and understanding of written language | Supported by literature | A1 |
| 2 | Content and layout of text affect how well people with aphasia can read it | Supported by literature and confirmed by 1 conversation | A2, A3, B1 |
| 3 | Formatting adjustments help some readers but are not preferred by all | Supported by literature | A3 |
| 4 | Decorative images can distract | Supported by literature (printed material) | A4 |
| 5 | Combined text and audio is preferred by many | Supported by one study; 1 conversation: helps some people | A5, B1 |
| 6 | Readability scores alone do not predict comprehension for this group | Supported by one study | A6 |
| 7 | Short or frequent words help | Supported by laboratory evidence and confirmed by 1 conversation | A7, B1 |
| 8 | People with aphasia struggle with ordinary web pages | Supported by an external project (focus groups, not our interviews); local confirmation pending | A9, A10, Part B |
| 9 | Users want customisable settings and adjustable help | Supported by an external project for customisation in general; an adjustable simplification level specifically is **confirmed by 1 conversation** (B1) and needs confirmation by further conversations | A3, A9, A10, B1 |
| 10 | Users and therapists would use a URL-based simplifier | **Assumption** | Part B |
| 11 | Which websites and content types matter most | **Unknown** | Part B |
| 12 | Existing tools do not meet these needs | **Working hypothesis**, tested in DIP-5 | DIP-5 |

After each conversation this table is updated: a row changes to "Confirmed by N conversations" or "Contradicted", with what was learned.

## 5. Consequences for the project

- Requirements that rest on literature only are marked with their source in `06-requirements.md`; the adjustable level (row 9) now has first direct support from B1; rows 10–11 (URL-based use, content types) remain assumptions and are flagged for confirmation.
- B1 noted that a reader may read every word but miss the overall meaning; this supports putting the key points first at level 3 (FR-05) and measuring main-idea comprehension separately in E3 (`09-verification-plan.md`).
- The usability evaluation in `09-verification-plan.md` doubles as a check of these assumptions if conversations are not completed before implementation.
- Next step: 2–4 more conversations (at least one with a relative of a person with aphasia), using the outreach message (3.3).
