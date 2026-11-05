# Installed third-party skills

The skills were installed with `npx skills@1.7.4 add`, with exact Git revisions and the Codex, Claude Code, Cursor, GitHub Copilot, and Gemini CLI targets.
This repository copies the installed files. It does not run the installer again.
[skills-lock.json](../skills-lock.json) records the upstream revisions and installer hashes.
The local adaptations are rewritten for the README generator.

## Inventory

| Skill                     | Source             | Purpose                                                               |
| ------------------------- | ------------------ | --------------------------------------------------------------------- |
| property-based-testing    | trailofbits/skills | Test invariants and generated input cases, for example in validators. |
| sharp-edges               | trailofbits/skills | Find unsafe interfaces and configuration defaults.                    |
| differential-review       | trailofbits/skills | Review changes for security regressions.                              |
| supply-chain-risk-auditor | trailofbits/skills | Examine dependency metadata and advisories when requested.            |
| agents-md                 | getsentry/skills   | Maintain concise project instruction files.                           |
| skill-scanner             | getsentry/skills   | Examine skill instructions and scripts before adoption.               |
| code-simplifier           | getsentry/skills   | Improve generator code readability while preserving behavior.         |
| doc-coauthoring           | anthropics/skills  | Develop substantial documents and examine reader understanding.       |
| find-bugs                 | getsentry/skills   | Find bugs and vulnerabilities in branch changes without edits.        |
| gha-security-review       | getsentry/skills   | Find exploitable GitHub Actions workflow vulnerabilities.             |
| fp-check                  | trailofbits/skills | Verify whether a suspected vulnerability is a true positive.          |
| second-opinion            | trailofbits/skills | Get an external review from another agent CLI on explicit request.    |
| grilling                  | mattpocock/skills  | Ask plan questions in rounds, each with a recommended answer.         |
| writing-for-agents        | mattpocock/skills  | Apply writing principles to agent instructions and skills.            |

Project requirements remain in [AGENTS.md](../AGENTS.md).
Installing a skill does not authorize its scripts, network requests, delegation, or changes to user data.
Use the host's applicable permission controls for those actions.
No installed skill lists Bash, Write, Edit, or network tools in `allowed-tools`.

## Storage and discovery

The common files are in `.agents/skills/<name>/`.
The installer reports this location for Codex, Cursor, GitHub Copilot, and Gemini CLI.
Claude Code uses relative directory links in `.claude/skills/<name>`.
All fourteen third-party links point inside this repository.
The project skill adapters in `.claude/skills/` are regular files.

Native host discovery depends on the installed host version and settings.
Use [the agent setup guide](AI_AGENTS.md) to examine a new session.

On Windows, Git must preserve symbolic links for the Claude entries.
If links are unavailable, use the installer's copy mode for the selected Claude skills.
Apply the local patches to those copies too.
Do not replace the root CLAUDE.md import.

## Revisions and local changes

[skills-lock.json](../skills-lock.json) records the upstream sources, Git revisions, and installer hashes.
The lockfile describes upstream versions. It does not preserve local adaptations.

| Source             | Git revision                               |
| ------------------ | ------------------------------------------ |
| trailofbits/skills | `2be6433b39e009cd2b6fd3be1895f6b1faa5af9a` |
| getsentry/skills   | `d12f250460ffd961f549dbebf7eeb0cf4afd4c2e` |
| anthropics/skills  | `ca45fc496a3436251f191354091d557da31c15b5` |
| mattpocock/skills  | `a496f5513d4a4cf889bc48e189e579f4b0bdf6aa` |

Most skills have a "Project integration" section before the upstream text. It refers to [AGENTS.md](../AGENTS.md) and keeps the safety limits:

- code-simplifier replaces the upstream text with generator invariants, limited refactoring scope, and verification requirements.
- skill-scanner, differential-review, supply-chain-risk-auditor, fp-check, gha-security-review, and second-opinion remove Bash, Write, or Edit from `allowed-tools`.
- find-bugs adds a checklist for the template, card data, files, subprocesses, and external effects.
- gha-security-review reports deviations from the stricter project Actions rules in a separate section.
- second-opinion permits an external provider call only on an explicit user request in the current task.
- supply-chain-risk-auditor permits the collector only on an explicit user request for a dependency audit.

The property-based-testing, sharp-edges, and doc-coauthoring skills have no local changes.

Each installed skill has LICENSE and UPSTREAM.md files.
Trail of Bits files retain CC-BY-SA-4.0 attribution.
Sentry files retain Apache-2.0 attribution.
Matt Pocock files retain MIT attribution.
The Anthropic README identifies the example skills as Apache-2.0.

## Limits

The dependency collector and its network requests were not executed.
Its source includes registry lookups and a GitHub token lookup through `gh auth token`.
Do not run it without an authorized dependency audit and an appropriate credential environment.

The second-opinion skill sends a patch and project instructions to OpenAI, Google, or another selected provider.
Installation does not authorize that transfer.
These limits are instructions, not access controls. Hosts that ignore `allowed-tools` keep their own permission rules.

## Updates

**CAUTION: Reinstallation can replace local adaptations and license additions.**

1. Save the current skill files before an update.
2. Review the selected source revision and the installer overwrite summary.
3. Update only the selected skills.
4. Apply the "Project integration" sections again.
5. Restore the relevant license and attribution files.
6. Repeat security, metadata, and link checks.
