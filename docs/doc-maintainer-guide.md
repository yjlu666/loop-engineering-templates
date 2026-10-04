# Doc Maintainer Loop Guide

[Simplified Chinese](zh-CN/doc-maintainer-guide.md)

## Get Started in Three Steps

1. Copy the entire `templates/doc-maintainer/` directory to your working location.
2. Fill in the upstream repository, your fork, and the documentation paths to maintain in your copy of `CONFIG.md`.
3. Tell your local agent: `Read <instance-directory>/LOOP.md and execute one round.` Use the same file next time to continue from the saved progress.

## Configuration

| Field | Default | How to Fill It In |
|---|---|---|
| `upstream_repo` | `""` | Required. The upstream repository URL; scan PRs merged into its base branch and submit PRs to it |
| `fork_repo` | `""` | Required. Your fork's repository URL, used to push documentation update branches |
| `base_branch` | `"main"` | The base branch name; the default can be kept |
| `documents` | `[]` | Required. A list of documentation paths in the upstream repository, such as `["README.md", "docs/api.md"]` |

## Execution

The first run (when `STATE.md` is empty) scans only PRs merged into the base branch in the last 24 hours. Subsequent rounds continue after `last_merged_pr` in `STATE.md`, processing only merged PRs with higher numbers. The agent compares each PR's code changes against every document in `documents`:

- If discrepancies are found: create a worktree and branch from the latest commit on the upstream base branch, make minimal documentation changes, push to `fork_repo`, and submit one PR to the upstream repository without merging it.
- If no discrepancies are found: create no branch or PR; only record the round's results.

Each round's results are written to `reports/<timestamp>.md` in the instance directory for you to read; the agent does not read these reports back. The highest PR number scanned in the round is then saved in `STATE.md`. Clones are cached under `loop-engineering-templates/` in the system's user cache directory, and worktrees are removed after use.

For scheduled runs, configure your local agent to invoke the same `LOOP.md`.
