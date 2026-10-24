# ADR-0001: Backend platform — FastAPI

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

The backend must extract page content, call a language-model API, compute readability indicators and expose a documented API (D1–D5 in `../05-technology-comparison.md`).

## Options considered

FastAPI (Python); Django + Django REST Framework (Python); Node.js + Express. Full comparison: `../05-technology-comparison.md`.

## Decision

Use FastAPI.

## Consequences

- Python text-processing libraries (Trafilatura, BeautifulSoup, textstat) are available in-process.
- OpenAPI documentation is generated automatically (FR-15).
- The team must learn FastAPI; this is recorded as a risk.
