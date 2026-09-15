# GitHub Maintainer Loop

## LOOP CONTRACT

### Goal

处理本轮全部新增及待跟进的 Issue/PR，生成 1 份总结，并向指定会话发送 1 条汇总消息。

### Boundary

只处理 [CONFIG.md](CONFIG.md) 指定的仓库和通知目标，遵守 [POLICY.md](POLICY.md) 与仓库现有审批规则。不实现 Issue，不自动关闭条目，不改用户已有 checkout。每次执行一轮。reports/ 下的报告面向用户，Agent 不回读。

### SOP

1. 读取配置、[STATE.md](STATE.md) 和 [TRACKING.md](TRACKING.md)；需要排查异常时再查 [LOGS.md](LOGS.md)，不读 reports/ 历史报告。
2. 每个仓库分派一个 subagent 并发检查，给出仓库 URL 和独立工作目录的绝对路径；工具操作显式指定目录。不支持 subagent 时顺序执行。
3. 通过 GitHub API 分页扫描 Issue/PR：没有游标时只处理最近 24 小时新增的条目，之后处理编号大于游标的条目。以 [TRACKING.md](TRACKING.md) 清单为跟进依据，逐条核对并继续处理尚未结束的条目；Issue 关闭、PR 关闭或合并后从清单移除。
4. 优先读取 diff 和相关文件，需要完整源码时再 clone/fetch。源码放在系统用户缓存目录的 loop-engineering-templates/ 下，按仓库隔离；使用后更新缓存目录时间。PR 分析使用对应提交的临时 worktree，用完移除，clone 留待下轮复用。
5. 等待所有仓库分析完成，主 Agent 按 POLICY 统一回复和合并。
6. 主 Agent 将各仓库本次扫描到的最大编号写入 STATE；回收闲置超过 cache_ttl_days 的缓存，总容量超过 max_cache_gib 时优先回收最久未使用的缓存。
7. 将本轮结果、条目链接和清单变化写入 reports/<时间>.md（供用户查看，Agent 不回读），并更新 TRACKING.md；主 Agent 通过 Computer Use 向指定会话发送一条全仓库汇总。

## STATE + LOG

### State

[STATE.md](STATE.md) 只保存“仓库 → 最后扫描编号”的 JSON 映射。Issue 与 PR 共用编号；空仓库记为 0。[TRACKING.md](TRACKING.md) 保存未结束条目清单和每条的下轮动作，是跟进状态的唯一来源。

### Logs

[LOGS.md](LOGS.md) 只记录实际遇到的问题和解决办法。
