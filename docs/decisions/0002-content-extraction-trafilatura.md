# ADR-0002: Main-content extraction — Trafilatura

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

Users submit arbitrary page URLs; menus, adverts and footers must be removed while headings, paragraphs and lists are kept (FR-03).

## Options considered

Trafilatura; Mozilla Readability; custom BeautifulSoup rules. Full comparison: `../05-technology-comparison.md`.

## Decision

Use Trafilatura as the primary extractor and BeautifulSoup for post-processing.

## Consequences

- No second runtime is needed.
- Extraction quality is measured in test E1; Readability is the fallback candidate if E1 fails for a page type.
