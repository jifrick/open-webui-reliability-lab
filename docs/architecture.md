# Architecture and baseline

## Source provenance and identity

This repository includes the Open WebUI source tree copied from the official repository's `main` branch at commit `8bd8b4fac5e059578ac0c74b3c18d11139f88b7d` (version `0.11.4`, observed 2026-10-01). The source snapshot is available at [the upstream commit](https://github.com/open-webui/open-webui/commit/8bd8b4fac5e059578ac0c74b3c18d11139f88b7d). The imported source remains at the repository root so the upstream project manifests and development commands work in place.

The import is a source snapshot, not a GitHub fork and not a history-preserving Git merge; this lab's Git history does not contain the upstream commit ancestry. The exact imported source revision is recorded to make the provenance explicit, and the official repository is configured as the read-only `upstream` remote. `UPSTREAM_README.md`, root `CHANGELOG.md`, `.github/upstream-pull_request_template.md`, `LICENSE`, `LICENSE_NOTICE`, `LICENSE_HISTORY`, and `CONTRIBUTOR_LICENSE_AGREEMENT` preserve the upstream README, release notes, template, and notices. Lab-only release notes are kept separately in `LAB_CHANGELOG.md`; retaining the upstream changelog at its original root path is required by the application's version parser. Source files and assets retain their original Open WebUI naming and branding.

This is an independent repository and is not affiliated with or endorsed by Open WebUI Inc. The upstream license is identified by GitHub as "Other"; it grants redistribution under conditions, including retaining notices and Open WebUI branding. Consult the full checked-in license documents rather than treating this source as MIT-licensed. The upstream contributor license agreement is also preserved.

## Upstream structure and stack

Observed at the imported revision:

- **Frontend:** Svelte 5, SvelteKit 2, TypeScript, Vite 5, Tailwind CSS 4. `package.json` and `package-lock.json` define npm as the package manager; package engines require Node.js `>=18.13.0 <=22.x.x` and npm `>=6`.
- **Backend:** Python 3.11 or 3.12, FastAPI, Uvicorn, Pydantic, SQLAlchemy, and Alembic. `pyproject.toml` and `uv.lock` define the project and locked dependencies; uv is the documented lockfile-oriented environment manager.
- **Frontend source:** `src/routes/` contains route/page and app-shell components; `src/lib/` contains components, stores, APIs, and utilities; `static/` contains static assets.
- **Backend source:** `backend/open_webui/main.py` wires the ASGI application; `backend/open_webui/routers/` contains HTTP endpoints; `models/`, `retrieval/`, `tools/`, `utils/`, and `migrations/` contain persistence, document retrieval, tool execution, shared services, and schema migrations.
- **Build output:** `npm run build` runs `scripts/prepare-pyodide.js` and Vite production build. The Python package build configuration includes the generated frontend under `backend/open_webui/frontend`.

The exact relevant paths for individual issues are to be recorded in their dossiers only after tracing the current code. A text search for likely terms is a navigation aid, not root-cause evidence.

## Development setup

The upstream Vite config defaults its backend proxy to `http://localhost:8080` and accepts `WEBUI_BACKEND_URL` as an override. The frontend dev server defaults to port 5173. The CLI in `backend/open_webui/__init__.py` exposes `open-webui dev` and `open-webui serve`; `dev` enables reload. These are the source-defined commands:

```powershell
# Terminal 1, repository root
npm ci --force
npm run dev

# Terminal 2, repository root
uv sync --locked
uv run open-webui dev --host 127.0.0.1 --port 8080
```

The command above is intended for Python 3.11/3.12 and a compatible uv installation. The CLI `dev` command requires the secret key to be configured, unlike `serve`, which generates one automatically. The imported upstream `backend/dev.sh` is Unix-specific; use the Python CLI on Windows.

## Tests, lint, type checking, and build

Commands exposed by upstream manifests/workflows:

| Area | Command | Source evidence |
|---|---|---|
| Frontend unit tests | `npm run test:frontend` | `package.json` script: Vitest with `--passWithNoTests` |
| Frontend type check | `npm run check` | SvelteKit sync and `svelte-check` |
| Frontend lint | `npx eslint .` | Same ESLint checker as `lint:frontend`, without upstream's `--fix` mutation |
| Python format | `uv run ruff format --check . --exclude .venv --exclude venv` | `.github/workflows/backend.yaml` |
| Python static checks | `uv run ruff check --select=F --ignore=F401,F403,F405,F541,F811,F841 .` | `.github/workflows/backend.yaml` |
| Production frontend build | `npm run build` | `package.json`, `.github/workflows/frontend.yaml` |
| Full package lint script | `npm run lint` | Runs ESLint with `--fix`, Svelte check, and `pylint backend/`; not a safe baseline command because ESLint may edit files |

The checked-out source does not include the separate full browser-regression suite: `.github/workflows/regression.yaml` calls the reusable workflow in `open-webui/tests`. The `test/` folder in this repository holds fixtures; `npm run test:frontend` explicitly allows no tests. Record that distinction when interpreting a successful empty Vitest run.

Vitest is the frontend unit-test runner declared by the npm script, but there are no frontend test files in the imported `src/` tree. Python's development dependency group includes `pytest-asyncio`, but no backend test files or backend pytest job were found; `.github/workflows/backend.yaml` runs Ruff format and static checks only. Do not treat dependency presence as an existing test suite.

## Baseline environment and exact results

The official default branch and source revision were checked via GitHub on 2026-10-01. Repository metadata reported default branch `main`; latest release was `v0.11.4`, published 2026-09-21. The import clone resolved to `8bd8b4fac5e059578ac0c74b3c18d11139f88b7d`.

| Check | Exact command | Result |
|---|---|---|
| Frontend dependency installation | Node `v22.23.2`; `CYPRESS_INSTALL_BINARY=0 npm ci --force` | **Passed**, exit 0; 1,119 packages installed. Cypress's separate browser binary was skipped after its postinstall did not complete in this environment; the repository's configured browser regression workflow uses the separate `open-webui/tests` repository. npm reported 35 audit advisories; no dependency changes were made. |
| Backend dependency installation | Python `3.11.9`; `py -3.11 -m uv sync --locked` | **Passed**, exit 0; resolved 328 packages, built the editable project, installed 245 packages. The hatch build hook ran the frontend build. |
| Frontend development startup | `npm run dev` | **Passed**, server returned HTTP 200 at `http://127.0.0.1:5173/`; response contained the Open WebUI title. |
| Backend development startup | `$env:WEBUI_SECRET_KEY = py -3.11 -c "import secrets; print(secrets.token_urlsafe(48))"; $env:RAG_EMBEDDING_MODEL_AUTO_UPDATE = 'false'; py -3.11 -m uv run open-webui dev --host 127.0.0.1 --port 8080` | **Passed basic startup smoke test**: Uvicorn started and `curl.exe -i http://127.0.0.1:8080/health` returned HTTP 200 with `{"status":true}`. Initial default startup began downloading the 30-file Sentence Transformers model and did not reach health in the observation window. That attempt was stopped; with model auto-update disabled, the partially cached model logged a load error, which Open WebUI catches, and the server became healthy. This verifies API startup only; local embeddings/RAG were not verified and are unavailable with the incomplete cache. |
| Frontend tests | `npm run test:frontend` | **Passed**, exit 0, but Vitest reported no test files. This is not evidence of regression coverage. |
| Frontend lint | `npx eslint .` | **Failed**, exit 2: `@typescript-eslint/no-unused-vars` throws a TypeError while linting `src/lib/components/chat/AskUserCard.svelte`. No source changes were made to suppress it. |
| Type check | `npm run check` | **Failed**, exit 1: `svelte-check found 6,980 errors and 198 warnings in 341 files`, including repeated `i18n` store diagnostics. No source changes were made. |
| Python format | `py -3.11 -m uv run ruff format --check . --exclude .venv --exclude venv` | **Passed**, exit 0; 530 files already formatted. |
| Python static checks | `py -3.11 -m uv run ruff check --select=F --ignore=F401,F403,F405,F541,F811,F841 --output-format=github .` | **Passed**, exit 0. |
| Production frontend build | Node `v22.23.2`, `NODE_OPTIONS=--max-old-space-size=8192 npm run build` | **Passed**, exit 0; Vite built 6,280 modules and adapter-static wrote `build/` in 3m32s. Svelte emitted warnings. |

The local system initially had Node `v24.11.1`, outside upstream's engine range, no Python installation, and no uv. Node `v22.23.2`, Python `3.11.9`, and uv `0.12.21` were installed for this validation. Docker is unavailable. Both development servers returned HTTP 200, and the production frontend build passed. Full baseline validation is **not green**: frontend lint and type checking fail, the Vitest command finds no tests, Cypress's browser binary is absent, the backend has no in-tree test suite, and the backend smoke test ran without a usable embedding model. These limitations are recorded rather than masked with source changes. The configured end-to-end regression suite is external. The research gate is complete, but the recorded baseline test limitations remain unresolved and must be considered before any future implementation.

## Research snapshot source comparison

On 2026-10-01, the official repository still reported `main` as its default branch and `v0.11.4` as the latest release (published 2026-09-21). The imported `main` source is `8bd8b4fac5e059578ac0c74b3c18d11139f88b7d`. The live upstream `dev` branch was separately fetched and compared at `015dbc8619568ac077ec1d782661836a63691ba1` (198 commits ahead and 48 behind `main`). All retained P01-P15 code paths were checked against dev for unreleased changes; none resolves the retained reported behavior. The relevant exception is P07: dev contains merged #30453 bulk archive/delete tag handling, but its single-chat archive path still loses the original tag display name. Each dossier records related history and any reproduction boundary.

The research gate is complete for this dated snapshot and is summarized in [`PRD.md`](PRD.md) and [`problem-research.md`](problem-research.md). In-memory/fake-dependency harnesses were used where noted and were not committed; no application source or regression tests were added or modified during research. Before any future implementation, refresh issue/PR/release/dev state and address the documented baseline test limitations.

## Research boundary and implementation gate

This repository contains the actual application source. The dated research snapshot validates exactly 15 current candidates and records reproduction limitations; it does not imply that later issue changes have been reviewed. Revalidate every issue and linked fix history immediately before future implementation. This document records the exact baseline failures and does not represent them as passing.

## Implementation closeout

The authorized implementation phase stopped after four focused internal changes: P01, P02, P03, and P05. Their branches and PRs are stacked on the baseline import in the order recorded by `docs/contribution-log.md`; all remain open and unmerged. The implementation-specific validations are recorded in the problem dossiers and contribution log. The baseline `Format & Build` failure, frontend lint/type-check failures, absent browser regression environment, absent backend pytest suite, and unavailable external integrations remain limitations. No CI workflow was changed, no required check was bypassed, and no upstream acceptance is claimed.
