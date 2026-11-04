<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- GCP
- Azure

## Decision Outcome

Chosen option: AWS, because Hosts the API server, the managed PostgreSQL database and the S3 cover-image storage in one place, keeping cost low for a mostly-idle single-library workload.

## Consequences

Single-cloud dependency, but it meets the modest, cost-sensitive hosting need without over-building.
<!-- sdlc:auto:end adr -->