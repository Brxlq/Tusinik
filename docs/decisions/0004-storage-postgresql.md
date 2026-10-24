# ADR-0004: Storage — PostgreSQL, no user accounts

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

The first version has no accounts. The server stores only URL-result cache entries with expiry, anonymous ratings and evaluation records; settings and history stay in the browser.

## Options considered

PostgreSQL; MongoDB; SQLite. Full comparison: `../05-technology-comparison.md`.

## Decision

Use PostgreSQL in deployment and SQLite in automated tests.

## Consequences

- Evaluation reports use SQL aggregation by level, language and provider.
- Cache expiry is implemented as a scheduled delete by timestamp (24 hours, FR-18).
