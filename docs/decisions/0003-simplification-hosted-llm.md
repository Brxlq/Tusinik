# ADR-0003: Simplification — hosted LLM behind a provider interface

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

Text must be rewritten at three levels in Russian, Kazakh and English without losing meaning (FR-05–FR-07).

## Options considered

Rule-based lexical substitution; hosted LLM API; self-hosted open-weight model. Full comparison: `../05-technology-comparison.md`.

## Decision

Use a hosted LLM API behind a provider-independent interface, plus a rule-based safety layer (sentence-length limits, preservation of numbers, dates and names) and an automatic meaning check.

## Consequences

- Pasted text is sent to an external service: users are informed and the text is not stored (NFR-05).
- The provider is selected by comparison P1 in `../09-verification-plan.md`.
- Kazakh quality is reported separately.
