# Change or add a shared check

The rules every ViciApp pull request must pass. Merging here changes them everywhere at once.

1. Ask the owner first when the change adds, removes or loosens a check, or changes what one accepts ([governance](../governance.md#without-asking-and-not)).
2. Edit `.github/workflows/pull-request.yml`. A new rule is a new job, following [patterns.md](../patterns.md): its own least `permissions`, pinned actions, untrusted values through `env:`, an `::error::` that tells the author what to do.
3. Add the new job to the `needs` list of `pass`. If it only makes sense for one event, give it a job-level `if:` so it ends `skipped` elsewhere.
4. If the job needs a permission the callers do not grant (`actions: read`, `contents: read`, `pull-requests: read` today), stop: every repository's `pr-checks.yml` has to grant it first, in pull requests that land before this one.
5. Run `actionlint` and `zizmor .github/workflows`, and the script on a passing and a failing case ([testing.md](../testing.md)).
6. Update [reusability.md](../reusability.md) (the job table) and the layout in `AGENTS.md` in the same pull request.
7. Open the pull request with a `ci(pr-checks): ...` or `fix(pr-checks): ...` title. Read the run here, then say in the body what it could not exercise and which repositories it affects.
