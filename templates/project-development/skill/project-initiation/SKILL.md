---
name: project-initiation
description: Document project initiation, decompose major requirements, and select the first for this Loop execution.
---

# Project Initiation

Establish the project-level brief shared by its requirements and subsequent development stages.

## Approach

- Read the request, project instructions, and available documentation or code. For an existing project, use its initiation document if available; otherwise establish the brief from the request and project context. Assess whether the request changes project-level goals, scope, or constraints. For a new project, identify the smallest useful initial outcome.
- State the problem, intended users, desired outcome, scope, and explicit exclusions. Keep user requirements distinct from assumptions.
- Identify constraints or dependencies that could prevent delivery, such as a required integration or unavailable input. Investigate what the project can answer; ask only about decisions that materially change scope or feasibility.
- Create or maintain `<loop-directory>/<project-name>-docs/project-brief.md`, deriving a short, stable project name from the project context or reusing its existing document directory. Resolve the path from the directory containing `LOOP.md`. Capture the project objective, scope, constraints, feasibility, and recommended starting point, referencing existing project material as useful. Reuse the existing document unchanged when a new requirement does not alter these project-level decisions; do not replace the project brief with a single requirement's scope. Leave detailed requirements and implementation decisions to their respective stages.
- In the brief, break the project into a small number of major requirements with stable IDs, directories, objectives, boundaries, and dependencies. Order them so the first is a useful deliverable with satisfiable prerequisites. Preserve existing IDs and decomposition when still valid; do not force an artificial split for a project with only one coherent requirement.
- Select only the first major requirement for this execution's requirements clarification and later stages. The other major requirements remain outside this execution.

## Result

Return the brief path, ordered major requirements, selected first requirement and directory, and unresolved issues. Return `passed` when the brief contains the breakdown, the first requirement is actionable, and no known feasibility blocker prevents starting it. Return `blocked` when a necessary decision or dependency prevents a credible starting point; explain what is missing. Later requirements' prerequisites belong in the brief and do not alone block the first requirement.
