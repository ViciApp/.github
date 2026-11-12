# AGENTS.md

Entry point for every AI agent working on this repository. Read it first.

## What this is

`ViciApp/.github`, the organization's special repository. It holds two things:

- **The org profile**: `profile/README.md` is what github.com/ViciApp shows to visitors.
- **The shared pull request checks**: `.github/workflows/pull-request.yml` is a reusable workflow that every ViciApp repository calls from its own `.github/workflows/pr-checks.yml`. The org `baseline` ruleset requires the `pr-checks / pass` status it produces on every repository's default branch.

Callers pin `@main`, so a change merged here applies to every ViciApp pull request at once, with no release in between. A broken rule here blocks merging everywhere.

## Commands

There is no package manifest and no build. The local gates for a workflow change:

- `actionlint`: syntax and expression checks for the workflows
- `zizmor .github/workflows`: security audit (permissions, pinning, injection)

Nothing to run for a change that only touches Markdown.

## Layout

- `.github/workflows/pull-request.yml`: the shared rules, triggered only by `workflow_call`. Jobs `title`, `not-empty`, `release-fresh` (merge queue only) and the aggregator `pass`.
- `.github/workflows/pr-checks.yml`: this repository's own caller. It runs the local copy (`uses: ./.github/workflows/pull-request.yml`), so a change to the rules is tested on the pull request that makes it.
- `.github/CODEOWNERS`: one owner for every path.
- `profile/README.md`: the public org profile.
- `docs/ai/`: these agent docs.

## Where to look

| You are about to                                   | Read first                                                                                   |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Touch a workflow, `CODEOWNERS` or the org profile  | [`docs/ai/governance.md`](./docs/ai/governance.md)                                           |
| Add a file or wonder where something goes          | [`docs/ai/structure.md`](./docs/ai/structure.md)                                             |
| Write workflow YAML or a `run:` script             | [`docs/ai/patterns.md`](./docs/ai/patterns.md)                                               |
| Add a job or a check                               | [`docs/ai/reusability.md`](./docs/ai/reusability.md)                                         |
| Prove a workflow change works                      | [`docs/ai/testing.md`](./docs/ai/testing.md)                                                 |
| Open a pull request                                | [`docs/ai/pr-and-ci.md`](./docs/ai/pr-and-ci.md)                                             |
| Change or add a shared check                       | [`docs/ai/workflows/change-a-shared-check.md`](./docs/ai/workflows/change-a-shared-check.md) |
| Wire a new ViciApp repository to the shared checks | [`docs/ai/workflows/add-a-repository.md`](./docs/ai/workflows/add-a-repository.md)           |
| Edit the org profile                               | [`docs/ai/workflows/edit-the-profile.md`](./docs/ai/workflows/edit-the-profile.md)           |

**Truth hierarchy:** the code, then CI, then `CODEOWNERS`, then `docs/ai/**`, then this file, then tool layers such as `CLAUDE.md`; the [code standard](https://github.com/AntonioVentilii/officina/blob/main/standards/code.md#truth-hierarchy) says what to do on a contradiction. **Meta-update rule:** a change that adds a pattern, a shared piece, a convention or a workflow updates the matching page in the same pull request; [`docs/ai/governance.md`](./docs/ai/governance.md#meta-update-map) says which.

## Rules

Every change follows the universal [code standard](https://github.com/AntonioVentilii/officina/blob/main/standards/code.md). What follows only adds what is specific to this repository.

- **Every caller runs `main`.** Treat a merge to `main` as a deploy to every ViciApp repository. A change must keep passing the pull requests that are open across the org today, and it lands through the merge queue like everything else.
- **The required status is a contract.** `pr-checks / pass` is the caller's job id (`pr-checks`) and this workflow's job id (`pass`). Renaming either breaks the `baseline` ruleset for every repository.
- **Permissions only go down.** A called workflow cannot hold more permissions than its caller grants. A job that needs a new permission means changing every repository's `pr-checks.yml` first.
- **Never weaken a check or the `pass` gate** without the owner asking for it.
