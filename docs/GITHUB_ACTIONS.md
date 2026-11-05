# GitHub Actions

The repository has four GitHub Actions workflows and a Dependabot configuration.
See [the setup guide](SETUP.md) for the one-time steps in the repository settings.

| Workflow                                              | Start                                                 | Result                                                                       |
| ----------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------------------------------------- |
| [Daily README](../.github/workflows/daily-readme.yml) | Each day at 11:00 in Amsterdam, manual start.         | Tests the generator, writes a new `README.md`, and pushes it to `master`.    |
| [Tests](../.github/workflows/test.yml)                | Pull request, push to `master`, manual start.         | Formatting, lint, type checks, tests, and a dry run of the generator.        |
| [Secrets](../.github/workflows/secrets.yml)           | Pull request, push to `master`, manual start.         | Lefthook validation and Gitleaks scans of the history and the working files. |
| [CodeQL](../.github/workflows/codeql.yml)             | Pull request, push to `master`, weekly, manual start. | Static security analysis of JavaScript, TypeScript, and the workflows.       |

## Daily README

### Schedule

```yaml
schedule:
  - cron: '0 11 * * *'
    timezone: Europe/Amsterdam
```

GitHub evaluates the cron expression in the given IANA time zone. Thus the run stays at 11:00 local time in summer and in winter.
GitHub can start a scheduled run some minutes late. Scheduled runs start only from the default branch.

### Manual start

1. Open **Actions → Daily README**.
2. Click **Run workflow** and select `master`.
3. Set `dry_run`. A dry run generates `README.md` and shows the card in the log. It does not commit or push.
4. Click **Run workflow**.

### Steps

1. Check out `master` without stored credentials.
2. Install the locked dependencies with `npm ci --ignore-scripts`.
3. Run `npm run typecheck` and `npm test`. A failure stops the run before `README.md` changes.
4. Run `npm run readme`.
5. If `README.md` changed and the run is not a dry run, commit only `README.md` as `github-actions[bot]` and push it to `master`.

The push fails when `master` received another commit during the run. Start the workflow again in this case.

## Security controls

| Control              | Implementation                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Token permissions    | The default is `contents: read`. Only the Daily README `update` job has `contents: write`. Only the CodeQL `analyze` job has `security-events: write`. |
| Checkout credentials | All jobs use `persist-credentials: false`. The Daily README job gives the token to the push command only, through a one-time Git option.               |
| Pushed files         | The Daily README job stages only `README.md`.                                                                                                          |
| Actions              | Each action is pinned to a full commit SHA with the version in a comment.                                                                              |
| Trigger events       | No `pull_request_target`, `workflow_run`, `issue_comment`, pull request review, or discussion events.                                                  |
| Input handling       | Inputs go to scripts through environment variables, not through `${{ }}` in shell text.                                                                |
| Dependency scripts   | `npm ci` uses `--ignore-scripts`, and `.npmrc` sets `ignore-scripts=true`. No dependency cache is used.                                                |

Limits:

- The Daily README job runs the code and the dependencies on `master` with a write token. A change on `master` is trusted. Review changes before they reach `master`.
- Commits made with `GITHUB_TOKEN` do not start other workflows. The daily commit does not start Tests, Secrets, or CodeQL.
- GitHub-hosted runners, the pinned actions, and the locked development dependencies are trusted. The `min-release-age` setting in `.npmrc` delays new dependency versions.

### Manual version pins

Dependabot updates only the actions in `uses:` and the npm packages. Update these pins manually:

| Pin                     | Location                                                        | Check                   |
| ----------------------- | --------------------------------------------------------------- | ----------------------- |
| Node.js for the CI jobs | `.nvmrc` and `engines.node` in `package.json`                   | Exact version `X.Y.Z`.  |
| Gitleaks                | Download URL in [secrets.yml](../.github/workflows/secrets.yml) | SHA-256 of the archive. |

Select a Node.js version that is at least 7 days old, as `min-release-age` in `.npmrc` requires for npm packages. A security release can be used earlier.

## CodeQL

The CodeQL workflow analyzes TypeScript, JavaScript, and the GitHub Actions workflows with the `security-extended` query suite and the [configuration file](../.github/codeql/codeql-config.yml).
It uses `build-mode: none`. Do not enable CodeQL default setup together with this workflow. GitHub rejects results from both.
A human decides how to handle each alert. Do not dismiss an alert without a recorded reason.

## Dependabot

The [Dependabot configuration](../.github/dependabot.yml) makes version update pull requests one time each week, on Monday.

- A new version must be 7 days old before Dependabot proposes it. A major npm version must be 14 days old.
- Security updates do not wait. Enable them in **Settings → Advanced Security → Dependabot security updates**.
- A human examines each Dependabot pull request and merges it.
