# Project Development Loop

Configuration: [CONFIG.md](CONFIG.md)

## LOOP CONTRACT

### Goal

Fulfill the user's request and ensure all stages selected by the user pass. If the maximum iteration count is reached and not all stages have passed, report the stages that have not passed, the specific issues, and the attempted remedies. Do not claim completion.

### Boundary

- Work only on the project containing this Loop template, respecting user authorization and project conventions.
- Execute only stages with `enabled: true`, in the order shown below. Skip disabled stages and their skills. The user supplies procedures in the body of `skill/<stage-id>/SKILL.md`. Keep scripts and other resources in the corresponding skill directory, resolving resource paths relative to that directory. If required procedures or inputs are missing, report this to the user and wait.
- Count an iteration only when a stage after code development returns to code development for a code fix. Allow at most `max_iterations` such iterations. If the limit is reached without resolution, stop and report to the user.

### SOP

1. Read CONFIG. Verify that `max_iterations` is a positive integer, `stages` contains every stage in the order below, each `enabled` and `use_subagent` value is a boolean, and at least one stage is enabled. Otherwise, report this to the user and wait. Start the current user request at the first enabled stage.

   | Order | Stage ID | Stage | Procedure File |
   |---|---|---|---|
   | 1 | `project-initiation` | Project initiation | [skill/project-initiation/SKILL.md](skill/project-initiation/SKILL.md) |
   | 2 | `requirements-clarification` | Requirements clarification | [skill/requirements-clarification/SKILL.md](skill/requirements-clarification/SKILL.md) |
   | 3 | `requirements-document` | Requirements document generation | [skill/requirements-document/SKILL.md](skill/requirements-document/SKILL.md) |
   | 4 | `architecture-document` | Architecture document generation | [skill/architecture-document/SKILL.md](skill/architecture-document/SKILL.md) |
   | 5 | `technical-design` | Technical design generation | [skill/technical-design/SKILL.md](skill/technical-design/SKILL.md) |
   | 6 | `code-development` | Code development | [skill/code-development/SKILL.md](skill/code-development/SKILL.md) |
   | 7 | `code-review` | Code review | [skill/code-review/SKILL.md](skill/code-review/SKILL.md) |
   | 8 | `test-deployment` | Test deployment | [skill/test-deployment/SKILL.md](skill/test-deployment/SKILL.md) |
   | 9 | `test-acceptance` | Test acceptance | [skill/test-acceptance/SKILL.md](skill/test-acceptance/SKILL.md) |
   | 10 | `production-deployment` | Production deployment | [skill/production-deployment/SKILL.md](skill/production-deployment/SKILL.md) |

2. Execute the current stage once according to its `use_subagent` setting and return the result, output locations, supporting evidence, and unresolved issues.
   - `false`: The main agent reads the corresponding skill and executes it. If the skill explicitly requires subagents for particular actions, delegate those actions as specified.
   - `true`: The main agent does not read the corresponding skill. It gives a subagent the skill's absolute path, the project directory, the user's request, existing outputs, and unresolved issues. Instruct the subagent to read the skill and execute the current stage, following this Loop's boundaries and result rules. The main agent waits and decides the next step solely from the returned result.
   - Subagents must not advance to other stages or modify logs. Any delegation of specific tasks explicitly required by a skill must still be performed.
3. The main agent decides the next step from the result:
   - `passed`: The skill's pass criteria are met. Proceed to the next enabled stage and return to step 2. Report completion and outputs, then finish, only when the user's request is fulfilled and all enabled stages have passed.
   - `retry_stage`: The current stage has not passed, but a viable remedy is available within this stage. Retain the issues and return to step 2 to retry. Stages after code development may use this result only when no code changes are needed. Return `blocked` if the issues cannot be resolved independently.
   - `fix_code`: A stage after code development requires code changes. The current executor stops without fixing code in that stage. Invalidate previous pass results for code development and all later stages. Return to code development with the issues and execute it according to step 2. After the fix, rerun all subsequent enabled stages in order. If code development is disabled, report this to the user and wait for action; do not enable it automatically.
   - `blocked`: Procedures, inputs, or capabilities are insufficient, the issue cannot be resolved independently, or the next step cannot be determined. Report the specific problem and wait for user action.

## STATE + LOG

### State

Each execution handles only the current user request. Do not save state across executions.

### Logs

[LOGS.md](LOGS.md) is initially empty. Record only reusable general problems and verified solutions, not issues specific to a request or its execution history.
