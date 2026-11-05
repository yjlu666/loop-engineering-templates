---
name: code-review
description: Use an independent subagent to review the current task's changes for actionable defects without fixing code in this stage.
---

# Code Review

Have a reviewer that did not implement the changes assess them against the request and surrounding code. The implementer's self-review does not satisfy this stage.

Review the active sub-requirement when split, otherwise the selected major requirement, including its necessary shared changes and their effects on existing behavior. Use the parent architecture when available, otherwise assess shared contracts from requirements and code; unimplemented sibling scope is not a missing feature of this execution. Return findings and evidence directly in the conversation or subagent response; do not save review reports or evidence directories by default.

## Independent Reviewer

- Use a fresh subagent that did not author the changes. With `use_subagent: false`, the main agent coordinates this stage and must delegate the actual review. If the stage already runs in a fresh, independent subagent, that subagent performs the review directly; do not delegate again. Otherwise dispatch a fresh reviewer. One reviewer covering the whole scope is the default.
- Give the reviewer the project path, this skill, the requested behavior, the selected scope, and the complete change range or file set, including uncommitted changes. Include relevant project instructions, available documents, and verification evidence. Use a fresh context rather than copying the implementation conversation or the author's verdict; the reviewer must inspect the code and reach its own conclusion.
- The reviewer follows the approach below and returns findings without editing project code or starting other stages. The coordinator may check disputed findings against evidence, but must not replace the independent review with its own approval. If an independent subagent is unavailable, return `blocked` and state the missing capability instead of silently falling back to self-review.

## Reviewer Approach

- Establish the complete review scope from the user request, project changes, and any available development result; no development-stage report or design document is required. Include relevant committed, staged, unstaged, and new files; do not assume the last commit contains the whole task. If development was skipped, use the supplied change range or infer it from the task and repository; ask only if the scope remains ambiguous.
- Read the changed code and relevant callers, contracts, and tests. Focus on concrete defects, regressions, missing requirements, and consequential security or data-handling issues.
- Verify suspected issues through code evidence or focused checks. Report actionable findings with location, triggering conditions, impact, and a suggested correction. Distinguish demonstrated defects from unresolved suspicions.
- Keep optional style or refactoring advice separate from issues that must be fixed. Do not invent findings to fill a quota or expand the task into a general audit.
- Review without modifying project code. Return necessary fixes to the Loop's code-development stage.

## Result

Return the reviewer identity, reviewed scope, findings ordered by impact, supporting evidence, and verification limits. Return `passed` only after the independent review is complete and no required fix remains; optional suggestions may remain. Return `fix_code` for an actionable issue requiring code or test changes, leaving fixes to code development. After fixes, independently review the updated changes when this stage runs again; the earlier verdict does not cover the new code. Return `retry_stage` for a resolvable review-evidence gap, or `blocked` when independence, scope, or a necessary judgment cannot be established.
