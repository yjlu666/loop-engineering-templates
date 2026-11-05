---
name: requirements-clarification
description: Resolve material ambiguity in requested behavior and acceptance criteria during requirements clarification.
---

# Requirements Clarification

Use the request, available brief, and project context to establish what the change must do.

Clarify only the selected requirement, using the user's request to establish its scope when initiation was skipped. Use the project brief and other requirements as context if available; do not require a brief or expand this execution to cover other requirements. Verify any stated prerequisites from the project before relying on them.

## Approach

- Reuse answers already present in the conversation, documentation, and code. Identify gaps in user behavior, scope, constraints, or the meaning of success.
- Ask only questions whose answers would materially change the outcome. Include a recommended interpretation when useful. Resolve routine, reversible details with project conventions and state any meaningful assumptions.
- Express the main user flows and relevant failure cases as observable behavior. Distinguish required behavior from optional ideas; do not expand the request to fill a checklist.
- Summarize the resolved requirements, acceptance criteria, and remaining assumptions in the conversation or subagent response, without a separate clarification file. Carry lasting conclusions into the requirements document when that stage runs. If the request is already clear, produce this summary directly without another approval round.

## Result

Return the clarification summary and unresolved questions. Return `passed` when implementation and acceptance can proceed without guessing a material product decision. Return `blocked` for a missing answer that changes the intended behavior, scope, or required constraints; include the precise question.
