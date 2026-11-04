<!-- sdlc:auto:start adr -->
# ADR-0002: Use Django 5.1 for Backend

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- Django 5.1
- FastAPI
- Express

## Decision Outcome

Chosen option: Django 5.1, because Provides the REST API, built-in auth/session handling for staff login, an admin-grade ORM for item records, and clean integration points for Meilisearch and S3.

## Consequences

Heavier than a micro-framework, but its batteries-included auth, validation and ORM reduce custom code for this CRUD-centric app.
<!-- sdlc:auto:end adr -->