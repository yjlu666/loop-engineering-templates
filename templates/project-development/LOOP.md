# Project Development Loop

配置：[CONFIG.md](CONFIG.md)

## LOOP CONTRACT

### Goal

完成用户提出的需求，并确保用户指定的所有环节都通过。达到最大循环次数仍未全部通过时，向用户报告未通过的环节、具体问题和已尝试的处理，不得假装完成。

### Boundary

- 只处理本 Loop 模板所在的项目，遵守用户授权和项目约定。
- 仅按下表顺序执行 `enabled: true` 的环节，跳过禁用环节及其 skill。具体做法由用户填写在 `skill/<环节 ID>/SKILL.md` 正文中，脚本等资源放在对应 skill 目录内，资源路径以该目录为基准；所需做法或输入缺失时，报告用户并等待。
- 代码开发之后的环节因需要修复代码而回到代码开发，才算一次循环。最多进行 `max_iterations` 次循环；达到上限仍未解决时，停止并报告用户。

### SOP

1. 读取 CONFIG。确认 `max_iterations` 为正整数，`stages` 按下表顺序保留全部环节，每项 `enabled` 和 `use_subagent` 为布尔值，且至少启用一个环节；否则报告用户并等待。从首个已启用环节开始处理当前用户需求。

   | 顺序 | 环节 ID | 环节 | 做法文件 |
   |---|---|---|---|
   | 1 | `project-initiation` | 立项 | [skill/project-initiation/SKILL.md](skill/project-initiation/SKILL.md) |
   | 2 | `requirements-clarification` | 需求澄清 | [skill/requirements-clarification/SKILL.md](skill/requirements-clarification/SKILL.md) |
   | 3 | `requirements-document` | 需求文档生成 | [skill/requirements-document/SKILL.md](skill/requirements-document/SKILL.md) |
   | 4 | `architecture-document` | 架构文档生成 | [skill/architecture-document/SKILL.md](skill/architecture-document/SKILL.md) |
   | 5 | `technical-design` | 技术方案生成 | [skill/technical-design/SKILL.md](skill/technical-design/SKILL.md) |
   | 6 | `code-development` | 代码开发 | [skill/code-development/SKILL.md](skill/code-development/SKILL.md) |
   | 7 | `code-review` | 代码审查 | [skill/code-review/SKILL.md](skill/code-review/SKILL.md) |
   | 8 | `test-deployment` | 部署到测试环境 | [skill/test-deployment/SKILL.md](skill/test-deployment/SKILL.md) |
   | 9 | `test-acceptance` | 测试环境验收 | [skill/test-acceptance/SKILL.md](skill/test-acceptance/SKILL.md) |
   | 10 | `production-deployment` | 部署到生产环境 | [skill/production-deployment/SKILL.md](skill/production-deployment/SKILL.md) |

2. 按当前环节的 `use_subagent` 执行一次，并返回结果、产物位置、判断依据和待处理问题。
   - `false`：主 Agent 读取对应 skill 并执行。skill 明确要求某些动作用 subagent 时，按要求派发。
   - `true`：主 Agent 不读取对应 skill，将其绝对路径、项目目录、用户需求、已有产物和待处理问题交给 subagent，要求其自行读取并执行当前环节，遵守本 Loop 的边界和结果规则。主 Agent 等待并只依据返回结果决定下一步。
   - subagent 不推进其他环节、不修改日志；skill 中明确要求的局部委派仍须执行。
3. 主 Agent 根据本次结果决定下一步：
   - `passed`：达到 skill 的通过标准，进入下一个已启用环节并回到第 2 步。用户需求完成且全部已启用环节通过，才报告完成及产物并结束。
   - `retry_stage`：当前环节尚未通过，有可行的本环节处理办法；保留问题，回到第 2 步重试。代码开发之后的环节仅在不需要改代码时使用此结果。无法自行处理则返回 `blocked`。
   - `fix_code`：代码开发之后的环节需要改代码，当前执行者停止，不在该环节修复。代码开发及之后的原通过结论失效，带上问题返回代码开发并按第 2 步执行；修复后按顺序重跑全部后续已启用环节。代码开发未启用时，报告用户并等待处理，不自行启用环节。
   - `blocked`：做法、输入或能力不足，无法自行处理，或无法判断下一步；报告具体问题并等待用户处理。

## STATE + LOG

### State

每次执行只处理当前用户需求，不保存跨次执行状态。

### Logs

[LOGS.md](LOGS.md) 初始留空，只记录可复用的通用问题和已验证的解决方法，不记录具体需求的问题或执行过程。
