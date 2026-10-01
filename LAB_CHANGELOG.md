# Changelog

All notable changes to this lab are documented here.

## Unreleased

### Implemented fixes

- P01: made the chat archive menu label and success toast follow the resulting archive state. Internal PR #3 remains open.
- P02: preserved searchable text for edited structured replies by materializing visible structured output into empty legacy content fields. Internal PR #4 remains open.
- P03: prevented Ctrl/Meta+Shift+Enter from also triggering normal submission when Ctrl/Meta+Enter-to-Send is enabled. Internal PR #5 remains open.
- P05: refreshed registered folder chat lists after successful deletion of the active chat. Internal PR #6 remains open.

### Research-only and unresolved

- The baseline source import is internal PR #2 and remains open because its required `Format & Build` check fails on the documented upstream translation-preparation mutation.
- P04 and P06-P15 were not implemented. P08, P09, P13, and P14 remain blocked by unavailable browser, Docling, provider/cache, or related validation infrastructure.
- No lab PR was merged and no change was accepted upstream. Frontend lint/typecheck, browser coverage, backend test coverage, and other baseline limitations remain documented in `docs/architecture.md`.
