# GitHub Maintainer Loop

## LOOP CONTRACT

### Goal

Handle all new issues and PRs and those requiring follow-up in this round, produce one summary, and send one summary message to the specified chat.

### Boundary

Work only with the repositories and notification destination specified in [CONFIG.md](CONFIG.md). Follow [POLICY.md](POLICY.md) and the repositories' existing approval rules. Do not implement issues, automatically close items, or modify the user's existing checkout. Execute one round per invocation. Reports under reports/ are for the user; the agent must not read them back.

### SOP

1. Read the configuration, [STATE.md](STATE.md), and [TRACKING.md](TRACKING.md). Consult [LOGS.md](LOGS.md) only when troubleshooting; do not read historical reports under reports/.
2. Assign one subagent per repository to inspect repositories concurrently, providing the repository URL and the absolute path to an isolated working directory. Specify the directory explicitly in tool calls. If subagents are unavailable, process repositories sequentially.
3. Scan issues and PRs through the GitHub API with pagination. Without a cursor, process only items created in the last 24 hours; afterward, process items with numbers greater than the cursor. Use [TRACKING.md](TRACKING.md) as the basis for follow-up, checking and continuing work on each unresolved item. Remove an item from the list when its issue is closed or its PR is closed or merged.
4. Read diffs and related files first; clone or fetch only when full source is needed. Store source under loop-engineering-templates/ in the system's user cache directory, separated by repository, and update the cache directory's timestamp after use. Analyze PRs in temporary worktrees at the corresponding commits. Remove worktrees after use and retain clones for later rounds.
5. Wait for all repository analyses to finish. The main agent then handles replies and merges according to POLICY.
6. The main agent saves the highest number scanned in each repository during this round in STATE. Reclaim caches idle longer than cache_ttl_days. If total size exceeds max_cache_gib, reclaim the least recently used caches first.
7. Write the round's results, item links, and tracking-list changes to `reports/<timestamp>.md` for the user to read, without reading it back, and update TRACKING.md. The main agent sends one summary covering all repositories to the specified chat through Computer Use.

## STATE + LOG

### State

[STATE.md](STATE.md) stores only a JSON mapping from repository to last scanned number. Issues and PRs share the same numbering sequence; record 0 for an empty repository. [TRACKING.md](TRACKING.md) stores unresolved items and each item's next-round action, and is the sole source of follow-up state.

### Logs

[LOGS.md](LOGS.md) records only problems actually encountered and their solutions.
