# GitHub Maintainer Loop 使用指南

[English](../github-maintainer-guide.md)

## 三步开始

1. 复制 `templates/github-maintainer/` 整个目录到自己的工作位置。
2. 在副本的 `CONFIG.md` 中填写仓库列表、通知应用和接收会话；缓存参数可保留默认值。
3. 告诉本地 Agent：`读取 <实例目录>/LOOP.md 并执行一轮`。下次仍指定同一个文件，接续已有进度。

## 配置

| 字段 | 默认值 | 填写方式 |
|---|---|---|
| `repositories` | `[]` | 必填，填写一个或多个 GitHub 仓库 URL |
| `notify_app` | `""` | 必填，填写本地通讯应用名称，如飞书或微信 |
| `notify_chat` | `""` | 必填，填写接收总结的确切会话名称 |
| `cache_ttl_days` | `7` | 源码缓存闲置天数，可不改 |
| `max_cache_gib` | `20` | 所有仓库源码缓存的合计容量目标，单位 GiB，可不改 |

首次只纳入最近 24 小时新增的 Issue/PR，之后增量发现并跟进已纳管项。Agent 自动识别仓库规则与缓存路径，保留现有审批，仅在检查与审查通过时合并小 PR；缓存默认闲置 7 天回收、总容量 20 GiB。

## 跟进状态

`TRACKING.md` 是未结束 Issue/PR 的清单，Agent 每轮只读它和 `STATE.md` 来继续跟进，并按清单逐条更新状态与下一步。`reports/` 下的报告供你查看，Agent 不回读，避免历史报告污染上下文。升级已有实例时手动建立该文件即可，Agent 会在首轮从 GitHub 上仍打开且本 Loop 已参与的条目重建清单。

主 Agent 按仓库分派 subagent 并发分析，为每个 subagent 指定绝对仓库路径；宿主支持工作目录选项时设置该目录，否则在工具调用中显式指定路径。不支持 subagent 时顺序执行。

所有仓库分析结束后，由主 Agent 统一处置，并仅通过 Computer Use 发送一条汇总。

需要定时运行时，在本地 Agent 中设置对同一个 `LOOP.md` 的调用。
