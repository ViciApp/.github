# Testing

There is no test suite: the code is two workflow files. A change is proven in three layers.

1. **Static checks**, locally, before the pull request: `actionlint` (syntax, expressions, shell via shellcheck) and `zizmor .github/workflows` (permissions, pinning, template injection). Both must come out clean.
2. **Script logic**, locally: copy the `run:` block into a throwaway shell or a scratch git repository and run it on the cases that matter, a passing one and a failing one at least. For example, the title pattern against a good and a bad title, or `git diff --quiet HEAD^1 HEAD` against a real commit and an empty one.
3. **The real run.** `pr-checks.yml` here calls the local copy of `pull-request.yml`, so the pull request that changes a rule runs the changed rule on itself. Read that run, not just its colour: a job that should have run but was `skipped` also turns `pass` green.

What a pull request here cannot exercise, say in its body: the `merge_group` path until the entry is queued, and how the change behaves in the other repositories, whose callers pick it up only once it is on `main`. For a risky change, point one caller at the branch in a throwaway pull request (`uses: ViciApp/.github/.github/workflows/pull-request.yml@<branch>`) and close it unmerged.
