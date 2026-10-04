# GitHub Maintainer Loop Guide

[Simplified Chinese](zh-CN/github-maintainer-guide.md)

## Get Started in Three Steps

1. Copy the entire `templates/github-maintainer/` directory to your working location.
2. Fill in the repository list, notification app, and recipient chat in your copy of `CONFIG.md`. You can keep the default cache settings.
3. Tell your local agent: `Read <instance-directory>/LOOP.md and execute one round.` Use the same file next time to continue from the saved progress.

## Configuration

| Field | Default | How to Fill It In |
|---|---|---|
| `repositories` | `[]` | Required. One or more GitHub repository URLs |
| `notify_app` | `""` | Required. The name of a local messaging app, such as Feishu or WeChat |
| `notify_chat` | `""` | Required. The exact name of the chat that receives the summary |
| `cache_ttl_days` | `7` | The number of days a source cache can remain idle; the default can be kept |
| `max_cache_gib` | `20` | The target total size of source caches across all repositories, in GiB; the default can be kept |

The first run includes only issues and PRs created in the last 24 hours. Later runs discover new items incrementally and follow up on tracked items. The agent automatically identifies repository rules and cache paths, preserves existing approval requirements, and merges small PRs only after checks and reviews pass. By default, caches are reclaimed after 7 idle days, with a total size target of 20 GiB.

## Follow-up State

`TRACKING.md` lists unresolved issues and PRs. The agent uses only this file and `STATE.md` to resume follow-up each round, updating each item's status and next action. Reports under `reports/` are for you to read; the agent does not read them back, keeping historical reports out of its context. When upgrading an existing instance, create this file manually. On its first round, the agent rebuilds the list from open GitHub items in which this Loop has already participated.

The main agent assigns a subagent to analyze each repository concurrently, providing an absolute repository path for each subagent. If the host supports a working-directory option, set it to that directory; otherwise, specify the path explicitly in tool calls. If subagents are unavailable, process repositories sequentially.

After all repository analyses finish, the main agent handles the resulting actions and sends a single combined summary using Computer Use only.

For scheduled runs, configure your local agent to invoke the same `LOOP.md`.
