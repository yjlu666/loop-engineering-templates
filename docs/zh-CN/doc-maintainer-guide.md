# Doc Maintainer Loop 使用指南

[English](../doc-maintainer-guide.md)

## 三步开始

1. 复制 `templates/doc-maintainer/` 整个目录到自己的工作位置。
2. 在副本的 `CONFIG.md` 中填写官方仓库、个人 fork 和需要维护的文档路径。
3. 告诉本地 Agent：`读取 <实例目录>/LOOP.md 并执行一轮`。下次仍指定同一个文件，接续已有进度。

## 配置

| 字段 | 默认值 | 填写方式 |
|---|---|---|
| `upstream_repo` | `""` | 必填，官方仓库 URL，扫描其已合并到主分支的 PR 并向它提交 PR |
| `fork_repo` | `""` | 必填，个人 fork 的仓库 URL，用于推送文档更新分支 |
| `base_branch` | `"main"` | 主分支名，可不改 |
| `documents` | `[]` | 必填，官方仓库内的文档路径列表，如 `["README.md", "docs/api.md"]` |

## 执行方式

首次运行（`STATE.md` 为空）只扫描最近 24 小时合入主分支的 PR；之后每轮从 `STATE.md` 记录的 `last_merged_pr` 之后继续，只处理编号更大的已合并 PR。Agent 把 PR 的代码变更与 `documents` 中每份文档逐项对比：

- 发现偏差：从官方仓库主分支最新提交建立 worktree 和分支，对文档做最小修改，推送到 `fork_repo`，向官方仓库提交 1 个 PR 且不合并。
- 没有偏差：不建分支、不提 PR，只记录本轮结果。

每轮结果写入实例目录下的 `reports/<时间>.md`（供你查看，Agent 不回读），随后把本轮扫描到的最大 PR 编号写入 `STATE.md`。clone 缓存在系统用户缓存目录的 `loop-engineering-templates/` 下，worktree 用完即移除。

需要定时运行时，在本地 Agent 中设置对同一个 `LOOP.md` 的调用。
