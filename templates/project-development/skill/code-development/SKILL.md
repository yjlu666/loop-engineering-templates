---
name: code-development
description: Implement the requested behavior or returned fixes and verify the resulting changes using project conventions.
---

# Code Development

Implement the request using the available requirements, design, project conventions, and any issues returned by later stages.

Implement only the active sub-requirement when the major requirement was split; otherwise implement the selected major requirement. Use the available requirements, technical design, and shared parent architecture as context; when a preceding stage was skipped, use equivalent information from the request and code. Include shared code changes only as necessary for this scope. Return implementation and fix summaries directly in the conversation or subagent response; do not create development reports, fix reports, or evidence directories by default.

## Approach

- Inspect the affected code and working-tree changes before editing. Preserve unrelated work and determine the current task's change scope, including existing task commits and uncommitted files.
- Make the smallest coherent implementation that satisfies the intended behavior. Reuse project patterns and dependencies; adjust routine plan details when code evidence warrants it and explain material deviations.
- Add or update tests where they meaningfully protect changed behavior or reproduce a defect. Use the project's testing approach; do not impose a test framework or a fixed test-first sequence.
- Run checks appropriate to the affected surface and any checks required by the project. Diagnose failures, fix those caused by the change, and distinguish unrelated failures or unavailable checks from passing results.
- Inspect the final diff for scope and accidental changes. Update affected usage or interface documentation when behavior changes.

## Result

Return the change summary and locations, the task's review scope, checks actually run with their outcomes, and unresolved issues. Return `passed` when the requested implementation or returned fixes are complete and required local checks pass. Return `retry_stage` if a known local remedy remains, or `blocked` if a necessary decision, dependency, or verification capability is unavailable. Local success does not establish deployment or environment acceptance.
