# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### EPIC-001 · draft · source: requirements:EPIC-001

Public, login-free keyword search and filtering across up to ~10,000 catalogue items, powered by Meilisearch.

- US-001 [critical] Keyword search of holdings: anonymous visitors search by keyword with no login; results ranked by relevance via Meilisearch; each result shows title/author/availability (AC-001, AC-003); empty-results message shown with search box still available when nothing matches (AC-002).
- US-002 [high] Browse/filter by author, subject, availability: filters update results via Meilisearch filtering (AC-004); filters combine with keyword search (AC-005); clearing a filter removes its restriction (AC-006).

**Related:** US-001, US-002, NFR-004, NFR-005

### EPIC-002 · draft · source: requirements:EPIC-002

Public, login-free item detail page showing full bibliographic data, cover image, shelf/location and read-only availability.

- US-003 [high] View full item detail: selecting an item from search/browse opens detail page with title, author, ISBN, publisher, year, subject/category, description, cover, shelf/location, availability (AC-007); placeholder image shown if no cover uploaded, other fields still shown normally (AC-008); availability status shown read-only (AC-009).

**Related:** US-003

### EPIC-003 · draft · source: requirements:EPIC-003

Staff email/password authentication gates the management area; single staff/librarian role, no public login.

- US-004 [critical] Staff login: valid credentials authenticate and open the dashboard (AC-010); invalid credentials denied with generic error not revealing which field was wrong (AC-011); single staff role has full create/edit/delete/upload rights, no admin/editor tiers at launch (AC-012).
- US-005 [high] Protect management area: unauthenticated access to any management page/action redirects to login and the action does not proceed (AC-013); logout ends session, requiring re-login (AC-014).
Assumption stated: email/password auth is sufficient; no separate admin vs editor tiers at launch.

**Related:** US-004, US-005, NFR-001, NFR-002, NFR-007

### EPIC-004 · draft · source: requirements:EPIC-004

Staff manually create, edit, delete catalogue records and manage them via a dashboard; manual entry is the only data-entry path (no bulk import/ISBN lookup/ILS sync).

- US-006 [critical] Create item record: full bibliographic form including shelf/location and availability saves a record and makes it searchable via Meilisearch (AC-015); missing required field blocks save and is flagged (AC-016); no bulk import/ISBN lookup/ILS sync offered (AC-017).
- US-007 [high] Edit item record: changes update record and propagate to public detail page and search index (AC-018); availability change is reflected publicly and as a filter value (AC-019).
- US-008 [medium] Delete item record: deletion requires confirmation (AC-021) and removes record from catalogue and public search/browse (AC-020).
- US-009 [high] Management dashboard: lists records with create/open/edit/delete controls (AC-022); supports search/filter within dashboard to find a record (AC-023).

**Related:** US-006, US-007, US-008, US-009, NFR-004

### EPIC-005 · draft · source: requirements:EPIC-005

Staff upload, replace and remove cover images for item records, stored in Amazon S3 and shown publicly or as a placeholder.

- US-010 [medium] Upload cover: valid image uploaded during create/edit is stored in S3 and linked to the record (AC-024); shown in public results/detail (AC-025); unsupported file types are rejected with a message, no invalid image attached (AC-026).
- US-011 [low] Replace/remove cover: new upload replaces stored image and prior one (AC-027); removing a cover falls back to placeholder in public catalogue (AC-028).

**Related:** US-010, US-011, NFR-003

### NFR-001 · draft · source: requirements:NFR-001

Security: management area restricted to authenticated staff only.

All management pages/actions reachable only by authenticated staff; anonymous attempts redirect to login without performing the action; logout ends session requiring re-login.

**Related:** US-005

### NFR-002 · draft · source: requirements:NFR-002

Security: failed login attempts must not reveal which credential field was wrong.

Login failures return a generic error message regardless of whether the email or password was incorrect.

**Related:** US-004

### NFR-003 · draft · source: requirements:NFR-003

Security: only valid/supported image files may be stored as covers.

Cover uploads are validated; unsupported/invalid files are rejected with a message and not attached to the record.

**Related:** US-010

### NFR-004 · draft · source: requirements:NFR-004

Scalability: catalogue must reliably support up to ~10,000 items under modest traffic.

Search and browse must return results reliably at target scale of ~10,000 item records.

**Related:** US-001, US-002

### NFR-005 · draft · source: requirements:NFR-005

Usability: keyword search must tolerate typos/misspellings.

A slightly mistyped title, author or subject must still find matching items, with results ranked by relevance.

**Related:** US-001

### NFR-006 · draft · source: requirements:NFR-006

Compatibility: responsive web app for desktop and mobile browsers, no native app.

Applies to the whole system; delivered as a single responsive web application, no native mobile app planned.

### NFR-007 · draft · source: requirements:NFR-007

Accepted risk: single staff role at launch, no admin/editor tiers.

One staff/librarian role has full create/edit/delete/cover-management rights; finer-grained access control explicitly deferred as accepted risk.

**Related:** US-004

### NFR-008 · draft · source: requirements:NFR-008

Accepted risk: no offline use supported.

Applies to the whole system; the catalogue requires network connectivity and offline mode is explicitly out of scope.
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-11-04

**Capabilities in scope:** Public keyword search of library holdings, Item detail pages (bibliographic fields, cover, shelf/location, availability), Browse and filter holdings by author, subject and availability, Staff login to the management area, Manual item record creation, editing and deletion, Cover image upload for item records, Staff catalogue management dashboard, Meilisearch for catalogue keyword search and filtering, Amazon S3 storage for uploaded cover images, Public browses without logging in; only staff authenticate

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | Django | — |
| cloud | AWS | A single library with modest traffic needs cheap, reliable hosting that can sit mostly idle and still be dependable. AWS covers the server, the PostgreSQL database and the cover-image storage in one place, and at this scale the running cost stays low. It is the only option offered and fits the requirement without over-building. |
| database | PostgreSQL | A library catalogue is structured records — title, author, ISBN, subject, availability, shelf location — which is exactly what a relational database stores and filters cleanly. At ~10,000 items with modest traffic, PostgreSQL handles the keyword search and author/subject/availability filters comfortably and keeps the data reliable for staff edits. |
| frontend | Vue | — |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Public Catalogue Search & Discovery

Lets any member of the public search and browse the library's holdings without logging in. Keyword search and filtering are powered by Meilisearch across the small catalogue (up to ~10,000 items). Covers keyword search and filtering by author, subject/category and availability.

### US-001: Keyword search of holdings

**Priority:** critical

> As a member of the public, I want to search the library catalogue by keyword without logging in, so that I can quickly find items that match words in their title, author, subject or description.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | I am an anonymous visitor on the public catalogue home page | I enter a keyword and submit the search | the system returns a list of matching item records ranked by relevance, retrieved via Meilisearch, with no login required |
| AC-002 | I have entered a keyword that matches no items | the search completes | the system shows an empty-results message indicating nothing matched and keeps the search box available to refine the query |
| AC-003 | I submit a search against a catalogue of up to 10,000 items | results are returned | results are displayed with each item's title, author and availability status |

### US-002: Browse and filter holdings by author, subject and availability

**Priority:** high

> As a member of the public, I want to filter and browse catalogue results by author, subject/category and availability, so that I can narrow down to the kinds of items I am interested in.

| ID | Given | When | Then |
|---|---|---|---|
| AC-004 | I am viewing catalogue results or the browse view | I apply an author, subject/category or availability filter | the result list updates to show only items matching the selected filter(s) using Meilisearch filtering |
| AC-005 | I have one or more filters applied | I combine a keyword search with the filters | results reflect both the keyword and all active filters together |
| AC-006 | I have filters applied | I clear a filter | the result list updates to remove that filter's restriction |

## EPIC-002: Item Detail Pages

A public, login-free detail page for each catalogue item showing its full bibliographic fields, cover image, shelf/location and staff-set availability status.

### US-003: View full item detail

**Priority:** high

> As a member of the public, I want to open an item's detail page from search or browse results, so that I can see all the information the library holds about that item.

| ID | Given | When | Then |
|---|---|---|---|
| AC-007 | I am viewing a list of search or browse results as an anonymous visitor | I select an item | its detail page opens showing title, author, ISBN, publisher, year, subject/category, description, cover image, shelf/location and availability status |
| AC-008 | an item has no cover image uploaded | I view its detail page | a placeholder image is shown in place of a cover and the remaining fields are displayed normally |
| AC-009 | a staff member has set an item's availability status | I view the item detail page | the current availability status is displayed as read-only information |

## EPIC-003: Staff Authentication & Access

Library staff authenticate with email/password accounts to reach the management area. A single staff/librarian role grants full access to catalogue management; the public never logs in. Assumption: email/password auth is sufficient and there are no separate admin vs. editor tiers at launch.

### US-004: Staff login to the management area

**Priority:** critical

> As a library staff member, I want to log in with my email and password, so that I can access the catalogue management area to maintain records.

| ID | Given | When | Then |
|---|---|---|---|
| AC-010 | I am a staff member with a valid email/password account | I submit correct credentials on the login page | I am authenticated and taken to the staff catalogue management dashboard |
| AC-011 | I submit an incorrect email or password | the login is processed | access is denied and an error message is shown without revealing which field was wrong |
| AC-012 | I hold the single staff/librarian role | I am logged in | I can create, edit and delete records and upload cover images, as there are no separate admin/editor tiers at launch |

### US-005: Protect the management area from public access

**Priority:** high

> As a library, I want the management area to be accessible only to authenticated staff, so that the public cannot alter catalogue records.

| ID | Given | When | Then |
|---|---|---|---|
| AC-013 | I am an anonymous visitor who is not logged in | I attempt to open any management-area page or action | I am redirected to the staff login and the management function is not performed |
| AC-014 | I am a logged-in staff member | I choose to log out | my session ends and management-area pages again require login |

## EPIC-004: Catalogue Record Management

The management area where authenticated staff manually create, edit and delete item records, which is the only way holdings enter the catalogue. Includes a dashboard for finding and managing records. Changes are reflected in the public Meilisearch index. Excludes bulk import, ISBN auto-lookup and ILS sync.

### US-006: Create an item record

**Priority:** critical

> As a library staff member, I want to manually create a new item record with its bibliographic fields, availability and shelf/location, so that the item appears in the public catalogue.

| ID | Given | When | Then |
|---|---|---|---|
| AC-015 | I am logged in to the management area | I complete the new-item form with title, author, ISBN, publisher, year, subject/category, description, shelf/location and availability status and save | a new item record is stored and becomes searchable in the public catalogue via Meilisearch |
| AC-016 | I submit the new-item form with a required field missing | I attempt to save | the record is not created and the form indicates which required field must be completed |
| AC-017 | manual entry is the only data-entry method | I add items | no bulk import, ISBN auto-lookup or ILS sync is offered in the create flow |

### US-007: Edit an item record

**Priority:** high

> As a library staff member, I want to edit an existing item record including its availability status and shelf/location, so that the catalogue reflects accurate, up-to-date information.

| ID | Given | When | Then |
|---|---|---|---|
| AC-018 | I am logged in and viewing an existing item record | I change one or more fields and save | the record is updated and the changes are reflected in the public catalogue and search index |
| AC-019 | I change an item's staff-set availability status | I save the record | the updated availability status is shown on the public item detail page and is available as a filter value |

### US-008: Delete an item record

**Priority:** medium

> As a library staff member, I want to delete an item record, so that items no longer held by the library are removed from the catalogue.

| ID | Given | When | Then |
|---|---|---|---|
| AC-020 | I am logged in and viewing an existing item record | I choose to delete it and confirm the action | the record is removed from the catalogue and no longer appears in public search or browse results |
| AC-021 | I choose to delete a record | I am asked to confirm | the deletion only proceeds after I confirm, and I can cancel to keep the record |

### US-009: Catalogue management dashboard

**Priority:** high

> As a library staff member, I want a dashboard listing catalogue records with the ability to find and open them, so that I can manage the collection efficiently.

| ID | Given | When | Then |
|---|---|---|---|
| AC-022 | I am logged in to the management area | I open the dashboard | I see a list of existing item records and controls to create a new record and to open an existing record for editing or deletion |
| AC-023 | the catalogue contains many records | I search or filter within the dashboard for a specific item | the matching records are shown so I can select one to manage |

## EPIC-005: Cover Image Management

Staff upload and manage cover images for item records. Uploaded images are stored in Amazon S3 and displayed on public detail pages and results.

### US-010: Upload a cover image

**Priority:** medium

> As a library staff member, I want to upload a cover image when creating or editing an item record, so that the item displays a cover in the public catalogue.

| ID | Given | When | Then |
|---|---|---|---|
| AC-024 | I am logged in and creating or editing an item record | I upload a cover image file and save | the image is stored in Amazon S3 and associated with the item record |
| AC-025 | an item has an uploaded cover image | the public views search results or the item detail page | the stored cover image is displayed for that item |
| AC-026 | I upload a file that is not a valid or supported image | I attempt to save | the upload is rejected with a message and no invalid image is attached to the record |

### US-011: Replace or remove a cover image

**Priority:** low

> As a library staff member, I want to replace or remove an item's cover image, so that I can correct or update the displayed cover.

| ID | Given | When | Then |
|---|---|---|---|
| AC-027 | I am editing an item that already has a cover image | I upload a new image and save | the new image is stored in S3 and shown in place of the previous one |
| AC-028 | I am editing an item that has a cover image | I remove the cover and save | the item is displayed with a placeholder image in the public catalogue |
<!-- sdlc:auto:end epics -->
