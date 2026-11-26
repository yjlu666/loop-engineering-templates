# Project Development Loop Guide

[Simplified Chinese](zh-CN/project-development-guide.md)

## Getting Started

1. Copy the entire [templates/project-development/](../templates/project-development/) directory into the project you want to develop, including `skill/`.
2. Fill in your copy of `CONFIG.md`. Select stages through each stage's `enabled` field, keeping the complete list and its original order.
3. Review the built-in skills for stages 1–7 and adapt them to your project if needed. Stages 8–10 (`test-deployment`, `test-acceptance`, and `production-deployment`) contain only metadata: supply their project-specific inputs, procedures, results, and pass criteria before enabling them, or disable them. These procedures must follow the document-retention boundaries in `LOOP.md`; acceptance and deployment results do not require report files. All stages are enabled in the default configuration, so an unfilled stage will block execution. Disabled stages do not need procedures.
4. Give the agent a specific request and tell it: `Read <instance-directory>/LOOP.md and execute it until the work is complete, user action is needed, or the iteration limit is reached.`

## Skill Directories

Each stage uses its own directory with `SKILL.md` as the entry point. Add scripts, references, or assets within that directory as needed and reference them from `SKILL.md`. Resolve paths relative to that skill directory.

```text
skill/<stage-id>/
├── SKILL.md
├── scripts/       # Add scripts as needed
├── references/    # Add references as needed
└── assets/        # Add assets as needed
```

The built-in skills provide concise procedures and pass criteria without scripts or external skill dependencies.

## Built-in Skills

The skills leave routine implementation choices to the agent, use existing project documents as context, and ask questions only for material gaps. The Loop controls delegation and returns to code development; the skills do not introduce additional approval gates, mandatory TDD, commit sequences, or execution ledgers.

Stages can use equivalent context when earlier stages are disabled; they do not require a particular input document or file name. Test acceptance needs an identified test target; when test deployment is disabled, supply an existing target or explicitly agree to local acceptance.

## Generated Documents

Default retained documents are the project brief, requirements, architecture, and technical design, as needed for the selected scope and enabled stages. Write them for people to understand and maintain the project. All paths are relative to the directory containing `LOOP.md`. Keep the project brief at project level and shared architecture at major requirement level. When a major requirement is split, each child gets its own requirements and technical design. For example:

```text
<loop-directory>/
├── LOOP.md
├── CONFIG.md
├── skill/
└── shop-docs/
    ├── project-brief.md
    ├── user-login/
    │   ├── requirements.md
    │   ├── architecture.md
    │   ├── password-login/
    │   │   ├── requirements.md
    │   │   └── technical-design.md
    │   └── password-reset/
    │       ├── requirements.md
    │       └── technical-design.md
    └── order-export/
        ├── requirements.md
        ├── architecture.md
        └── technical-design.md
```

Stage skills resolve document directories from the selected scope and existing outputs, and return those paths for subsequent stages. Reuse stable IDs and update documents in place across retries and follow-ups. No child gets a separate architecture document. Split requirements by user-visible outcomes, not by reviews, test runs, deployment attempts, or fixes within the same scope.

Clarification, development, review, repair, acceptance, and deployment results belong in the conversation or subagent response. Do not create files or directories solely to record execution steps, intermediate results, verification evidence, or stage handoffs by default, regardless of their names or formats. Keep acceptance criteria in requirements and the verification approach in technical design. Return concise evidence from actual commands and outcomes, code locations, tests, or existing CI/deployment records; no report file is needed to pass a stage. Tool-generated diagnostic files belong in temporary storage or the project's existing artifact system, not the maintained documentation directory.

Additional documents need an explicit user request, a project requirement, or a clear ongoing reader need, such as a deployment runbook or user guide. Reuse the relevant maintained document and fold lasting conclusions into it; do not append execution history or create timestamped or commit-suffixed copies. These rules also apply to subagents and project-specific acceptance and deployment procedures.

## Decomposition and Separate Executions

The stage skills own decomposition and document placement. The main agent follows `LOOP.md` to include the next child's prompt in its final response after completing a child.

1. **Project initiation:** Write the project-level `project-brief.md`, including an ordered breakdown into major requirements, their scope, and dependencies. Continue with only the first major requirement. A project with one coherent requirement need not be artificially split.
2. **Requirements documentation:** If the selected major requirement is too large and meaningfully divisible, retain its overall scope and child index in the parent `requirements.md` and generate a requirements document for every child. Continue with only the first child. Order the children by dependencies.
3. **Architecture (optional):** When enabled and included in this execution, create, update, or reference the whole major requirement's `architecture.md`, even in a child execution. Preserve shared contracts and distinguish planned sibling behavior from implemented behavior.
4. **Technical design and later stages:** Work only on the selected child, or the major requirement if unsplit, including necessary shared changes and regression checks. This also applies to project-specific test and production deployment procedures.

A new Loop means a separate execution or conversation using the same `LOOP.md` and documents, not a copied template or an automatically started sibling. Each generated prompt must contain actual absolute paths, the selected scope, its dependencies, and a named start stage. After completing a child, read the child order in the parent `requirements.md` and end the final response with the next child's complete, copyable prompt, even if this execution skipped requirements documentation or the prompt appeared earlier. Keep prompts only in the conversation. Do not write them into the project brief, requirements documents, or other project files.

An explicit start stage skips earlier stages for this execution without changing CONFIG or marking them passed. The start stage must be enabled and required inputs available; otherwise report the blocker. Subsequent enabled stages run in their configured order. When architecture documentation is absent, technical design uses requirements and project context directly, recording necessary decisions in the technical design without requiring an architecture-stage restart. Verify dependency completion from actual project evidence instead of assuming another Loop finished it.

For children A → B → C: completing A ends with B's prompt; completing B in a new conversation ends with C's prompt; completing C reports the whole major requirement complete and stops. Do not hand off to another major requirement.

## Configuration

Configuration is stored separately in [CONFIG.md](../templates/project-development/CONFIG.md) and contains only the following data:

| Field | Default | Purpose |
|---|---|---|
| `max_iterations` | `30` | A positive integer limiting how many times later stages may return to code development for fixes |
| `stages` | All 10 stages | Keep the complete list and its order; select stages through `enabled` |
| `stages[].id` | See the stage table in LOOP | The stage ID, corresponding to `skill/<id>/SKILL.md` |
| `stages[].enabled` | All `true` | Whether to enable the stage; at least one must be enabled. When `false`, skip both the stage and its skill |
| `stages[].use_subagent` | All `false` | Whether to delegate the entire stage to a subagent |

The fixed order is: project initiation → requirements clarification → requirements document generation → architecture document generation → technical design generation → code development → code review → test deployment → test acceptance → production deployment. Enable one or more stages as needed. Skipped stages do not make their documents mandatory; use equivalent context from the request and project, and ask only for information that is genuinely necessary and unavailable.

When `use_subagent: true`, the main agent does not read the stage's skill. It delegates the entire stage to a subagent to read and analyze the skill and execute the stage, then decides the next step from the returned result. When `false`, the main agent reads and analyzes the skill and executes the stage. This setting does not disable subagents for tasks within the stage: tasks suited to subagents must still be delegated, even when the skill does not explicitly require delegation. Any delegation requirements in the skill must also be followed.

Code review requires a fresh subagent that did not implement the changes. With `use_subagent: false`, the main agent coordinates and dispatches the reviewer. If the stage is already delegated to a fresh independent reviewer, it reviews directly without further delegation. The reviewer receives the task, change scope, and relevant evidence rather than the implementation conversation. One reviewer is sufficient by default. Self-review cannot substitute for this step; unavailable independent review is reported as blocked. Fixes return to code development and are independently reviewed again on the next review stage.

## Returning to Earlier Stages and Stopping

- After a stage passes, proceed to the next enabled stage. This execution is complete only when its selected scope is fulfilled and all applicable enabled stages from its start stage onward have passed. Give the next child's prompt at the end; after the last child, report the major requirement complete and stop.
- If a stage after code development requires code changes, return to code development. After fixing the code, rerun all subsequent enabled stages in order. If code development is disabled, report the issue to the user and wait for action.
- Handle issues that do not require code changes within the current stage. If they cannot be resolved, report them to the user and wait.
- An iteration is counted only when a stage after code development returns to code development for a code fix. If the `max_iterations` limit is reached and not all stages have passed, stop and report the stages that have not passed, the specific issues, and the attempted remedies. Do not claim completion.

Each execution handles only its selected scope and saves no execution state across executions. Documents retain decomposition and dependencies, not follow-up prompts or stage-progress records. A new execution starts at the stage named in its prompt, or the first enabled stage when none is named. `LOGS.md` initially contains no entries and records only reusable general problems and verified solutions, not issues specific to a request or its execution history.
