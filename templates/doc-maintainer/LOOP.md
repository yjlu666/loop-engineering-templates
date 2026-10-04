# Doc Maintainer Loop

Configuration: [CONFIG.md](CONFIG.md)

## LOOP CONTRACT

### Goal

Keep the documents listed in [CONFIG.md](CONFIG.md) consistent with code merged into the upstream repository's base branch. Complete one incremental scan and produce one run report per round. If discrepancies are found, also submit one documentation update PR without merging it.

### Boundary

Work only with the upstream repository, personal fork, and documentation paths specified in [CONFIG.md](CONFIG.md), using existing GitHub credentials. Modify only the listed documents, not code or unlisted files. Do not merge any PR or modify the user's existing checkout. Execute one round per invocation. Reports under reports/ are for the user; the agent must not read them back.

### SOP

1. Read the configuration and [STATE.md](STATE.md). Consult [LOGS.md](LOGS.md) only when troubleshooting; do not read historical reports under reports/.
2. Use the GitHub API with pagination to list PRs merged into the upstream repository's base_branch. Select those with numbers greater than the cursor, in ascending merge-time order. If the cursor is empty or 0 (the first run), select only PRs merged in the last 24 hours. Record the highest PR number scanned.
3. If there are no new PRs, skip steps 4–5 and proceed to step 6.
4. Read each PR's code changes (the diff, plus related files if needed) and compare them with every configured document. List discrepancies: new behavior not covered by the documentation, outdated descriptions, and examples that contradict the implementation.
5. If discrepancies exist, fetch the latest upstream base_branch commit in the cached clone and create a new worktree and branch named `docs/sync-<date>` from that commit. Make minimal documentation changes in the worktree, commit and push the branch to fork_repo, and submit one PR to the upstream repository. The title should describe the documentation sync; the body should list the source PR numbers and changed documents. Do not merge it. If there are no discrepancies, create no branch or PR.
6. Write the round's results to `reports/<timestamp>.md`: the range of PR numbers scanned, the comparison findings for each document, and the PR link. Save the highest merged PR number scanned in this round in STATE.
7. Remove the worktree after use. Keep clones under loop-engineering-templates/ in the system's user cache directory, separated by repository, for reuse in later rounds.

## STATE + LOG

### State

[STATE.md](STATE.md) stores only the last processed merged PR number in the `last_merged_pr` field. An empty state indicates the first run. Reports under reports/ are for the user; the agent must not read them back.

### Logs

[LOGS.md](LOGS.md) records only problems actually encountered and their solutions.
