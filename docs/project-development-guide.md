# Project Development Loop Guide

[Simplified Chinese](zh-CN/project-development-guide.md)

## Getting Started

1. Copy the entire [templates/project-development/](../templates/project-development/) directory into the project you want to develop, including `skill/`.
2. Fill in your copy of `CONFIG.md`. Select stages through each stage's `enabled` field, keeping the complete list and its original order.
3. Fill in the body of `skill/<stage-id>/SKILL.md` for each enabled stage, specifying its inputs, procedures, outputs, and pass criteria. Each `SKILL.md` initially contains only the required `name` and `description` metadata, with its procedure left blank. Disabled stages do not need to be filled in.
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

The template provides only `SKILL.md`, without predefined procedures or scripts.

## Configuration

Configuration is stored separately in [CONFIG.md](../templates/project-development/CONFIG.md) and contains only the following data:

| Field | Default | Purpose |
|---|---|---|
| `max_iterations` | `30` | A positive integer limiting how many times later stages may return to code development for fixes |
| `stages` | All 10 stages | Keep the complete list and its order; select stages through `enabled` |
| `stages[].id` | See the stage table in LOOP | The stage ID, corresponding to `skill/<id>/SKILL.md` |
| `stages[].enabled` | All `true` | Whether to enable the stage; at least one must be enabled. When `false`, skip both the stage and its skill |
| `stages[].use_subagent` | All `false` | Whether to delegate the entire stage to a subagent |

The fixed order is: project initiation → requirements clarification → requirements document generation → architecture document generation → technical design generation → code development → code review → test deployment → test acceptance → production deployment. Enable one or more stages as needed. The user must supply any prerequisite inputs that skipped stages would otherwise produce.

When `use_subagent: true`, the main agent does not read the stage's skill. It delegates the skill to a subagent to read and execute, then decides the next step from the returned result. When `false`, the main agent executes the stage. If the skill explicitly requires subagents for particular actions, those actions must still be delegated.

## Returning to Earlier Stages and Stopping

- After a stage passes, proceed to the next enabled stage. The work is complete only when the user's request is fulfilled and all enabled stages have passed.
- If a stage after code development requires code changes, return to code development. After fixing the code, rerun all subsequent enabled stages in order. If code development is disabled, report the issue to the user and wait for action.
- Handle issues that do not require code changes within the current stage. If they cannot be resolved, report them to the user and wait.
- An iteration is counted only when a stage after code development returns to code development for a code fix. If the `max_iterations` limit is reached and not all stages have passed, stop and report the stages that have not passed, the specific issues, and the attempted remedies. Do not claim completion.

Each execution handles only the current user request and saves no state across executions. A new request starts at the first enabled stage. `LOGS.md` initially contains no entries and records only reusable general problems and verified solutions, not issues specific to a request or its execution history.
