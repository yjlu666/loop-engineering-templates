# Repository Development Guidelines

[Simplified Chinese](docs/zh-CN/AGENTS.md)

This repository develops and maintains Markdown Loop templates. The following guidelines apply to changes in this project.

- Keep templates in templates and usage documentation in docs. Each template must be independently copyable and usable without relying on files outside its directory.
- Keep LOOP CONTRACT (Goal, Boundary, SOP) and STATE + LOG (State, Logs) in LOOP.md.
- Implement only the necessary workflow. Do not introduce runtime programs, scheduling code, or additional configuration systems.
- Keep only essential configuration options and define state for each specific Loop. CONFIG and STATE contain data only; leave initial state and logs empty.
- Keep Starter as a generic placeholder without predefined business workflows or state fields.
- After making changes, check links, JSON, and documentation consistency.

.local contains local materials. Do not read, reference, or commit it by default.
