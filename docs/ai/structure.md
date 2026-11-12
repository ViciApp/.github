# Structure

The repository is small and every folder has a meaning GitHub gives it, so a new file almost always belongs in one of these.

| Path                     | What lives there                                                                                   | Who reads it                                                 |
| ------------------------ | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `profile/README.md`      | The org profile                                                                                    | GitHub, on github.com/ViciApp                                |
| `.github/workflows/`     | `pull-request.yml` (reusable, `workflow_call` only) and `pr-checks.yml` (this repository's caller) | Every ViciApp repository's `pr-checks.yml`, and Actions here |
| `.github/CODEOWNERS`     | Review ownership                                                                                   | GitHub, for this repository only                             |
| `docs/ai/`               | These agent docs                                                                                   | Agents and people changing this repository                   |
| `AGENTS.md`, `CLAUDE.md` | The entry point and its Claude Code pointer                                                        | Agents                                                       |

## Where new things go

- **A new shared pull request rule**: a new job in `.github/workflows/pull-request.yml`, added to the `needs` of `pass`. Not a new workflow file: callers only call this one, and a second one would mean editing every repository.
- **A new reusable workflow** for something other than pull request checks (a release step, say): a new file in `.github/workflows/` with `on: workflow_call`. Ask first ([governance](./governance.md#without-asking-and-not)).
- **A community health default** for every ViciApp repository: the root or `.github/`, under the name GitHub looks for. Ask first: it changes every repository without one.
- **Anything about one product repository**: that repository, not here.

## Direction

Product repositories depend on this one, never the other way round: nothing here names a specific product repository, except the profile's table of repositories. `pull-request.yml` reads only what every caller has by convention (the event payload, the `release-please.yml` workflow name and the `release-please--*` branch prefix), so a repository that keeps those conventions works without a change here.
