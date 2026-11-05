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
- Select only the first major requirement for this execution's requirements clarification and later stages. The other major requirements remain outside this execution. For each, present a separate follow-up prompt in the conversation and tell the user to open a new Loop execution starting at `requirements-clarification`, in dependency order.

## Follow-up Prompt

Produce one filled-in prompt per remaining major requirement in the user's language, using actual absolute paths and concrete prerequisites rather than placeholders. A new Loop means a separate execution using the same `LOOP.md` and project documents; no template copy is needed. Provide these prompts only in the conversation, including the final or early-stop handoff; do not save them in project documents. Do not start the later tasks automatically or claim they are complete.

```text
Read <absolute LOOP.md path> and execute a new Loop for major requirement <ID and title> only, starting at requirements-clarification. Project: <absolute project path>. Read <absolute project-brief.md path> for this requirement's scope and dependencies. Use <absolute major requirement directory> for its documents. Prerequisites: <specific dependencies and how to verify them, or none>. Skip project initiation for this execution; preserve CONFIG and follow its enabled stages from the start stage onward. If the start stage is disabled or prerequisites are unavailable, report the blocker. If requirements documentation splits this requirement, continue only with its first child and provide separate prompts for the others. Do not execute other major requirements or claim the whole project is complete.
```

## Result

Return the brief path, ordered major requirements, selected first requirement and directory, all follow-up prompts, and unresolved issues. Return `passed` when the brief contains the breakdown, the follow-up prompts have been provided in the conversation, the first requirement is actionable, and no known feasibility blocker prevents starting it. Return `blocked` when a necessary decision or dependency prevents a credible starting point; explain what is missing. Later requirements' prerequisites belong in their handoff prompts and do not alone block the first requirement.
