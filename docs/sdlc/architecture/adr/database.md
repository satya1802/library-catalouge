<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- SQLite

## Decision Outcome

Chosen option: PostgreSQL 16, because Reliable relational store for the ~10,000 structured bibliographic records and staff accounts, and the source of truth that feeds the Meilisearch index (NFR-004).

## Consequences

Requires a managed instance (RDS) to run, but offers strong consistency and reliability for staff edits.
<!-- sdlc:auto:end adr -->