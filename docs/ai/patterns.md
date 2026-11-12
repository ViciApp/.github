# Patterns

The patterns the workflows really use. Follow them in every new job and step.

## Workflows

- **One aggregated status.** The ruleset requires one context, `pr-checks / pass`. The `pass` job needs every other job and fails unless each one ended `success` or `skipped` (`.github/workflows/pull-request.yml:110`). A new job counts only once it is in that `needs` list.
- **Skip by job or by step, on purpose.** A job that only makes sense in one event uses a job-level `if:` and is then `skipped`, which `pass` accepts (`release-fresh`, `.github/workflows/pull-request.yml:66`). A job that must always report runs and skips its work at step level instead, printing why (`title`, `.github/workflows/pull-request.yml:17`).
- **Least privilege, per job.** `permissions: {}` at the top of both files, then each job asks only for what it uses (`.github/workflows/pull-request.yml:9`, `:15`, `:42`, `:68`). The caller grants the union (`.github/workflows/pr-checks.yml:19`).
- **Actions pinned by full commit SHA**, with the release as a comment: `actions/checkout@<sha> # v7.0.1` (`.github/workflows/pull-request.yml:46`). Checkout never keeps credentials (`persist-credentials: false`, `:49`).
- **Pinned runner image**: `ubuntu-24.04`, not `ubuntu-latest`.
- **Both events.** Callers trigger on `pull_request` (`opened`, `edited`, `reopened`, `synchronize`, so a title edit reruns the title check) and `merge_group`; every job must behave in both. The concurrency group falls back to `github.run_id` when there is no pull request number (`.github/workflows/pr-checks.yml:13`).

## Scripts in `run:`

- **Untrusted values go through `env:`**, never `${{ }}` inside the script: the pull request title arrives as `PR_TITLE` (`.github/workflows/pull-request.yml:23`), the queue refs as `HEAD_REF`, `HEAD_SHA`, `BASE_REF` (`:76`).
- **Never print untrusted text at the start of a line**, where it could pass for a `::workflow-command::` (`.github/workflows/pull-request.yml:34`).
- **Fail with `::error::`** and a sentence that tells the author what to do, then `exit 1`; print the evidence on a plain line after it (`.github/workflows/pull-request.yml:97`, `:104`).
- **Read GitHub through `gh api`** with `GH_TOKEN: ${{ github.token }}` and `--jq`, not `curl` (`.github/workflows/pull-request.yml:87`).
- **Comments say why**, above the step or the line: each workflow file opens with what it is for and who calls it.

## Anti-patterns

None in the code today. Do not introduce: a check that callers must copy into their own workflows (it belongs here), an `always()` aggregator that does not inspect `needs.*.result`, or an action referenced by tag or branch.
