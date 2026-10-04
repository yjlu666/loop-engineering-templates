# Writing a New Loop

[Simplified Chinese](zh-CN/template-authoring.md)

`templates/starter/` is a blank template for a new Loop. Copy it, fill in the specific task, and give it to an agent to execute.

## Using the Files

| File | What to Put In It |
|---|---|
| LOOP.md | You define the goal for each round, operational boundaries, and concrete steps |
| CONFIG.md | You fill in the configurable parameters this Loop needs |
| STATE.md | The agent saves the necessary state as defined by the Loop |
| LOGS.md | The agent records exceptions, lessons learned, and solutions as defined by the Loop |

If the Loop needs the agent to carry a list across rounds, such as unresolved items, save it in a separate Markdown file and describe it in the State section of LOOP.md. User-facing reports are for people to read; the agent should not read them back.

## Get Started in Three Steps

1. Copy `templates/starter/` and rename it for your template.
2. Replace the `[ ... ]` placeholders in LOOP.md to define the goal, constraints, steps, and the state and logs to retain. Fill in CONFIG.md as needed.
3. Tell your local agent: `Read <template-directory>/LOOP.md and execute one round.`
