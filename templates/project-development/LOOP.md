# Project Development Loop

Configuration: [CONFIG.md](CONFIG.md)

## LOOP CONTRACT

### Goal

Fulfill the current execution scope and ensure all applicable enabled stages pass. If the maximum iteration count is reached and not all applicable stages have passed, report the stages that have not passed, the specific issues, and the attempted remedies. Do not claim completion.

### Boundary

- Work only on the project containing this Loop template, respecting user authorization and project conventions.
- Persist documents for people to read and maintain: by default, the project brief, requirements, architecture, and technical design, only as needed for the selected scope and enabled stages. Reuse and update existing documents in place. Additional documents need an explicit user request, a project requirement, or a clear ongoing reader need, such as operating or using the delivered system; stage execution alone is not a reason to create a file.
- Return clarification, development, review, repair, acceptance, and deployment results in the conversation or subagent response. Do not create files or directories solely to record execution steps, intermediate results, verification evidence, or stage handoffs by default, regardless of their names or formats. Cite actual code, commands and outcomes, test results, or existing CI/deployment records as evidence without copying them into project documents. Put tool-generated diagnostic files in temporary storage or the project's existing artifact system, not the maintained documentation directory.
- Retries, fixes, and deployments reuse the same requirement scope and document paths; do not create per-attempt, timestamped, or commit-suffixed documents. Fold lasting changes to requirements, design decisions, or operating instructions into the relevant maintained document, without appending execution history. These output rules apply to every stage, project-specific deployment procedure, and subagent.
- Execute only stages with `enabled: true`, in the order shown below. Skip disabled stages and their skills. Use the procedures and output conventions in `skill/<stage-id>/SKILL.md`. Keep scripts and other resources in the corresponding skill directory, resolving resource paths relative to that directory. If required procedures or inputs are missing, report this to the user and wait.
- Count an iteration only when a stage after code development returns to code development for a code fix. Allow at most `max_iterations` such iterations. If the limit is reached without resolution, stop and report to the user.

### SOP

1. Read CONFIG. Verify that `max_iterations` is a positive integer, `stages` contains every stage in the order below, each `enabled` and `use_subagent` value is a boolean, and at least one stage is enabled. Otherwise, report this to the user and wait. Start at the enabled stage specified by the user, or the first enabled stage if none is specified. Skip earlier stages for this execution without changing CONFIG. If the specified stage is disabled, report this to the user and wait.

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

2. Execute the current stage once according to its `use_subagent` setting and return the result, current scope, locations of any actual deliverables, concise supporting evidence, and unresolved issues in the conversation or subagent response. A stage result is not a file deliverable; do not create a document just to supply an output path or prove the stage ran. Carry the returned scope and handoff information into subsequent stages.
   - `false`: The main agent reads and analyzes the corresponding skill, then executes the stage. This setting does not disable subagents for tasks within the stage. Delegate tasks suited to subagents, even when the skill does not explicitly require delegation, and follow any delegation requirements in the skill.
   - `true`: The main agent does not read the corresponding skill. It gives a subagent the skill's absolute path, the project directory, the absolute path of `LOOP.md`, the current scope, the user's request, existing outputs, and unresolved issues. Instruct the subagent to read the skill and execute the current stage, following this Loop's boundaries and result rules. The main agent waits and decides the next step solely from the returned result.
   - Give every subagent the document-retention boundaries above and require it to return findings directly, without writing a handoff or report file. Subagents must not advance to other stages or modify logs. Any delegation of specific tasks explicitly required by a skill must still be performed.
3. The main agent decides the next step from the result:
   - `passed`: The skill's pass criteria are met. Proceed to the next enabled stage and return to step 2. Only when the current scope is fulfilled and all applicable enabled stages have passed, report completion and outputs, then follow step 4 before finishing.
   - `retry_stage`: The current stage has not passed, but a viable remedy is available within this stage. Retain the issues and return to step 2 to retry. Stages after code development may use this result only when no code changes are needed. Return `blocked` if the issues cannot be resolved independently.
   - `fix_code`: A stage after code development requires code changes. The current executor stops without fixing code in that stage. Invalidate previous pass results for code development and all later stages. Return to code development with the issues and execute it according to step 2. After the fix, rerun all subsequent enabled stages in order. If code development is disabled, report this to the user and wait for action; do not enable it automatically.
   - `blocked`: Procedures, inputs, or capabilities are insufficient, the issue cannot be resolved independently, or the next step cannot be determined. Report the specific problem and wait for user action.

4. After completing a child requirement, the main agent reads the child order in the parent `requirements.md`. If another child follows, end the final response with a complete, copyable prompt for that next child in the user's language, including actual absolute paths, its scope, dependencies, and an enabled start stage. This is required even if requirements documentation was skipped or the prompt appeared earlier. If this was the last child, report that the whole major requirement is complete and stop; do not hand off to another major requirement.

## STATE + LOG

### State

Each execution handles only its current scope. Do not save execution state across executions. Retain only useful maintained project documents under the boundaries above, not stage-progress records or execution evidence archives.

### Logs

[LOGS.md](LOGS.md) is initially empty. Record only reusable general problems and verified solutions, not issues specific to a request or its execution history.
