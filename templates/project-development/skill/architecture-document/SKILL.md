---
name: architecture-document
description: Create, update, or reference the shared architecture of a major requirement for whole-requirement or child execution.
---

# Architecture Document

Maintain the shared architecture of the current major requirement, including when this execution implements only one of its children.

## Approach

- Read available requirements, child documents when split, and existing architecture or code. If earlier stages were skipped, establish the major requirement's scope from the user request and project context; do not require a brief, requirements file, or existing implementation. Distinguish existing implementation from planned components; sibling requirements are design context, not authorization to implement them.
- Create, update, or reference `architecture.md` in `<loop-directory>/<project-name>-docs/<requirement-id>/`, resolving the major requirement directory from the current scope, user prompt, or existing documents even when the active directory is a child. Reuse existing paths or derive stable names from the project and request. Never create `architecture.md` in a child directory. Cover the whole major requirement's component responsibilities, shared interfaces, dependencies, and data flow at a useful level of detail. Add a diagram only when it makes these relationships clearer.
- For consequential decisions, record the context, choice, rationale, and tradeoffs. Mention alternatives when they explain the choice; keep any separate decision records in the major requirement directory. Preserve shared contracts used by other children and identify the impact of changes to them.
- Address security boundaries, persistence, deployment, or scaling only to the extent required by the change. Prefer established project patterns and avoid speculative infrastructure.
- Check the shared design against the whole major requirement and verify that the active child's needs are covered. If the existing parent architecture is sufficient, reference it without rewriting it. Hand off only the selected child, or the intact major requirement when unsplit, to the next enabled stage.

## Result

Return the parent architecture path, key decisions, the selected scope and active directory for later stages, and unresolved risks. Return `passed` when the shared document covers the major requirement's architecture and supports the selected scope. Return `blocked` if a consequential decision needs unavailable information or user input.
