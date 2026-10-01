# Contribution log

Only verified project activity is recorded here. No upstream contribution is implied.

| ID | Problem | Branch | Commit | PR | CI | Review | Merge |
|---|---|---|---|---|---|---|---|
| Baseline | Open WebUI source import | `chore/import-open-webui-baseline` | `24e305f` | [#2](https://github.com/jifrick/open-webui-reliability-lab/pull/2) | Ruff 3.11/3.12 and unit checks passed; Format & Build failed because the upstream preparation step mutates translation files during its clean-tree check | Not yet reviewed | Open; merge blocked by failed required check |
| P01 | Header archive toggle state | `fix/p01-archive-toggle` | `d891b95` | [#3](https://github.com/jifrick/open-webui-reliability-lab/pull/3) | Local build and no-test Vitest command passed; baseline lint/typecheck remain failed; stacked CI pending | Not yet reviewed | Open; blocked by baseline PR |
| P02 | Search edited structured replies | `fix/p02-search-edited-replies` | `df4dcb0` | [#4](https://github.com/jifrick/open-webui-reliability-lab/pull/4) | Python compile and direct structured-output extraction passed; targeted Ruff reports pre-existing module diagnostics; no backend pytest suite; stacked CI not started | Not yet reviewed | Open; merge blocked by baseline PR |
| P03 | Message-pair shortcut collision | `fix/p03-message-pair-shortcut` | `b635299` | [#5](https://github.com/jifrick/open-webui-reliability-lab/pull/5) | Keyboard predicate harness and frontend build passed; browser regression infrastructure unavailable; stacked CI not started | Not yet reviewed | Open; merge blocked by baseline PR |
| P05 | Active deletion folder refresh | `fix/p05-folder-delete-refresh` | In progress | Not opened | Source wiring check and frontend build passed; targeted ESLint reports pre-existing baseline errors; browser regression infrastructure unavailable | Not yet reviewed | Not started |

The research baseline was committed before implementation work began. P01 implementation is committed and submitted as stacked internal PR #3; merge is blocked by the baseline PR's pre-existing required-check failure.
