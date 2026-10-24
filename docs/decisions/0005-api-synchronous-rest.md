# ADR-0005: Client–backend interaction — synchronous REST

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

One request may fetch a page, extract text and simplify several blocks; the target is 15 s for up to 1,500 words (NFR-01).

## Options considered

Synchronous REST; REST with background jobs; streaming (SSE/WebSocket). Full comparison: `../05-technology-comparison.md`.

## Decision

Use synchronous REST with a text-length limit and a client-side progress indicator.

## Consequences

- Simple to document and test.
- If test E4 shows NFR-01 is not met, long requests move to background jobs with a status endpoint.
