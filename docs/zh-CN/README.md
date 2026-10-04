# Loop Engineering 模板

[English](../../README.md)

**Loop Engineering Templates**：一组可独立复制使用的 Markdown Loop 实践模板。

模板定义目标、边界、操作步骤，以及需要保存的状态和日志。本地 Agent 读取模板，在你的项目或工作目录中完成每轮任务。

## 模板

| 模板 | 用途 | 使用指南 |
|---|---|---|
| [GitHub Maintainer](../../templates/github-maintainer/) | 跟进 GitHub Issue/PR，按仓库规则回复、合并并汇总通知 | [使用与配置](github-maintainer-guide.md) |
| [Doc Maintainer](../../templates/doc-maintainer/) | 对比已合并 PR 的代码变更与指定文档，发现偏差时提交文档更新 PR | [使用与配置](doc-maintainer-guide.md) |
| [Project Development](../../templates/project-development/) | 按需串联立项到生产部署，支持环节委派、代码修复回退和循环次数上限 | [使用与配置](project-development-guide.md) |
| [Starter](../../templates/starter/) | 编写新 Loop 的通用空白模板 | [模板编写指南](template-authoring.md) |

## 使用现成模板

1. 将所需模板的整个目录复制到自己的项目或工作目录。
2. 按对应指南填写副本中的 `CONFIG.md`，如有需要，补全所需文件。
3. 告诉本地 Agent：`读取 <模板副本目录>/LOOP.md 并执行一轮`。

需要定时执行时，在本地 Agent 中设置。

## 贡献与许可

欢迎新增模板、改进流程或完善文档，参见[贡献指南](CONTRIBUTING.md)。本项目采用 [MIT License](../../LICENSE)。
