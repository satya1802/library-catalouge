# Library catalouge — SDLC Documentation

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start overview -->
Build a Library Catalogue System

**Documentation version:** v2
<!-- sdlc:auto:end overview -->

## Contents

<!-- sdlc:auto:start contents -->
| Document | State |
|---|---|
| [Requirements](requirements/README.md) | v1 approved |
| [Architecture](architecture/README.md) | 15 entries |
| [Development](development/README.md) | 1 entry |
| [Testing](testing/README.md) | 1 entry |
| [Deployment](deployment/README.md) | 1 entry |
| [Maintenance](maintenance/README.md) | 1 entry |
| [Data model](architecture/data-model.md) | generated |
| [API (OpenAPI)](architecture/api/openapi.yaml) | generated |
| [Decisions (ADRs)](architecture/adr/README.md) | 4 decisions |
| [Sprint plan](development/sprint-plan.md) | not generated yet |
| [Developer guide](development/developer-guide.md) | not generated yet |
| [API reference](development/api-reference.md) | 0 routes |
| [Test report](testing/test-report.md) | not run yet |
| [Tests](testing/test-cases.md) | 0 tests |
| [Deployment guide](deployment/deployment-guide.md) | not deployed yet |
| [Runbook](deployment/runbook.md) | not deployed yet |
| [Release notes](deployment/release-notes.md) | 0 releases |
| [Traceability matrix](traceability-matrix.md) | 11 requirements |
| [Changelog](CHANGELOG.md) | 2 versions |
<!-- sdlc:auto:end contents -->

## Open gaps

<!-- sdlc:auto:start open-gaps -->
### Architecture

- Overview does not state which of US-001..US-011 each architectural capability satisfies; mapping must come from the requirements phase.
- No explicit requirement id(s) attached to this component in the architecture input.
- Specific SMTP provider/configuration not specified.
- Not mapped to a specific US- requirement id in the architecture input.
- Supported image formats/size limits are not specified.
- No explicit requirement id(s) attached to these entities in the architecture input.
- Field-level validation rules (e.g. ISBN format, required vs optional fields) are not specified at the entity level.
- No explicit requirement id(s) attached to individual endpoints in the architecture input.
- Request/response payload schemas and pagination details for list endpoints are not specified.
- Meilisearch and the Django API share a single EC2 instance with no redundancy or failover described; this single point of failure is not addressed as a decision or accepted risk in the input.
- No explicit requirement id(s) attached to the deployment topology.
- Technology decisions cite NFR-004 and NFR-006, but the known requirement keys for this project are only US-001 through US-011 -- these NFR ids are not resolvable against the given requirement set and should be confirmed/reconciled with the requirements phase.

### Development

- Development phase has not started -- no tickets, code, or build artifacts available to document.

### Testing

- Testing phase has not started -- no test plan or results available.

### Deployment

- No deployment recorded yet -- environment setup, release process, and runtime configuration (AWS, S3, Meilisearch, SMTP) are undocumented.

### Maintenance

- No maintenance activity recorded -- project has not reached post-deployment.
<!-- sdlc:auto:end open-gaps -->
