# Wire a new ViciApp repository to the shared checks

The org `baseline` ruleset requires `pr-checks / pass` on every repository's default branch, so a repository without the caller cannot merge anything. The change happens in that repository, not here.

1. Copy `.github/workflows/pr-checks.yml` from this repository into the new one, and change `uses:` to `ViciApp/.github/.github/workflows/pull-request.yml@main`. Change nothing else: the job id `pr-checks`, the triggers, the concurrency group and the permissions are the contract ([reusability.md](../reusability.md#caller)).
2. Rewrite the header comment for the caller side: the rules live in `ViciApp/.github`, and the ruleset requires the status this job reports (the existing callers, such as vici-app's, say exactly that).
3. If the repository releases with release-please, keep its workflow at `.github/workflows/release-please.yml` so `release-fresh` can find the run.
4. Open the pull request there (`ci(pr-checks): call the org-wide pull request checks`); it is the first one to run the checks.
5. If the repository is public, add it to the table in `profile/README.md` here, in its own pull request ([edit-the-profile.md](./edit-the-profile.md)).
