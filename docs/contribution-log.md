# Contribution log

Only verified project activity is recorded here. No upstream contribution is implied.

| ID | Problem | Branch | Base | Implementation commit | Documentation commit | PR | CI / validation | Merge |
|---|---|---|---|---|---|---|---|---|
| Baseline | Open WebUI source import | `chore/import-open-webui-baseline` | `main` | `24e305f` | Included in baseline | [#2](https://github.com/jifrick/open-webui-reliability-lab/pull/2) | Ruff 3.11/3.12 and unit checks passed; required Format & Build failed because the upstream preparation step mutates translation files during its clean-tree check | Open; not merged |
| P01 | Header archive toggle state | `fix/p01-archive-toggle` | `chore/import-open-webui-baseline` | `d891b95` | `85f135d` | [#3](https://github.com/jifrick/open-webui-reliability-lab/pull/3) | Local build and no-test Vitest command passed; frontend lint/typecheck and browser coverage unavailable or baseline-failing; stacked CI not scheduled | Open; not merged |
| P02 | Search edited structured replies | `fix/p02-search-edited-replies` | `fix/p01-archive-toggle` | `df4dcb0` | `b98fdc0` | [#4](https://github.com/jifrick/open-webui-reliability-lab/pull/4) | Python compilation and direct structured-output extraction passed; targeted Ruff reports pre-existing diagnostics; no backend pytest suite; stacked CI not scheduled | Open; not merged |
| P03 | Message-pair shortcut collision | `fix/p03-message-pair-shortcut` | `fix/p02-search-edited-replies` | `b635299` | `7e90834` | [#5](https://github.com/jifrick/open-webui-reliability-lab/pull/5) | Keyboard predicate harness and frontend build passed; browser regression infrastructure unavailable; stacked CI not scheduled | Open; not merged |
| P05 | Active deletion folder refresh | `fix/p05-folder-delete-refresh` | `fix/p03-message-pair-shortcut` | `e83564e` | `a2a41ba` | [#6](https://github.com/jifrick/open-webui-reliability-lab/pull/6) | Source wiring check and frontend build passed; targeted ESLint reports pre-existing errors; browser regression infrastructure unavailable; stacked CI not scheduled | Open; not merged |

The final state is intentionally incomplete: no PR is represented as merged, no upstream acceptance is claimed, and P06 was not started. The root `CHANGELOG.md` remains the upstream application changelog; lab closeout notes are recorded in `LAB_CHANGELOG.md`.
