# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Stand up staff authentication, protect the management area, and enable manual item record creation and deletion.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-004 | Story | Staff login to the management area | 5 | backend-agent | — |
| US-005 | Story | Protect the management area from public access | 3 | security-agent | US-004 |
| US-006 | Story | Create an item record | 8 | backend-agent | US-004 |
| ↳ US-006-T1 | Task | Define item data model and migration | — | backend-agent | — |
| ↳ US-006-T2 | Task | Implement POST /api/items endpoint with validation | — | backend-agent | — |
| ↳ US-006-T3 | Task | Sync newly created items to the Meilisearch index | — | backend-agent | — |
| ↳ US-006-T4 | Task | Build staff create-item form UI | — | frontend-agent | — |
| US-008 | Story | Delete an item record | 3 | backend-agent | US-006 |

## Sprint 2: Sprint 2

Launch public keyword search, enable item editing, and deliver the staff dashboard.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Keyword search of holdings | 8 | backend-agent | US-006 |
| ↳ US-001-T1 | Task | Configure Meilisearch index settings | — | devops-agent | — |
| ↳ US-001-T2 | Task | Implement GET /api/search endpoint | — | backend-agent | — |
| ↳ US-001-T3 | Task | Build public search results page | — | frontend-agent | — |
| US-007 | Story | Edit an item record | 5 | backend-agent | US-006 |
| US-009 | Story | Catalogue management dashboard | 5 | frontend-agent | US-006, US-005 |

## Sprint 3: Sprint 3

Add item detail pages, browse/filter by author/subject/availability, and cover image upload, replace and removal.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-002 | Story | Browse and filter holdings by author, subject and availability | 5 | frontend-agent | US-001 |
| US-003 | Story | View full item detail | 3 | frontend-agent | US-006, US-001 |
| US-010 | Story | Upload a cover image | 8 | backend-agent | US-006 |
| ↳ US-010-T1 | Task | Configure S3 storage backend for cover uploads | — | devops-agent | — |
| ↳ US-010-T2 | Task | Implement cover upload endpoint with validation | — | backend-agent | — |
| ↳ US-010-T3 | Task | Build cover upload control in item form UI | — | frontend-agent | — |
| US-011 | Story | Replace or remove a cover image | 3 | backend-agent | US-010 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
