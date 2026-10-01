# Contributing

This is an independent research and engineering lab, not an Open WebUI project. Contributions must not imply affiliation or upstream acceptance.

## Before proposing work

1. Read [`docs/PRD.md`](docs/PRD.md), [`docs/workflow.md`](docs/workflow.md), and the relevant `problems/Pxx.md`.
2. Re-check the upstream issue, linked discussions, pull requests, commits, and current release/dev source. Issue state can change after a dossier is written.
3. Record evidence and the date checked. Distinguish reporter-provided evidence from independently reproduced behavior.
4. Do not implement a report that is closed, fixed, duplicate, invalid, unreproducible, or already being addressed unless the dossier explains a new, independently verified scope.

## Upstream contribution policy

Do not submit an unsolicited code PR to `open-webui/open-webui`. The upstream contribution policy requires an explicit maintainer request for code changes, apart from its stated localization exception. This repository may document a reproduction, regression-test design, or implementation notes without claiming upstream ownership. An upstream PR may be prepared only after a maintainer explicitly requests it.

PRs in this repository are internal to this independent lab. They are not upstream Open WebUI PRs and do not imply acceptance by upstream maintainers.

## Engineering standards

- Work on one problem at a time and keep each change narrowly scoped.
- Reproduce before fixing; add a regression test before or alongside the fix.
- Use the actual package scripts and architecture of the code being changed. Do not invent paths or test results.
- Run targeted tests, broader relevant checks, and inspect the complete diff.
- Record commits, PRs, CI, reviews, and merge status only after verifying each fact.
- Never fabricate issue status, reproduction, test results, maintainer requests, or merges.
- Follow the PR template and update the affected dossier and contribution log.

