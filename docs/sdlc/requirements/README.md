# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-STACK · approved · source: requirements:technology-stack

Product owner approved an exact technology stack for the Library Catalogue System: Django 5.1 backend, PostgreSQL 16 database, Vue 3 frontend, hosted on AWS.

Stack approved by the product owner, to be designed against exactly as specified (no substitutions):
- Backend: Django 5.1
- Database: PostgreSQL 16 -- chosen because catalogue data (title, author, ISBN, subject, availability, shelf location) is structured and relational; handles ~10,000 items with modest traffic, keyword search, and author/subject/availability filters while keeping data reliable for staff edits.
- Frontend: Vue 3
- Cloud: AWS -- chosen for cheap, reliable hosting for a single library with modest, mostly-idle traffic; covers server, PostgreSQL DB, and cover-image storage in one place at low running cost. Only option offered.

### REQ-SERVICES · approved · source: requirements:services

Three runtime services approved: Amazon S3 for cover images, Meilisearch for catalogue search, and an SMTP server for staff account emails.

Services approved by product owner, to be included exactly as specified:
- File storage: Amazon S3 (SaaS; env vars AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION, S3_BUCKET) -- stores staff-uploaded cover images cheaply, serves them to public pages without bloating the database.
- Search: Meilisearch (container getmeili/meilisearch:v1.10.3) -- provides fast, typo-tolerant keyword search across title/author/subject so misspellings still find results; plain DB search would miss these.
- Email: SMTP server (SaaS; env vars SMTP_HOST, SMTP_PORT, SMTP_USER, SMTP_PASSWORD) -- sends password-reset and account-setup emails for staff email/password accounts; standard SMTP suffices at this low volume.
Required API keys/env vars for the app: AWS_ACCESS_KEY_ID, AWS_REGION, AWS_SECRET_ACCESS_KEY, S3_BUCKET, SMTP_HOST, SMTP_PASSWORD, SMTP_PORT, SMTP_USER.

**Related:** US-001, US-002, US-004, US-010, US-011

### REQ-SCOPE · draft · source: requirements:scope

Delivery scope is lightweight, requirements version 1. In-scope capabilities cover public search/browse, item detail pages, staff login, manual record CRUD, and cover image management. Several library-system features are explicitly out of scope.

In scope: public keyword search of holdings; item detail pages (bibliographic fields, cover, shelf/location, availability); browse/filter by author, subject, availability; staff login to management area; manual item create/edit/delete; cover image upload; staff dashboard; Meilisearch for search/filtering; S3 for cover image storage; public browsing without login, staff-only authentication.
Out of scope -- not to be designed or built: check-out/return, due dates, overdue tracking; holds/reservations and reservation queues; member/patron accounts, borrowing history, fines; bulk import (CSV/MARC), ISBN auto-lookup, live ILS sync.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

### US-001 · draft · source: requirements:US-001

Critical story: anonymous public visitors can keyword-search the catalogue without logging in, with results powered by Meilisearch.

As a member of the public, search the catalogue by keyword without logging in to find items matching title, author, subject or description.
Acceptance criteria:
- AC-001: Anonymous visitor on public home page enters a keyword and submits; system returns relevance-ranked matches via Meilisearch, no login required.
- AC-002: A keyword matching no items shows an empty-results message; search box remains available to refine the query.
- AC-003: Against a catalogue of up to 10,000 items, results display each item's title, author and availability status.

**Related:** EPIC-001, AC-001, AC-002, AC-003, NFR-004, NFR-005

### US-002 · draft · source: requirements:US-002

High-priority story: public users can filter/browse results by author, subject/category and availability, combinable with keyword search.

As a member of the public, filter and browse catalogue results by author, subject/category and availability to narrow results.
Acceptance criteria:
- AC-004: Applying an author, subject/category or availability filter updates the result list via Meilisearch filtering.
- AC-005: Combining keyword search with filters returns results matching both the keyword and all active filters.
- AC-006: Clearing a filter updates the list to remove that filter's restriction.

**Related:** EPIC-001, AC-004, AC-005, AC-006, NFR-004

### US-003 · draft · source: requirements:US-003

High-priority story: public visitors can open a full item detail page from search/browse results.

As a member of the public, open an item's detail page from search or browse results to see all information the library holds about that item.
Acceptance criteria:
- AC-007: Selecting an item from results opens a detail page showing title, author, ISBN, publisher, year, subject/category, description, cover image, shelf/location and availability status.
- AC-008: If no cover image is uploaded, a placeholder image is shown and remaining fields display normally.
- AC-009: The staff-set availability status is displayed as read-only information.

**Related:** EPIC-002, AC-007, AC-008, AC-009

### US-004 · draft · source: requirements:US-004

Critical story: staff log in with email/password to reach the management dashboard under a single staff/librarian role.

As a library staff member, log in with email and password to access the catalogue management area.
Acceptance criteria:
- AC-010: Valid credentials authenticate the staff member and route to the management dashboard.
- AC-011: Incorrect email or password is denied with a generic error that does not reveal which field was wrong.
- AC-012: The single staff/librarian role can create, edit, delete records and upload cover images; no separate admin/editor tiers at launch (explicit assumption).

**Related:** EPIC-003, AC-010, AC-011, AC-012, NFR-002, NFR-007

### US-005 · draft · source: requirements:US-005

High-priority story: the management area is restricted to authenticated staff; anonymous users are redirected to login.

As a library, ensure the management area is accessible only to authenticated staff so the public cannot alter catalogue records.
Acceptance criteria:
- AC-013: An anonymous visitor attempting to open any management page/action is redirected to staff login; the action is not performed.
- AC-014: Logging out ends the session; management pages again require login.

**Related:** EPIC-003, AC-013, AC-014, NFR-001

### US-006 · draft · source: requirements:US-006

Critical story: logged-in staff manually create a new item record, which then becomes searchable publicly.

As a library staff member, manually create a new item record with bibliographic fields, availability and shelf/location so it appears in the public catalogue.
Acceptance criteria:
- AC-015: Completing the new-item form (title, author, ISBN, publisher, year, subject/category, description, shelf/location, availability) and saving stores the record and makes it searchable via Meilisearch.
- AC-016: Missing a required field blocks save and indicates which field is required.
- AC-017: Manual entry is the only data-entry method; no bulk import, ISBN auto-lookup or ILS sync is offered.

**Related:** EPIC-004, AC-015, AC-016, AC-017

### US-007 · draft · source: requirements:US-007

High-priority story: staff edit existing item records, with changes reflected publicly and in the search index.

As a library staff member, edit an existing item record, including availability status and shelf/location, to keep the catalogue accurate.
Acceptance criteria:
- AC-018: Changing and saving fields updates the record, the public catalogue and the search index.
- AC-019: Changing availability status updates the public detail page and the availability filter.

**Related:** EPIC-004, AC-018, AC-019

### US-008 · draft · source: requirements:US-008

Medium-priority story: staff delete item records no longer held, with a confirmation step.

As a library staff member, delete an item record so items no longer held are removed from the catalogue.
Acceptance criteria:
- AC-020: Confirmed deletion removes the record so it no longer appears in public search or browse results.
- AC-021: Deletion requires confirmation; cancelling keeps the record.

**Related:** EPIC-004, AC-020, AC-021

### US-009 · draft · source: requirements:US-009

High-priority story: a staff dashboard lists catalogue records with create/find/open controls.

As a library staff member, use a dashboard listing catalogue records to find and open them for management.
Acceptance criteria:
- AC-022: The dashboard lists existing records and provides controls to create a new record and open an existing one for edit/delete.
- AC-023: Searching/filtering within the dashboard surfaces matching records for selection.

**Related:** EPIC-004, AC-022, AC-023

### US-010 · draft · source: requirements:US-010

Medium-priority story: staff upload a cover image for an item, stored in S3 and shown publicly.

As a library staff member, upload a cover image when creating or editing an item record so the item displays a cover publicly.
Acceptance criteria:
- AC-024: Uploading an image on save stores it in Amazon S3 and associates it with the record.
- AC-025: The public sees the stored cover image in search results and on the detail page.
- AC-026: Uploading an invalid/unsupported image file is rejected with a message; no invalid image is attached.

**Related:** EPIC-005, AC-024, AC-025, AC-026, NFR-003

### US-011 · draft · source: requirements:US-011

Low-priority story: staff can replace or remove an item's existing cover image.

As a library staff member, replace or remove an item's cover image to correct or update what is displayed.
Acceptance criteria:
- AC-027: Uploading a new image on an item that already has a cover replaces it in S3 and in display.
- AC-028: Removing a cover and saving falls back to the placeholder image publicly.

**Related:** EPIC-005, AC-027, AC-028

### NFR-001 · draft · source: requirements:NFR-001

Security NFR: management area must be reachable only by authenticated staff.

All management-area pages/actions must be reachable only by authenticated staff. Anonymous attempts redirect to staff login without performing the action. Logging out ends the session, requiring re-login for those pages.

**Related:** US-005

### NFR-002 · draft · source: requirements:NFR-002

Security NFR: failed logins must show a generic error without revealing which credential was wrong.

On failed login, the system must show a generic error and must not reveal whether the email or password was incorrect.

**Related:** US-004

### NFR-003 · draft · source: requirements:NFR-003

Security NFR: only valid, supported image files may be stored as cover images.

Cover image uploads must be validated so only supported image files are stored; any other file type is rejected with a message, and no invalid image is attached to the record.

**Related:** US-010

### NFR-004 · draft · source: requirements:NFR-004

Scalability NFR: the catalogue must reliably support up to ~10,000 items with modest traffic.

Target: up to approximately 10,000 item records with modest traffic; search and browse results must remain reliable at this scale.

**Related:** US-001, US-002

### NFR-005 · draft · source: requirements:NFR-005

Usability NFR: keyword search must tolerate misspellings and rank by relevance.

Keyword search must tolerate misspellings so a mistyped title, author or subject still finds matching items, with results ranked by relevance.

**Related:** US-001

### NFR-006 · draft · source: requirements:NFR-006

Compatibility NFR: delivered as a responsive web app for desktop and mobile browsers; no native app.

Applies to whole system. The product is a responsive web application working in desktop and mobile browsers; there is no native mobile app.

### NFR-007 · draft · source: requirements:NFR-007

Security NFR (accepted risk): single staff role at launch, no admin/editor tiers.

At launch there is a single staff/librarian role with full create/edit/delete/cover-management access; no separate admin and editor tiers. Finer-grained access control is explicitly accepted as out of scope for now.

**Related:** US-004

### NFR-008 · draft · source: requirements:NFR-008

Availability NFR (accepted risk): offline use is explicitly out of scope.

Applies to whole system. The catalogue is only usable online; offline mode is accepted as out of scope, so the application is not expected to work without a network connection.
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
