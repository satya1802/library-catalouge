# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `b01456c` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
Library catalouge is a generated two-app repository from the Quorq AI SDLC platform: a Vite + Vue 3 frontend containing fully approved, designed screens that currently render seeded sample data, and a Django REST Framework backend that currently only exposes a health check endpoint (no data model or API routes have been approved/generated yet). The backend is intended to be backed by PostgreSQL 16 and Meilisearch, both provided via docker-compose for local development, and deployed on AWS per the approved stack.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (for the Vite/Vue frontend, npm scripts)
- Python 3.12 (matches backend/Dockerfile base image)
- pip
- Docker and Docker Compose (for PostgreSQL and Meilisearch via docker-compose.yml)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. Clone the repository
2. cd frontend && npm install
3. cd backend && pip install -r requirements.txt
4. From the repo root, run `docker-compose up -d` to start PostgreSQL and Meilisearch (optional until the backend has models/services wired to them)
5. cd backend && python manage.py migrate --run-syncdb (creates tables from backend/app/models.py, which is currently empty)
6. Copy .env.example to .env and fill in any required values if present
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| frontend dev | `cd frontend && npm run dev` | Starts the Vite dev server for the Vue 3 frontend |
| frontend build | `cd frontend && npm run build` | Bundles the frontend app (does not typecheck) |
| frontend preview | `cd frontend && npm run preview` | Serves the production build locally |
| backend runserver | `cd backend && python manage.py runserver` | Runs the Django development server (health check at /health) |
| backend migrate | `cd backend && python manage.py migrate --run-syncdb` | Creates database tables from models (scaffold ships no migrations) |
| docker services | `docker-compose up` | Starts PostgreSQL 16 and Meilisearch containers for local development |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `frontend/` | Vite + Vue 3 app with approved, seeded-data screens |
| `frontend/src` | Application source: entry point (main.ts), lib utilities (api.ts, icons.ts, navigate.ts, ui.ts), styles |
| `frontend/e2e` | Playwright end-to-end acceptance tests |
| `backend/` | Django 5.1 REST Framework service, currently scaffolded with only a health check |
| `backend/app` | The generated Django app: models.py, serializers.py, services.py, urls.py, and api/ for future views |
| `backend/config` | Django project config: settings.py, root urls.py, wsgi.py |
| `backend/tests` | Pytest test suite (currently test_health.py) |
| `.devcontainer/` | VS Code devcontainer configuration |
| `.quorq/` | Quorq platform metadata (handoff.json) |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend follows standard Django project layout: config/ for project settings/urls, app/ for the single Django application
- Serializers in backend/app/serializers.py are expected one-per-model, mirroring backend/app/models.py
- API views are organized under backend/app/api/, one module per architecture component that owns endpoints
- Frontend screens are plain single-file Vue components under src/, written against the shared component/icon vocabulary in src/lib/ui.ts and src/lib/icons.ts
- Frontend API calls are centralized through apiFetch in src/lib/api.ts, which attaches a bearer token (session token or shared API secret) and joins API_BASE_URL with request paths
- Component tests live beside the components they test, run via Vitest in jsdom; Playwright e2e tests live separately in frontend/e2e/
- Code style enforced via ESLint/Prettier (frontend) and ruff (backend)
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| frontend unit tests | `cd frontend && npm test` | Runs Vitest component tests |
| frontend lint | `cd frontend && npm run lint` | Runs ESLint over the frontend codebase |
| frontend format | `cd frontend && npm run format` | Formats code with Prettier |
| frontend typecheck | `cd frontend && npm run typecheck` | Runs vue-tsc --noEmit over the TypeScript (build does not typecheck) |
| frontend e2e | `cd frontend && npm run test:e2e` | Runs Playwright acceptance tests; boots backend and frontend locally unless E2E_BASE_URL is set |
| e2e browser install | `cd frontend && npm run test:e2e:install` | Installs the Chromium browser Playwright needs |
| backend tests | `cd backend && pytest` | Runs the pytest suite (currently just the health check test) |
| backend lint | `cd backend && ruff check .` | Lints the backend code with ruff |
| backend format | `cd backend && ruff format .` | Formats the backend code with ruff |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Backend returns errors or crashes because tables don't exist | Run `python manage.py migrate --run-syncdb` from backend/ -- the scaffold ships no migrations, so this creates tables directly from models.py |
| Frontend screens show seeded data instead of real API data | This is expected: the generated screens currently render seeded sample data by design. Wiring a screen to apiFetch in src/lib/api.ts is the next step, not something already done |
| `npm run build` succeeds but TypeScript errors exist | The build does not typecheck; run `npm run typecheck` (vue-tsc --noEmit) separately to catch TS issues |
| Playwright e2e tests fail to start the backend | The webServer config expects a `.venv/bin/python` in backend/ when E2E_BASE_URL is not set -- ensure a virtualenv named .venv exists in backend/ with dependencies installed, or set E2E_BASE_URL to point at a running deployment |
| docker-compose services (Postgres/Meilisearch) not reachable by backend | The backend currently has no services.py wiring to these containers beyond configured_services(); confirm docker-compose is up with `docker-compose up -d` and check backend/app/services.py for how services are detected/configured |
| Health check container reports unhealthy | The Dockerfile healthcheck polls http://localhost:8000/health using Python's urllib; ensure gunicorn/runserver is bound to 0.0.0.0:8000 inside the container and migrations ran first |
<!-- sdlc:auto:end troubleshooting -->
