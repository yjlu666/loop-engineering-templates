# Project Development Loop 使用指南

[English](../project-development-guide.md)

## 开始使用

1. 复制 [templates/project-development/](../../templates/project-development/) 整个目录到要开发的项目下，包括 `skill/`。
2. 填写副本的 `CONFIG.md`，通过各环节的 `enabled` 选择是否启用，保留完整列表和原顺序。
3. 填写已启用环节的 `skill/<环节 ID>/SKILL.md` 正文，说明输入、具体做法、产物和通过标准。各 `SKILL.md` 仅含规范要求的 `name` 和 `description`，具体做法留空，禁用环节可不填写。
4. 向 Agent 提出具体需求，并告诉它：`读取 <实例目录>/LOOP.md 并执行，直到完成、需要用户处理或达到循环上限。`

## Skill 目录

每个环节使用独立目录，以 `SKILL.md` 为入口。需要脚本、参考资料或资源文件时，在该目录内按需添加，并从 `SKILL.md` 引用；路径以该 skill 目录为基准。

```text
skill/<环节 ID>/
├── SKILL.md
├── scripts/       # 按需添加脚本
├── references/    # 按需添加参考资料
└── assets/        # 按需添加资源文件
```

模板只提供 `SKILL.md`，不预置具体做法或脚本。

## 配置

配置独立保存在 [CONFIG.md](../../templates/project-development/CONFIG.md)，只包含以下数据：

| 字段 | 默认值 | 用途 |
|---|---|---|
| `max_iterations` | `30` | 正整数，因代码修复而从后续环节返回代码开发的最大循环次数 |
| `stages` | 全部 10 个环节 | 保留完整列表和顺序，通过 `enabled` 选择环节 |
| `stages[].id` | 见 LOOP 中的环节表 | 环节 ID，对应 `skill/<id>/SKILL.md` |
| `stages[].enabled` | 全部为 `true` | 是否启用环节，至少启用一个；`false` 时跳过该环节及其 skill |
| `stages[].use_subagent` | 全部为 `false` | 是否将整个环节交给 subagent |

固定顺序：立项 → 需求澄清 → 需求文档生成 → 架构文档生成 → 技术方案生成 → 代码开发 → 代码审查 → 部署到测试环境 → 测试环境验收 → 部署到生产环境。可按需启用其中一个或多个环节，跳过环节所需的前置输入由用户提供。

`use_subagent: true` 时，主 Agent 不读取该环节的 skill，只交给 subagent 自行读取执行，再依据返回结果判断下一步。保持 `false` 时由主 Agent 执行；skill 内明确要求某些动作使用 subagent，仍按要求派发。

## 回退与停止

- 环节通过后进入下一个已启用环节；用户需求完成且所有已启用环节都通过，才算完成。
- 代码开发后的环节需要改代码时，返回代码开发，修复后依序重跑全部后续已启用环节。代码开发未启用则报告用户并等待处理。
- 不需要改代码的问题留在当前环节处理，处理不了则报告用户并等待。
- 只有代码开发后的环节因需要修复代码而回到代码开发，才算一次循环。达到 `max_iterations` 上限仍未全部通过时，停止并报告未通过的环节、具体问题和已尝试的处理，不得假装完成。

每次执行只处理当前用户需求，不保存跨次执行状态。下次提出新需求时，从首个已启用环节重新开始。`LOGS.md` 初始无记录，只记录可复用的通用问题和已验证的解决方法，不记录具体需求的问题或执行过程。
