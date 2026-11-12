# Governance

## Paths with an owner

`.github/CODEOWNERS` gives every path to one owner (`* @AntonioVentilii`). The org `baseline` ruleset asks for code owner review, resolved review threads and a Copilot review on every pull request to `main`, and allows squash merges only. This repository's own `merge-queue` ruleset puts every merge through the queue and requires the `workflows` check from `lint.yml`.

| Path                                               | Rule                                                                                                                                                                                                                                           |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.github/workflows/pull-request.yml`               | Runs on every ViciApp pull request the moment it merges. Ask before changing; never weaken a check, the `pass` gate or its `needs` list. Never rename the `pass` job.                                                                          |
| `.github/workflows/pr-checks.yml`                  | This repository's own caller. Keep it in step with the callers in the other repositories (same triggers, same permissions); its job id `pr-checks` is half of the required status name.                                                        |
| `.github/workflows/lint.yml`, `.github/zizmor.yml` | The workflow lint for this repository only. Never rename the `workflows` job: it is the required status name. Bump a pinned version freely when both tools pass; ask before ignoring a finding, and give every ignore its reason in a comment. |
| `.github/CODEOWNERS`                               | Decides who reviews what. Ask before changing.                                                                                                                                                                                                 |
| `profile/README.md`                                | Public: it is the org's front page on GitHub. Ask before changing what it says about the product.                                                                                                                                              |
| The rulesets                                       | Live in the org and repository settings, not in this repository. Changing them (required checks, merge queue, merge methods) is the owner's call, never part of a pull request here.                                                           |

## Without asking, and not

An agent may, without asking: fix a typo or a broken link in the profile, update these pages, add or sharpen a comment that says why, and bump a pinned action to a newer release of the same action (new SHA and version comment) when the gates pass.

An agent asks first before it: adds, removes or loosens a check in `pull-request.yml`; changes what a check accepts (the title pattern, what counts as empty, what makes a release pull request stale); asks callers for a new permission; adds a new reusable workflow other repositories will call; adds a community health file (`CONTRIBUTING.md`, `SECURITY.md`, issue or pull request templates), which GitHub would then apply as the default to every ViciApp repository without one; or changes `CODEOWNERS`.

## Meta-update map

A change that adds or changes one of these updates the matching page in the same pull request ([rule 13](https://github.com/AntonioVentilii/officina/blob/main/standards/code.md#13-docs-adapt-with-the-code)):

| You changed                                                      | Update                                                                              |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| A job in `pull-request.yml`, or what a check accepts             | [reusability.md](./reusability.md), and the layout in `AGENTS.md` for a new job     |
| The caller contract (triggers, permissions, job id, `@main` pin) | `AGENTS.md`, [workflows/add-a-repository.md](./workflows/add-a-repository.md)       |
| A new file or folder                                             | [structure.md](./structure.md) and the layout in `AGENTS.md`                        |
| A new way of writing workflow YAML or scripts                    | [patterns.md](./patterns.md)                                                        |
| A local gate or the pull request convention                      | [pr-and-ci.md](./pr-and-ci.md), [testing.md](./testing.md), commands in `AGENTS.md` |
| `CODEOWNERS` or who may change what                              | this page                                                                           |
| A recurring kind of change                                       | a page in [workflows/](./workflows)                                                 |
