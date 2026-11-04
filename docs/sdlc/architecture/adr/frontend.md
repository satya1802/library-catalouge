<!-- sdlc:auto:start adr -->
# ADR-0001: Use Vue 3 for Frontend

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- Vue 3
- React
- Svelte

## Decision Outcome

Chosen option: Vue 3, because Delivers the responsive public search/browse/detail pages and the staff management dashboard as a single SPA consuming the Django REST API (NFR-006).

## Consequences

A SPA needs a separate build/deploy step and client-side routing, but gives a smooth filter/search UX for the catalogue.
<!-- sdlc:auto:end adr -->