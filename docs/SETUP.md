# Setup guide

Do these steps one time after the first push. They need a person with administrator access to the repository on GitHub.

## 1. Let the workflow push to master

1. Open **Settings → Actions → General**.
2. In **Actions permissions**, make sure that actions are allowed.
   If you allow only selected actions, allow `actions/checkout`, `actions/setup-node`, and `github/codeql-action`.
3. In **Workflow permissions**, keep **Read repository contents and packages permissions**.
   The Daily README workflow asks for `contents: write` itself. The other workflows stay read-only.
4. Click **Save**.

If the repository is in an organization, the organization settings can block write access. Then allow it in the organization settings.

## 2. Check branch protection

The workflow pushes a commit directly to `master` with the `GITHUB_TOKEN`.

- If `master` has no branch protection rule or ruleset, do nothing.
- If `master` requires pull requests or status checks, the push fails. Open **Settings → Rules → Rulesets** (or **Settings → Branches**). Add **GitHub Actions** to the bypass list of the rule. If the rule type does not permit this bypass, remove `master` from the rule.

## 3. Make a test run

1. Open **Actions → Daily README**.
2. Click **Run workflow** and select `master`.
3. Set `dry_run` to on. Click **Run workflow**.
4. Open the run. The **Show the result** step shows the selected card. Nothing is pushed.
5. Run the workflow again with `dry_run` off. The run pushes a new `README.md` to `master`.
6. Open the repository page and examine the card.

## 4. Optional settings

- **CodeQL**: open **Settings → Advanced Security** and make sure that CodeQL **default setup** is off. The CodeQL workflow does the analysis.
- **Dependabot security updates**: enable them in **Settings → Advanced Security**.
- **Local Git hooks**: run `npm ci --ignore-scripts` and `npm run hooks:install`. See [Git hooks](GIT_HOOKS.md).

## Schedule notes

- The schedule is `0 11 * * *` with `timezone: Europe/Amsterdam`. GitHub changes the UTC time for daylight saving time.
- GitHub can start a scheduled run some minutes late when it has a high load. The run uses the date in Amsterdam when it starts.
- Scheduled workflows run only from the default branch. Keep `master` as the default branch.
- GitHub disables a schedule after 60 days without repository activity. The daily commits are activity, so this does not occur while the workflow works. If the schedule becomes disabled, open **Actions → Daily README** and click **Enable workflow**.
- Commits made with `GITHUB_TOKEN` do not start other workflows. Thus the daily commit does not start Tests or CodeQL.
