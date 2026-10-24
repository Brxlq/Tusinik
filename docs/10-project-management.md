# 10 — Project Management

**YouTrack task:** DIP-12

## 1. Team and responsibilities

| Member | GitHub | Role | Area of responsibility |
|---|---|---|---|
| Sovet Yerkebulan Talgatuly | [Brxlq](https://github.com/Brxlq) | Backend Developer | Backend API, extraction and simplification pipeline, meaning check, storage; relevance evidence, comparison of systems, technology decisions, requirements, supervisor feedback |
| Sadenov Ernar Aidynovich | [AIeeph](https://github.com/AIeeph) | Frontend Developer | Web client, reading view, settings, text-to-speech and icons; proposal, literature review, diagrams, prototype, verification plan, repository check and submission tag |

Each member reviews and merges the other member's pull requests.

## 2. Tasks for the first pre-defense

| Task | Title | Assignee | Priority | Due | Deliverable |
|---|---|---|---|---|---|
| DIP-1 | Repository structure and README | Yerkebulan Sovet | Critical | 20 Oct 2026 | `README.md`, folder structure, PR template |
| DIP-2 | Project proposal and scope | Ernar Sadenov | Major | 21 Oct 2026 | `docs/01-proposal.md`, `README.md` |
| DIP-3 | Evidence of relevance | Yerkebulan Sovet | Critical | 23 Oct 2026 | `docs/02-relevance-evidence.md` |
| DIP-4 | Literature review | Ernar Sadenov | Major | 22 Oct 2026 | `docs/03-literature-review.md` |
| DIP-5 | Comparison of existing systems | Yerkebulan Sovet | Major | 22 Oct 2026 | `docs/04-existing-systems-comparison.md` |
| DIP-6 | Technology comparison | Yerkebulan Sovet | Normal | 23 Oct 2026 | `docs/05-technology-comparison.md`, `docs/decisions/` |
| DIP-7 | Requirements and acceptance criteria | Yerkebulan Sovet | Major | 23 Oct 2026 | `docs/06-requirements.md` |
| DIP-8 | Diagrams | Ernar Sadenov | Major | 23 Oct 2026 | `docs/07-architecture.md`, `diagrams/` |
| DIP-9 | Prototype of key scenarios | Ernar Sadenov | Normal | 23 Oct 2026 | `prototype/`, `docs/08-ux-prototype.md` |
| DIP-10 | Verification and evaluation plan | Ernar Sadenov | Normal | 23 Oct 2026 | `docs/09-verification-plan.md` |
| DIP-11 | Supervisor feedback and how it was addressed | Yerkebulan Sovet | Normal | 23 Oct 2026 | `docs/11-supervisor-feedback.md` |
| DIP-12 | Repository check against the list and tag | Ernar Sadenov | Critical | 24 Oct 2026 | `docs/10-project-management.md`, tag `prob-period-1` |

Expected outcome and acceptance criteria of each task are written in the task description in YouTrack.

## 3. Milestones

| Milestone | Planned date | Content |
|---|---|---|
| M1 First pre-defense (`prob-period-1`) | 24 Oct 2026 | Analysis and design materials (this submission) |
| M2 Walking skeleton | 2 weeks after M1 | Backend `/api/v1/health` and `/simplify` with pasted text; React input and reading view; CI |
| M3 URL extraction and provider selection | 4 weeks after M1 | FR-01–FR-04; comparison P1; E1 |
| M4 Full first version | 8 weeks after M1 | All Must and Should requirements; E2, E4 |
| M5 User evaluation | Before the final pre-defense | E3 with users or representatives; report |

Dates after M1 are relative because the university schedule for the next pre-defense is not yet known to the team; they will be fixed in YouTrack when it is published.

## 4. Risks

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|
| No access to people with aphasia or therapists | High | High | Include representatives; state limitation honestly | Yerkebulan Sovet |
| LLM changes meaning | Medium | High | FR-07, FR-09, E2 | Yerkebulan Sovet |
| Lower quality in Kazakh | Medium | Medium | Kazakh in P1; results per language | Yerkebulan Sovet |
| Backend stack new to the team | Medium | Medium | Walking skeleton first; small PRs with tests | Yerkebulan Sovet |
| Accessibility defects in the client | Medium | High | axe-core in CI; keyboard and screen-reader checks | Ernar Sadenov |
| No Kazakh voice for text-to-speech in some browsers | High | Low | Clear message (FR-11); server TTS later | Ernar Sadenov |
| Workload peaks before pre-defenses | High | Medium | Weekly plan in YouTrack; review PRs within one day | Both |

## 5. Dependencies

- DIP-7 (requirements) → DIP-6, DIP-8, DIP-9, DIP-10 refer to requirement IDs.
- Provider selection P1 → final pipeline design (ADR-0003).
- Answer to supervisor question Q1 → conversations (DIP-3) and user study E3.

## 6. Working rules

- One YouTrack task per work item; branch `DIP-<id>-short-description`; commit message `DIP-<id>: description`.
- Every change goes through a pull request that names the task and says how the work was checked; the other member reviews and merges.
- Tags for submissions (`prob-period-1` and later ones) are never moved or replaced.
- No secrets or personal data in the repository; `config/.env.example` holds placeholder names only.
