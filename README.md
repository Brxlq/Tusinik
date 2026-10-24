# Tusinik
Diploma Project

# Aphasia-Friendly Web Page Simplifier Platform

This diploma project explores a web platform that accepts a web page URL or pasted text and produces a simplified version suitable for people with aphasia. The platform is intended to offer a configurable reading level while preserving meaning through a clear layout, with optional text-to-speech and icons with captions.

## 2. Problem statement

Aphasia is an acquired language disorder, usually after stroke, that can affect reading and comprehension. Long, term-heavy web pages can be difficult to read, while standard accessibility guidance mostly targets visual, motor, and hearing impairments.

## 3. Target users

- People with aphasia
- Their relatives
- Speech-language therapists

## 4. Expected outcomes

- A platform concept for simplifying web page content from a URL or pasted text.
- Simplified content with a configurable reading level, preserved meaning, and a clear layout.
- Optional text-to-speech and icons with captions.

## 5. Team members and responsibilities

| Name | GitHub | Role | Responsibility |
| --- | --- | --- | --- |
| Yerkebulan Sovet | [Brxlq](https://github.com/Brxlq) |
| Ernar Sadenov | [AIeeph](https://github.com/AIeeph) |

## 6. Supervisor

Mirzagalikova Botagoz Vitalyevna

## 7. Scope



### In scope

- Accepting a web page URL or pasted text.
- Producing a simplified version suitable for people with aphasia.
- A configurable reading level, meaning preservation, and a clear layout.
- Optional text-to-speech and icons with captions.

### Out of scope

- Final technology selections, including the frontend technology.
- Functionality beyond this draft scope.

## 8. Current status

Analysis and planning stage;

## 9. Selected technologies

- Python
- FastAPI
- PostgreSQL
- A language-model API
- BeautifulSoup
- textstat
- Docker
- pytest
- GitHub Actions
- Frontend technology:

## 10. Links

- [YouTrack project (DIP)](https://tusinik.youtrack.cloud/projects/DIP)
- [Project documentation](docs/)
- Prototype: 

## 11. Repository structure

```text
src/        Application code; backend and frontend placeholders
docs/       Planning and project documentation
diagrams/   Editable diagram sources and committed exports
tests/      Automated-test placeholder
config/     Configuration examples
.github/    Pull request workflow files
```

## 12. Workflow

Each work item has one YouTrack task. Branches use the name `DIP-<id>-short-description`, and commits use the message format `DIP-<id>: description`. Changes go through pull requests that reference the task and describe how the work was checked. Another team member reviews before merge, and `main` is the reviewed baseline.

## 13. Setup, run and test instructions

Will be added when runnable code is available.

## 14. Submission tags

Pre-defense tags are created for each submission and are never moved or replaced.
