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
- When split, select only the first child for the rest of this execution. Return its directory as the active document directory; architecture remains in the parent directory. Present a separate prompt for every later child in the conversation and tell the user to open a new Loop execution for each in dependency order. When unsplit, retain the major requirement as the active scope and directory. Do not implement or start sibling Loops here.

## Follow-up Prompt

Produce one filled-in prompt per remaining child in the user's language with actual absolute paths and concrete prerequisites. Choose the start stage according to the user's selection and CONFIG: `architecture-document` when selected, otherwise `technical-design`; if both are disabled, use the next enabled stage. Starting at technical design does not require an architecture document. Provide prompts only in the conversation, including the final or early-stop handoff; do not save them in requirements documents. Completing the selected child does not complete the parent requirement or project.

```text
Read <absolute LOOP.md path> and execute a new Loop for sub-requirement <child ID and title> of major requirement <parent ID and title> only, starting at <selected enabled start stage>. Project: <absolute project path>. Read <absolute project-brief.md path if available>, <absolute parent requirements.md path>, and <absolute child requirements.md path>. Major requirement directory: <absolute parent directory>. Active document directory: <absolute child directory>. Prerequisites: <specific dependencies and how to verify them, or none>. Reference the parent architecture.md if available. If the architecture stage runs, maintain it at major requirement level; never create a child architecture document. Architecture documentation is optional and its absence does not prevent technical design. Skip earlier stages, preserve CONFIG, and follow its enabled stages from the start stage onward. If the start stage is disabled or dependencies are unavailable, report the blocker. All stages after architecture serve only this child. Do not execute siblings or claim the parent requirement or project is complete.
```

## Result

Return the parent document path and, if split, every child document path, the selected first child and active directory, and all follow-up prompts. Return `passed` when the documents cover the major requirement with observable acceptance criteria and the first selected scope is actionable. A split also requires all child documents to be saved and all follow-up prompts to be provided in the conversation before passing. Return `blocked` if a material requirement cannot be resolved from available context.
