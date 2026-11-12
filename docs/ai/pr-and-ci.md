# Pull requests and CI

Each pull request has one purpose and starts from `main`, in parallel with the others; stack one only when it needs another's code (rules [2](https://github.com/AntonioVentilii/officina/blob/main/standards/code.md#2-scoped) and [3](https://github.com/AntonioVentilii/officina/blob/main/standards/code.md#3-parallel-not-stacked) of the code standard).

## Title

Pull requests are squash-merged, so the title becomes the commit on `main`. CI requires a Conventional Commit with a scope, the pattern at `.github/workflows/pull-request.yml:27`. Scopes so far: `pr-checks` for the shared checks and `github` for `CODEOWNERS` and other `.github/` files; use `profile` for the org profile and `ai` for these docs. The description is lowercase and says what changes for the reader, as in the history: `ci(pr-checks): add the org-wide pull request checks`.

## Body

Motivation, the changes in a few bullets, and how it was tested, including what could not be (the merge queue path, the other repositories). A change to a shared check lists the repositories it affects. No AI attribution lines, and no line that starts with `Test:` or `Fix:`.

## Local gates

For a change to a workflow:

```bash
actionlint
zizmor .
```

A Markdown-only change has no local gate.

## CI

`.github/workflows/pr-checks.yml` runs this repository's own copy of the shared checks on every pull request and queue entry; `pr-checks / pass` is the status the org `baseline` ruleset requires. Merges go through the merge queue (squash), where `not-empty` runs again against whatever is queued ahead. `.github/workflows/lint.yml` runs actionlint and zizmor on every pull request and queue entry; it is not a required status, so read it before merging.
