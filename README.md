# Open WebUI Reliability Lab

This repository is an independent engineering lab built on the actual [Open WebUI](https://github.com/open-webui/open-webui) source. It is **not the official Open WebUI repository**, and it is not affiliated with, endorsed by, or operated by Open WebUI Inc. Work and pull requests here are internal to this repository; they do not represent upstream contributions or acceptance.

The application in this checkout retains its Open WebUI identity and branding. The source snapshot was imported from `open-webui/open-webui` branch `main`, commit [`8bd8b4fac5e059578ac0c74b3c18d11139f88b7d`](https://github.com/open-webui/open-webui/commit/8bd8b4fac5e059578ac0c74b3c18d11139f88b7d), version `0.11.4`. Upstream attribution and license files are preserved. Read [`LICENSE`](LICENSE), [`LICENSE_NOTICE`](LICENSE_NOTICE), [`LICENSE_HISTORY`](LICENSE_HISTORY), and [`CONTRIBUTOR_LICENSE_AGREEMENT`](CONTRIBUTOR_LICENSE_AGREEMENT) before redistributing or contributing. In particular, the Open WebUI license contains branding requirements; this lab does not remove or alter that branding.

The upstream README snapshot is retained as [`UPSTREAM_README.md`](UPSTREAM_README.md), and the original upstream changelog remains at [`CHANGELOG.md`](CHANGELOG.md) because Open WebUI parses it at startup. Lab-only release notes are in [`LAB_CHANGELOG.md`](LAB_CHANGELOG.md). The upstream GitHub PR template is retained as `.github/upstream-pull_request_template.md`; `.github/pull_request_template.md` is the lab's internal template.

## Engineering lab

- [`docs/PRD.md`](docs/PRD.md) defines scope and candidate selection.
- [`docs/problem-research.md`](docs/problem-research.md) records research decisions and verification state.
- [`problems/`](problems/) contains the P01-P15 issue dossiers.
- [`docs/architecture.md`](docs/architecture.md) describes the imported source, development setup, and verified baseline.
- [`docs/workflow.md`](docs/workflow.md) describes the research-to-delivery workflow.
- [`docs/contribution-log.md`](docs/contribution-log.md) contains verified lab activity only.

The 15 reports were frozen as a research snapshot. Four were subsequently implemented in this lab as focused internal changes: P01, P02, P03, and P05. Their PRs remain open and unmerged because the baseline PR has a required `Format & Build` failure. The other dossiers remain researched only or blocked; no claim is made that all 15 issues were solved or accepted upstream.

## Final closeout status

- Implemented in the lab: P01, P02, P03, and P05.
- Internal PRs: [#3](https://github.com/jifrick/open-webui-reliability-lab/pull/3), [#4](https://github.com/jifrick/open-webui-reliability-lab/pull/4), [#5](https://github.com/jifrick/open-webui-reliability-lab/pull/5), and [#6](https://github.com/jifrick/open-webui-reliability-lab/pull/6), stacked after baseline PR [#2](https://github.com/jifrick/open-webui-reliability-lab/pull/2).
- Merge state: no lab PR is merged; no change was accepted upstream.
- Validation: targeted builds and focused source/harness checks passed where available. Frontend lint/type checking, browser coverage, backend test coverage, and the baseline required check retain documented limitations.
- Research-only or blocked work: P04, P06-P15 were not implemented. P08, P09, P13, and P14 remain explicitly blocked by unavailable infrastructure or validation dependencies.

Inspect `LAB_CHANGELOG.md`, `docs/contribution-log.md`, and the linked PRs for the verified closeout record. The root `CHANGELOG.md` remains the upstream Open WebUI changelog required by the application; lab-specific notes are kept separately.

## Development

The upstream project supports Node.js 18.13 through 22.x and npm 6 or later, and Python 3.11 or 3.12. Its manifests use npm (`package-lock.json`) for the Svelte/Vite frontend and uv (`uv.lock`) for the Python backend.

Install frontend dependencies and run the frontend in one terminal:

```powershell
npm ci --force
npm run dev
```

Run the Python backend in another terminal from the repository root:

```powershell
uv sync --locked
$env:WEBUI_SECRET_KEY = py -3.11 -c "import secrets; print(secrets.token_urlsafe(48))"
uv run open-webui dev --host 127.0.0.1 --port 8080
```

The Vite development server defaults to port 5173 and proxies API/WebSocket requests to `http://localhost:8080`; set `WEBUI_BACKEND_URL` to override that target. `open-webui dev` requires `WEBUI_SECRET_KEY` to be set; the command above generates a process-local random development key. The backend's `/docs` and API are available at port 8080 when it starts successfully.

Useful upstream checks:

```powershell
npm run check
npm run test:frontend
npx eslint .
uv run ruff format --check . --exclude .venv --exclude venv
uv run ruff check --select=F --ignore=F401,F403,F405,F541,F811,F841 .
npm run build
```

`npx eslint .` is the non-mutating frontend lint equivalent; the upstream `npm run lint:frontend` invokes ESLint with `--fix`. The repository's browser regression workflow is configured to call the separate `open-webui/tests` repository; the imported source does not contain that full end-to-end suite. See [`docs/architecture.md`](docs/architecture.md) for baseline results and limitations.

## Contributing

Read [`CONTRIBUTING.md`](CONTRIBUTING.md) and the relevant issue dossier before changing code. Upstream requires an explicit maintainer request for code PRs except localization; this lab will not make unsolicited upstream submissions. A repository PR here is an internal lab PR only.
