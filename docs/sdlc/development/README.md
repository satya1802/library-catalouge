# Development

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### DEV-PLAN-OVERVIEW · approved · source: plan:overview

Development plan version 1 is approved and organized into 3 sprints of 20-point capacity each, covering public search/discovery, item detail pages, staff authentication, catalogue record management, and cover image management.

Plan status: approved, version 1. Structure: 5 epics, 11 stories (56 total story points), 10 sub-tasks. Sprint goals: Sprint 1 - stand up staff authentication, protect the management area, enable manual item creation/deletion; Sprint 2 - launch public keyword search, enable item editing, deliver staff dashboard; Sprint 3 - add item detail pages, browse/filter by author/subject/availability, and cover image upload/replace/removal. Each sprint has a stated capacity of 20 points.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

- ⚠ No implementation status (done/in-progress/blocked) reported for any sprint, epic, story, or task.
- ⚠ No code, commit, pull-request, or build artifacts referenced in the input.
- ⚠ No dates given for plan approval or sprint start/end windows.

### DEV-SPRINT-1 · approved · source: plan:Sprint 1

Sprint 1 targets staff authentication, protecting the management area, and manual item record creation/deletion, with 20-point capacity.

Stated goal: 'Stand up staff authentication, protect the management area, and enable manual item record creation and deletion.' Capacity: 20 points. Based on the goal wording this scope maps to US-004 (staff login), US-005 (protect management area), US-006 (create item record), and US-008 (delete item record).

**Related:** US-004, US-005, US-006, US-008

- ⚠ Item-to-sprint assignment is inferred from the sprint goal text; the input does not explicitly list which backlog items belong to this sprint.
- ⚠ No sprint outcome, velocity, or completion status reported.

### DEV-SPRINT-2 · approved · source: plan:Sprint 2

Sprint 2 targets launching public keyword search, enabling item editing, and delivering the staff dashboard, with 20-point capacity.

Stated goal: 'Launch public keyword search, enable item editing, and deliver the staff dashboard.' Capacity: 20 points. Based on the goal wording this scope maps to US-001 (keyword search), US-007 (edit item record), and US-009 (catalogue management dashboard).

**Related:** US-001, US-007, US-009

- ⚠ Item-to-sprint assignment is inferred from the sprint goal text; the input does not explicitly list which backlog items belong to this sprint.
- ⚠ No sprint outcome, velocity, or completion status reported.

### DEV-SPRINT-3 · approved · source: plan:Sprint 3

Sprint 3 targets item detail pages, browse/filter by author/subject/availability, and cover image upload/replace/removal, with 20-point capacity.

Stated goal: 'Add item detail pages, browse/filter by author/subject/availability, and cover image upload, replace and removal.' Capacity: 20 points. Based on the goal wording this scope maps to US-003 (item detail), US-002 (browse/filter), US-010 (upload cover image), and US-011 (replace/remove cover image).

**Related:** US-003, US-002, US-010, US-011

- ⚠ Item-to-sprint assignment is inferred from the sprint goal text; the input does not explicitly list which backlog items belong to this sprint.
- ⚠ No sprint outcome, velocity, or completion status reported.

### DEV-EPIC-001 · approved · source: plan:EPIC-001

Epic grouping public catalogue search and discovery work: keyword search and browse/filter by author, subject and availability.

Epic 'Public Catalogue Search & Discovery' has no assigned role or point estimate itself; it groups stories US-001 (keyword search, 8 pts, backend-agent) and US-002 (browse/filter, 5 pts, frontend-agent).

**Related:** US-001, US-002

- ⚠ No implementation status reported for this epic or its stories.

### DEV-US-001 · approved · source: plan:US-001

Story to deliver keyword search of holdings, estimated at 8 points, led by the backend-agent role, broken into 3 implementation tasks.

Story: Keyword search of holdings (8 points, backend-agent), part of epic EPIC-001 and inferred to sit in Sprint 2. Tasks: (1) Configure Meilisearch index settings - devops-agent; (2) Implement GET /api/search endpoint - backend-agent; (3) Build public search results page - frontend-agent.

**Related:** US-001, EPIC-001, US-001-T1, US-001-T2, US-001-T3

- ⚠ No completion status, code reference, or test result provided for the story or any of its three tasks.

### DEV-US-002 · approved · source: plan:US-002

Story to let users browse and filter holdings by author, subject, and availability, estimated at 5 points, led by the frontend-agent role.

Story: Browse and filter holdings by author, subject and availability (5 points, frontend-agent), part of epic EPIC-001, inferred to sit in Sprint 3. No sub-tasks were broken out in the plan for this story.

**Related:** US-002, EPIC-001

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-EPIC-002 · approved · source: plan:EPIC-002

Epic for item detail pages, grouping the single story US-003.

Epic 'Item Detail Pages' has no assigned role or point estimate itself; it groups story US-003 (view full item detail, 3 pts, frontend-agent).

**Related:** US-003

- ⚠ No implementation status reported for this epic or its story.

### DEV-US-003 · approved · source: plan:US-003

Story to display a full item detail view, estimated at 3 points, led by the frontend-agent role.

Story: View full item detail (3 points, frontend-agent), part of epic EPIC-002, inferred to sit in Sprint 3. No sub-tasks were broken out in the plan for this story.

**Related:** US-003, EPIC-002

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-EPIC-003 · approved · source: plan:EPIC-003

Epic for staff authentication and access control, grouping login and management-area protection stories.

Epic 'Staff Authentication & Access' has no assigned role or point estimate itself; it groups US-004 (staff login, 5 pts, backend-agent) and US-005 (protect management area, 3 pts, security-agent).

**Related:** US-004, US-005

- ⚠ No implementation status reported for this epic or its stories.

### DEV-US-004 · approved · source: plan:US-004

Story to let staff log in to the management area, estimated at 5 points, led by the backend-agent role.

Story: Staff login to the management area (5 points, backend-agent), part of epic EPIC-003, inferred to sit in Sprint 1. No sub-tasks were broken out in the plan for this story.

**Related:** US-004, EPIC-003

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-US-005 · approved · source: plan:US-005

Story to protect the management area from public access, estimated at 3 points, led by the security-agent role.

Story: Protect the management area from public access (3 points, security-agent), part of epic EPIC-003, inferred to sit in Sprint 1. No sub-tasks were broken out in the plan for this story.

**Related:** US-005, EPIC-003

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-EPIC-004 · approved · source: plan:EPIC-004

Epic for catalogue record management, grouping create, edit, delete, and dashboard stories.

Epic 'Catalogue Record Management' has no assigned role or point estimate itself; it groups US-006 (create item, 8 pts, backend-agent), US-007 (edit item, 5 pts, backend-agent), US-008 (delete item, 3 pts, backend-agent), and US-009 (management dashboard, 5 pts, frontend-agent).

**Related:** US-006, US-007, US-008, US-009

- ⚠ No implementation status reported for this epic or its stories.

### DEV-US-006 · approved · source: plan:US-006

Story to create an item record, estimated at 8 points, led by the backend-agent role, broken into 4 implementation tasks.

Story: Create an item record (8 points, backend-agent), part of epic EPIC-004, inferred to sit in Sprint 1. Tasks: (1) Define item data model and migration - backend-agent; (2) Implement POST /api/items endpoint with validation - backend-agent; (3) Sync newly created items to the Meilisearch index - backend-agent; (4) Build staff create-item form UI - frontend-agent.

**Related:** US-006, EPIC-004, US-006-T1, US-006-T2, US-006-T3, US-006-T4

- ⚠ No completion status, code reference, or test result provided for the story or any of its four tasks.

### DEV-US-007 · approved · source: plan:US-007

Story to edit an existing item record, estimated at 5 points, led by the backend-agent role.

Story: Edit an item record (5 points, backend-agent), part of epic EPIC-004, inferred to sit in Sprint 2. No sub-tasks were broken out in the plan for this story.

**Related:** US-007, EPIC-004

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-US-008 · approved · source: plan:US-008

Story to delete an item record, estimated at 3 points, led by the backend-agent role.

Story: Delete an item record (3 points, backend-agent), part of epic EPIC-004, inferred to sit in Sprint 1. No sub-tasks were broken out in the plan for this story.

**Related:** US-008, EPIC-004

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-US-009 · approved · source: plan:US-009

Story to deliver a catalogue management dashboard, estimated at 5 points, led by the frontend-agent role.

Story: Catalogue management dashboard (5 points, frontend-agent), part of epic EPIC-004, inferred to sit in Sprint 2. No sub-tasks were broken out in the plan for this story.

**Related:** US-009, EPIC-004

- ⚠ No completion status, code reference, or test result provided for this story.

### DEV-EPIC-005 · approved · source: plan:EPIC-005

Epic for cover image management, grouping upload and replace/remove stories.

Epic 'Cover Image Management' has no assigned role or point estimate itself; it groups US-010 (upload cover image, 8 pts, backend-agent) and US-011 (replace or remove cover image, 3 pts, backend-agent).

**Related:** US-010, US-011

- ⚠ No implementation status reported for this epic or its stories.

### DEV-US-010 · approved · source: plan:US-010

Story to upload a cover image for an item, estimated at 8 points, led by the backend-agent role, broken into 3 implementation tasks.

Story: Upload a cover image (8 points, backend-agent), part of epic EPIC-005, inferred to sit in Sprint 3. Tasks: (1) Configure S3 storage backend for cover uploads - devops-agent; (2) Implement cover upload endpoint with validation - backend-agent; (3) Build cover upload control in item form UI - frontend-agent.

**Related:** US-010, EPIC-005, US-010-T1, US-010-T2, US-010-T3

- ⚠ No completion status, code reference, or test result provided for the story or any of its three tasks.

### DEV-US-011 · approved · source: plan:US-011

Story to replace or remove an item's cover image, estimated at 3 points, led by the backend-agent role.

Story: Replace or remove a cover image (3 points, backend-agent), part of epic EPIC-005, inferred to sit in Sprint 3. No sub-tasks were broken out in the plan for this story.

**Related:** US-011, EPIC-005

- ⚠ No completion status, code reference, or test result provided for this story.
<!-- sdlc:auto:end narrative -->
