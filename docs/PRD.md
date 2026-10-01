# Product Requirements Document

**Project:** Open WebUI Reliability Lab  
**Research snapshot:** 2026-10-01  
**Status:** Research gate finalized 2026-10-01; implementation has not started.

## Executive summary

This independent lab contains a pinned source snapshot of Open WebUI and records real upstream reliability and UX reports. It supports disciplined source inspection, reproduction, regression testing, and internal engineering; any upstream code contribution remains subject to upstream policy and explicit maintainer direction. This is not an official Open WebUI repository, and internal lab activity is not an upstream contribution or acceptance.

Exactly 15 live issue reports have been source-validated for this research snapshot. The original P04 report, #31663, was closed and contradicted by current behavior; it was replaced by open issue #30235, whose source-level mid-entry truncation was reproduced with synthetic records. Each frozen problem dossier records current status, source evidence, reproduction or explicit limitation, root cause, history/duplicate checks, proposed solution, acceptance criteria, test strategy, risks, and dependencies. Provider/browser/external-service behavior that could not be measured is explicitly identified rather than asserted. No application fix, branch, PR, or merge was created. This document freezes the research baseline only; it does not authorize implementation.

## Goals

- Maintain an auditable record of real, current upstream reports and evidence.
- Separate reported behavior, independently reproduced behavior, and source-level findings.
- Produce focused regression coverage and implementation notes only when justified.
- Keep independent lab work clearly distinct from upstream Open WebUI activity.

## Non-goals

- Claiming official affiliation with Open WebUI.
- Fabricating issue validity, implementation, test results, CI, review, or merge activity.
- Representing this independent lab as an official or GitHub-native fork of Open WebUI.
- Opening unsolicited upstream code PRs.
- Treating an issue remaining open as proof that its bug is still present or unclaimed.

## Research methodology and selection rules

The issue tracker, linked PRs, relevant source history, and release history were checked on 2026-10-01. The imported source is upstream `main` at `8bd8b4fac5e059578ac0c74b3c18d11139f88b7d`; upstream `dev` at `015dbc8619568ac077ec1d782661836a63691ba1` was separately checked for unreleased changes. All 15 retained issues were open at the snapshot. The dossiers link to primary reports and distinguish reporter claims, function-level/source harnesses, and full end-to-end reproduction.

Before an issue is accepted for implementation, investigators must:

1. Re-open the live issue and linked discussions; record state, date, and version context.
2. Inspect linked PRs, commits, release notes, and current `dev` changes.
3. Search for duplicates and determine whether a related issue has already resolved the behavior.
4. Reproduce on a current supported release and current `dev` when practical; label non-reproduction and environment limits. The 2026-10-01 comparison found no unreleased fixes for the retained behaviors; P07's dev-only bulk archive change does not fix the single-chat tag-name loss.
5. Inspect current source and relevant tests to establish the actual affected components and root cause.
6. Confirm the scope is technically understandable and useful to affected users.
7. Confirm an explicit upstream maintainer request before any upstream code PR.

Replace any candidate that is closed, fixed, duplicate, invalid, unsuitable, or already addressed. Preserve the audit trail and do not force the count by inventing a problem.

## Candidate backlog

The state shown is the public issue state checked on 2026-10-01. "Candidate" does not mean independently reproduced or source-validated. Each dossier records the remaining checks.

| ID | Upstream report | Candidate problem |
|---|---|---|
| P01 | [#31473](https://github.com/open-webui/open-webui/issues/31473) | Header Archive action toggles an already archived chat back to active |
| P02 | [#31471](https://github.com/open-webui/open-webui/issues/31471) | Search does not find a chat using edited reply text |
| P03 | [#31468](https://github.com/open-webui/open-webui/issues/31468) | Ctrl+Shift+Enter submits a message under Ctrl+Enter-to-Send |
| P04 | [#30235](https://github.com/open-webui/open-webui/issues/30235) | Memory context is sorted alphabetically, then character-sliced, potentially dropping or truncating entries |
| P05 | [#31321](https://github.com/open-webui/open-webui/issues/31321) | Deleting the active chat leaves it listed in its sidebar folder |
| P06 | [#31397](https://github.com/open-webui/open-webui/issues/31397) | Knowledge file mutations return a full listing, scaling response cost |
| P07 | [#30454](https://github.com/open-webui/open-webui/issues/30454) | Archiving a chat changes tag names on restoration |
| P08 | [#30422](https://github.com/open-webui/open-webui/issues/30422) | Auto-playback waits for the full response rather than streaming sentences |
| P09 | [#31571](https://github.com/open-webui/open-webui/issues/31571) | Docling extraction may run OCR despite an existing text layer |
| P10 | [#31407](https://github.com/open-webui/open-webui/issues/31407) | Deferred tools and tool-reference results need preservation in Anthropic conversion |
| P11 | [#30318](https://github.com/open-webui/open-webui/issues/30318) | Renaming a knowledge file leaves citations showing the old name |
| P12 | [#30362](https://github.com/open-webui/open-webui/issues/30362) | Moving conversations into folders should preserve chat recency |
| P13 | [#30239](https://github.com/open-webui/open-webui/issues/30239) | Memory injection should remain stable for prompt-cache reuse |
| P14 | [#31637](https://github.com/open-webui/open-webui/issues/31637) | Multi-model chats should expose all applicable action buttons |
| P15 | [#31609](https://github.com/open-webui/open-webui/issues/31609) | Selected routes should be configurable to skip OTEL metrics |

### Source validation and reproduction register

The linked dossier is authoritative for complete issue evidence, root-cause reasoning, proposed solution, acceptance criteria, regression plan, risks, and dependencies. This register summarizes the source trace and observed reproduction scope; a function-level or mocked harness is not represented as an end-to-end application test.

| ID | Source location and source-level finding | Reproduction evidence and limitation | Regression-test strategy |
|---|---|---|---|
| P01 | `Navbar/Menu.svelte` always labels the action Archive; `Chat.svelte` always toasts archived, while `routers/chats.py` toggles state. | Source-traced; no full UI/backend run. | Exercise archive and unarchive states, placement after reload, and truthful toast. |
| P02 | `Messages.svelte` clears `content` when editing structured `output`; `models/chats.py` search predicates search `content`, not `output`. | Actual SQLite predicate on an in-memory fixture returned no match for edited output and matched when `content` was populated. | Edit/save/reload/search, removal of old terms, title search, SQLite and PostgreSQL. |
| P03 | `MessageInput.svelte` handles Ctrl/Meta+Enter without excluding Shift; a separate layout shortcut handles Generate Message Pair. | Source predicate harness showed Ctrl+Shift+Enter satisfies the submit condition; reporter supplied a Playwright/mock-endpoint repro. DOM-level repro not run locally. | Assert no submit and exactly one pair for the chord; ordinary Ctrl/Meta+Enter still submits. |
| P04 | `utils/memory.py::add_memory_context` alphabetically orders sections and character-slices rendered context. | Actual function with synthetic records reproduced a 2,000-character limit with the last entry cut mid-entry; retrieval/database were mocked. | Preserve relevance, fit whole entries, define oversized-entry behavior, test limits and isolation. |
| P05 | Active deletion in `Chat.svelte` refreshes main/pinned lists but omits folder-list invalidation; folder state is separate in `chatList.ts`. | Source control-flow confirmed the missing refresh; no full UI/API deletion run. | Immediate folder-row removal after success, retention on failure, main/pinned lists, reload. |
| P06 | Knowledge mutation routes call `get_file_metadatas_by_id()` for the full linked collection. | Actual single-add route with mocked persistence/vector operations returned five synthetic file records; real database scaling not measured. | Single/batch add, update/remove, warnings, response size and query cost across collection sizes. |
| P07 | Single-chat archive deletes orphan tag rows; unarchive recreates them from normalized IDs via `ensure_tags_exist()`. | Actual ensure function with fake DB recreated `('my_project', 'my_project')`; no persistent archive round trip. | Spaced/mixed-case names, shared tags, single/bulk archive and restore, suggestions. |
| P08 | `Chat.svelte` triggers normal auto-playback in the completion branch; sentence splitting then occurs in `ResponseMessage.svelte`. | Source confirms completion-only playback; no browser/TTS end-to-end run. | Stream complete sentence chunks in order; cover final partial sentence, cancellation, and errors. |
| P09 | `DoclingLoader` forwards configured parameters to Docling Serve and has no per-document text-layer/OCR gate. | Actual loader with mocked HTTP forwarded `do_ocr=True`; no Docling server or PDFs to verify OCR execution, output, or timings. | Born-digital, scanned, mixed, empty, and explicit override cases against real/faithful Docling service. |
| P10 | `utils/anthropic.py` does not handle `tool_reference` and does not filter definitions by `defer_loading`. | Actual converter forwarded deferred definitions and converted a reference result to empty content; no provider request needed. | Deferred/un-deferred schemas, references, forced choices, prior calls, unrelated result content. |
| P11 | File rename updates stored filename but not indexed chunk metadata; retrieval citation metadata passes through. | Function-level retrieval repro returned the old filename after the file record was renamed; no live vector-store integration. | Rename indexed file; verify citations and sources update while old metadata does not leak. |
| P12 | `Chats.update_chat_folder_id_by_id_and_user_id` updates `updated_at` and `last_read_at`, which list ordering uses. | Actual method with fake session changed `updated_at` from `1750000000` to `1790840122`; no production DB run. | Move folders without changing recency; verify sorting, timestamps, ownership, and unrelated chat updates. |
| P13 | `add_memory_context` re-runs retrieval, strips/re-appends the system block, and has no per-conversation rendered-context cache. | Actual function preserved prompt bytes for identical inputs and changed them when memory content changed; provider cache hits/cost, real retrieval drift, and latency unmeasured. | Stable bytes for unchanged content; invalidation on add/update/delete; user isolation and provider telemetry. |
| P14 | `ResponseMessage.svelte` keeps actions on one horizontally scrollable no-wrap row and hides scrollbars within multi-model columns. | Issue supplies current-version steps and measurements; local browser reproduction unavailable. | Browser test multiple model columns/actions, visibility or accessible overflow, keyboard/touch access. |
| P15 | `utils/telemetry/metrics.py::setup_metrics` unconditionally records request count and duration in middleware `finally`. | Actual middleware with fake instruments recorded `/health` and `/health/db`; optional exporter/provider and live OTLP collector were not used. | Excluded/included routes, route-template/wildcard semantics, exceptions, status, and both instruments. |

The reported cache impact for P13 and Docling's OCR behavior for P09 are not independently quantified; P08 and P14 have no local browser end-to-end reproduction. These remain explicit validation boundaries, not inferred results.

## Product behavior and acceptance

The research gate succeeds when exactly 15 dossiers have dated primary evidence, current status, duplicate/fix-history checks, source-level findings, reproduction or an explicit limitation, proposed solution, acceptance criteria, and regression plan. Any separately authorized implementation additionally requires a focused regression test, relevant checks passing, a reviewed diff, and verified internal PR status. Upstream implementation remains gated on an explicit maintainer request.

## Architecture overview

The repository contains a pinned Open WebUI source snapshot at its root alongside the lab documentation. The frontend is the upstream Svelte/SvelteKit/Vite application under `src/`; the Python FastAPI application is under `backend/open_webui/`. Root `package.json`/`package-lock.json` and `pyproject.toml`/`uv.lock` define the frontend and backend environments. The lab's `problems/Pxx.md` dossiers and `docs/` workflow are separate from upstream application code. Provenance, source layout, and exact baseline command results are recorded in [`architecture.md`](architecture.md).

This PRD freezes the validated research snapshot, not authorization to change application behavior. Before any future implementation, refresh issue/PR/release/dev state and follow the sequential per-problem branch and review workflow. The imported snapshot is upstream `main` commit `8bd8b4fac5e059578ac0c74b3c18d11139f88b7d`; comparison source was upstream `dev` commit `015dbc8619568ac077ec1d782661836a63691ba1`. See [`architecture.md`](architecture.md) for provenance and exact baseline checks, and [`problems/`](../problems/) for each dossier.

## Testing strategy

For documentation, review link accuracy and factual claims. For an accepted code task, first add the smallest regression reproducer in the actual target source; run it against the unmodified behavior where practical, then after the fix. Run focused tests, the relevant broader suite, lint/typecheck/build where configured, and inspect accessibility/security/performance implications. The imported snapshot's `npm run test:frontend` command succeeds with no test files; its browser-regression workflow uses the separate `open-webui/tests` repository. Baseline lint and type-check failures are documented in [`architecture.md`](architecture.md).

## Delivery strategy

- **Branch:** one focused branch per accepted problem, processed sequentially and never implemented directly on `main`; use `fix/p01-short-description` through `fix/p15-short-description`.
- **Commits:** meaningful conventional commits; no activity-padding commits.
- **PR:** one focused internal lab PR per implemented problem, using the repository template. An upstream PR is a separate action and requires an explicit maintainer request.
- **CI/review:** inspect checks and review feedback; never bypass a failure or claim unobserved status.
- **Merge:** only when repository rules and required checks/reviews permit; verify the final GitHub state.

## Success metrics

- Exactly 15 source-validated issues at the 2026-10-01 snapshot, with no fabricated report.
- Every retained issue has a dated audit trail and a frozen dossier; invalid candidates have a recorded replacement reason.
- Every accepted fix has a regression test appropriate to the actual architecture.
- Every PR, check, review, and merge entry links to verifiable evidence.
- No upstream PR is made without the required maintainer request.

## Ethical contribution rules

This is an independent project. Respect upstream contribution policy, disclose uncertainty, credit primary sources, minimize scope, avoid contribution-graph padding, and never misrepresent an internal change as upstream acceptance.

## Research gate outcome

The research gate is complete for the 2026-10-01 snapshot. Issue state can change and must be revalidated before any implementation. No application code has been changed.
