---
name: requirements-document
description: Document the current major requirement, split it into children when needed, and select the first child for this execution.
---

# Requirements Document

Capture the clarified request in a document that implementation and acceptance can both use.

## Approach

- Use the user's request and available clarification results. If earlier stages were disabled, use equivalent supplied context; a missing stage artifact alone is not a blocker.
- Create or update `requirements.md` in `<loop-directory>/<project-name>-docs/<requirement-id>/`, resolving the major requirement directory from the current scope, project brief, or user prompt. Reuse stable names and paths across retries and follow-ups. Resolve the path from the directory containing `LOOP.md`; reference existing project documents as inputs.
- Cover the problem and scope, required user behavior, relevant constraints, exclusions, and verifiable acceptance criteria. Include failure behavior and nonfunctional requirements only where they matter to this request.
- Give each requirement a stable heading or identifier that later stages can reference. Write expected outcomes concretely enough to test; preserve assumptions as assumptions, and do not invent metrics or commitments.
- Check for contradictions and missing acceptance criteria. Keep the document proportional to the change and reuse existing detail by reference.
- If the major requirement is too large for one focused implementation and can be divided into coherent, verifiable outcomes, split it into a small number of sub-requirements. Otherwise keep it intact. Preserve existing child IDs when updating a prior breakdown.
- Split by user-visible outcomes, not workflow steps. Reviews, test runs, deployment attempts, and fixes within the same scope are not new requirements and do not get their own requirement directories or documents.
- When splitting, keep the parent `requirements.md` as the overall scope, shared constraints, ordered child index, and dependency map. Create `<sub-requirement-id>/requirements.md` for every child under the major requirement directory, with its own scope, acceptance criteria, dependencies, and a reference to the parent. Together the children must cover the parent without duplicate ownership or missing behavior. Order prerequisite work first so the first child is actionable.
- When split, select only the first child for the rest of this execution. Return its directory as the active document directory; architecture remains in the parent directory. When unsplit, retain the major requirement as the active scope and directory. Do not implement or start sibling Loops here.

## Result

Return the parent document path and, if split, every child document path, and the selected first child and active directory. Return `passed` when the documents cover the major requirement with observable acceptance criteria and the first selected scope is actionable. A split also requires all child documents to be saved before passing. Return `blocked` if a material requirement cannot be resolved from available context.
