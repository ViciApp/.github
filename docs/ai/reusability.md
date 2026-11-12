# Reusability

What already exists, so nobody writes it twice. Check here, and in `.github/workflows/pull-request.yml`, before adding a check to any ViciApp repository: a rule every repository shares belongs in this file, once.

## Reusable workflow

`.github/workflows/pull-request.yml`, called as `ViciApp/.github/.github/workflows/pull-request.yml@main`. It takes no inputs and no secrets.

| Job             | Runs on                               | What it enforces                                                                                                                                                                                                                                                               |
| --------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `title`         | `pull_request` (no-op otherwise)      | The title is a Conventional Commit with a scope: `type(scope): description`, or `type(scope)!: description` for a breaking change. The type list and the scope characters are the `pattern` at `.github/workflows/pull-request.yml:27`, the one copy of that rule for the org. |
| `not-empty`     | both events                           | `HEAD^1..HEAD` changes at least one file: a pull request that changes nothing, or a queue entry that an earlier entry made redundant, fails.                                                                                                                                   |
| `release-fresh` | `merge_group` only                    | A `release-please--*` pull request merges only when nothing is queued ahead of it and the caller's `release-please.yml` run on the current base tip finished green.                                                                                                            |
| `pass`          | after the others, even when one fails | The aggregator the ruleset requires as `pr-checks / pass`: green only when every job above ended `success` or `skipped`.                                                                                                                                                       |

## Caller

`.github/workflows/pr-checks.yml` is the template every repository copies, with `uses:` pointing at `ViciApp/.github/.github/workflows/pull-request.yml@main` instead of the local path. The parts that must not drift: the name `PR Checks`, the triggers (`pull_request` with `opened, edited, reopened, synchronize`, and `merge_group`), `permissions: {}` at the top, the concurrency group, the job id `pr-checks` and its permissions (`actions: read`, `contents: read`, `pull-requests: read`). See [workflows/add-a-repository.md](./workflows/add-a-repository.md).

## Conventions callers rely on

`release-fresh` assumes a caller releases with release-please from `.github/workflows/release-please.yml` and that its release branches start with `release-please--`. A repository that releases differently is simply never held by it.
