# Traceability Matrix

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start legend -->
Built from recorded links, not written by a model: requirement key → plan item (`source_stable_key`) → development ticket (`stable_key`) → the ticket's test steps → the last live deployment that started after the work finished. **AC tests** counts the story's acceptance criteria that have a test case, and those whose test cases all passed in the latest finished E2E run.
<!-- sdlc:auto:end legend -->

<!-- sdlc:auto:start matrix -->
| Requirement | Epic | Title | Plan items | Sprint | Design | Code | Commits | Ticket | Tests | AC tests | Deployed |
|---|---|---|---|---|---|---|---|---|---|---|---|
| US-001 | EPIC-001 | Keyword search of holdings | US-001, US-001-T1, US-001-T2, US-001-T3 | 2 | GET /api/search, backend, frontend, item, search | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-002 | EPIC-001 | Browse and filter holdings by author, subject and availability | US-002 | 3 | GET /api/facets, GET /api/search, backend, frontend, item, search | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-003 | EPIC-002 | View full item detail | US-003 | 3 | GET /api/items/{id}, backend, frontend, item | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-004 | EPIC-003 | Staff login to the management area | US-004 | 1 | POST /api/auth/login, backend, database, frontend, staff_user | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-005 | EPIC-003 | Protect the management area from public access | US-005 | 1 | POST /api/auth/logout, backend, frontend, staff_user | — | — | not started | not run | 0/2 tested · 0/2 passing | — |
| US-006 | EPIC-004 | Create an item record | US-006, US-006-T1, US-006-T2, US-006-T3, US-006-T4 | 1 | POST /api/items, backend, database, frontend, item, search | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-007 | EPIC-004 | Edit an item record | US-007 | 2 | PUT /api/items/{id}, backend, frontend, item, search | — | — | not started | not run | 0/2 tested · 0/2 passing | — |
| US-008 | EPIC-004 | Delete an item record | US-008 | 1 | DELETE /api/items/{id}, backend, frontend, item, search | — | — | not started | not run | 0/2 tested · 0/2 passing | — |
| US-009 | EPIC-004 | Catalogue management dashboard | US-009 | 2 | GET /api/items, backend, frontend, item | — | — | not started | not run | 0/2 tested · 0/2 passing | — |
| US-010 | EPIC-005 | Upload a cover image | US-010, US-010-T1, US-010-T2, US-010-T3 | 3 | POST /api/items/{id}/cover, backend, frontend, item, object_storage | — | — | not started | not run | 0/3 tested · 0/3 passing | — |
| US-011 | EPIC-005 | Replace or remove a cover image | US-011 | 3 | DELETE /api/items/{id}/cover, POST /api/items/{id}/cover, backend, frontend, item, object_storage | — | — | not started | not run | 0/2 tested · 0/2 passing | — |
<!-- sdlc:auto:end matrix -->
