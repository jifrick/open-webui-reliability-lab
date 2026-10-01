# Engineering workflow

This workflow is intentionally gated: a dossier is not implementation authorization.

## Per-problem sequence

1. Re-read `problems/Pxx.md` and refresh the live issue state.
2. Check discussions, linked PRs/commits, duplicates, and recent releases/dev changes.
3. Inspect the actual upstream source and tests; write the source map and root-cause evidence.
4. Reproduce the behavior on a current version where practical. Record environment and limitations.
5. Verify that the issue remains valid and that a maintainer has explicitly requested any upstream code PR.
6. Define a narrow acceptance test; add or run it before/alongside a change in the actual source repository.
7. Implement the smallest correct change; avoid unrelated refactoring.
8. Run targeted tests, broader relevant tests, and configured lint/typecheck/build checks.
9. Inspect the complete diff for regressions, security, accessibility, and performance.
10. Record commit and internal PR details only after verifying them.
11. Address review/CI feedback and re-run affected checks.
12. Merge only when rules permit and verify the final state.
13. Update the problem dossier, changelog when appropriate, and contribution log.

## Stop/replace criteria

Stop work on a candidate if it is fixed, closed without a remaining valid behavior, duplicate, invalid, unrepeatable without a technically justified alternative, or already covered by an active upstream fix. Replace it through fresh issue-tracker research and preserve why the former candidate was removed.

## Branch, commit, and PR discipline

Each actual fix should be one focused branch and internal PR, processed sequentially; never implement directly on `main`. Name branches `fix/p01-short-description` through `fix/p15-short-description` using the matching problem ID. Use conventional commit titles and do not create empty commits or PRs solely to satisfy a count. The internal PR template includes Problem, Evidence, Root Cause, Solution, Tests, Verification, Scope, Problem ID, and Breaking Changes.

## Upstream boundary

An internal lab PR is not an upstream Open WebUI PR. No upstream code PR may be opened without an explicit maintainer request. Re-check upstream policy before any external submission.

## Status reporting

Use only observed values: link the exact issue, commit, PR, CI, review, and merge evidence. Record unknown or blocked states as unknown/blocked, never as success.
