# ADR-0006: Frontend — React with TypeScript

- **Status:** Accepted
- **Date:** 2026-10-24
- **YouTrack task:** DIP-6

## Context

The client needs interactive reading blocks, display settings, read-aloud controls, browser-stored history and an interface in three languages, and must use the public API (FR-08–FR-15).

## Options considered

React; Vue; server-rendered Jinja2 templates. Full comparison: `../05-technology-comparison.md`.

## Decision

Use React with TypeScript, built with Vite.

## Consequences

- Accessibility is checked with eslint-plugin-jsx-a11y and axe-core (NFR-03).
- The frontend developer owns this decision; Vue remains an equivalent fallback.
