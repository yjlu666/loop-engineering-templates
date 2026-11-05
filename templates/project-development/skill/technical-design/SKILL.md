---
name: technical-design
description: Translate requirements and available project context into a concise implementation approach and verification plan.
---

# Technical Design

Describe the implementation decisions that a capable coding agent cannot infer from the requirements and code alone.

## Approach

- Establish the selected scope and expected behavior from the user's request and available requirements or clarification results. Read relevant code, tests, and the parent architecture if available. Prior documents and existing code are not prerequisites: derive necessary decisions from the available context and record them in the technical design. Ask only for material information that cannot be resolved, not for missing stage artifacts. Check actual dependencies rather than assuming earlier Loops completed them.
- Create or update `technical-design.md` in the active document directory: `<loop-directory>/<project-name>-docs/<requirement-id>/<sub-requirement-id>/` when split, otherwise `<loop-directory>/<project-name>-docs/<requirement-id>/`. Reuse the directory from the current scope or derive stable names from the project and request when no prior stage supplied it. Plan only the selected scope and shared changes necessary to support it. Identify affected modules, interfaces, data changes, and intended behavior; leave routine implementation details open. Do not plan sibling implementation or create a child architecture document.
- Explain non-obvious choices and applicable migration, failure-handling, or compatibility concerns. Keep a small change as a short plan; split larger work only where dependencies or independently verifiable outcomes justify it.
- Identify how the acceptance criteria will be checked using the project's available tests and tools. Include concrete commands when known and distinguish local checks from validation requiring the target environment. Keep this verification approach in the technical design; do not create a separate acceptance-plan file by default. Update lasting design changes in place without adding per-fix plans or execution history.
- Check that the approach covers the requirements without adding unrelated work. Resolve ordinary technical decisions directly; surface only uncertainties that materially affect correctness or scope.

## Result

Return the plan path, selected scope, active document directory, and unresolved issues. Carry only this scope into development, review, acceptance, and deployment stages; unfinished siblings remain separate tasks. Return `passed` when the intended changes, important contracts, and verification approach are sufficient to implement the request. Return `blocked` when a material decision cannot be made from available evidence.
