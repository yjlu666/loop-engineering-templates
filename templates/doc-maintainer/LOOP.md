# Doc Maintainer Loop

配置：[CONFIG.md](CONFIG.md)

## LOOP CONTRACT

### Goal

让 [CONFIG.md](CONFIG.md) 列出的文档与官方仓库已合并到主分支的代码保持一致：本轮完成 1 次增量扫描，产出 1 份运行报告；有偏差时额外提交 1 个文档更新 PR（不合并）。

### Boundary

只处理 [CONFIG.md](CONFIG.md) 指定的官方仓库、个人 fork 和文档路径，使用现有 GitHub 凭据。只改文档，不改代码，不改未列出的文件。不合并任何 PR，不改用户已有 checkout，每次执行一轮。reports/ 下的报告面向用户，Agent 不回读。

### SOP

1. 读取配置和 [STATE.md](STATE.md)；需要排查异常时再查 [LOGS.md](LOGS.md)，不读 reports/ 历史报告。
2. 用 GitHub API 分页列出官方仓库 base_branch 上已合并的 PR，按合入时间升序取编号大于游标的那些；游标为空或为 0（首次运行）时只取最近 24 小时合入的 PR。记录扫描到的最大 PR 编号。
3. 没有新增 PR 时跳过 4–5 步，直接进入 6。
4. 逐个读取 PR 的代码变更（diff，必要时读相关文件），与配置中的每份文档内容对比，列出偏差：文档未覆盖的新行为、已失效的旧描述、与实现矛盾的示例。
5. 存在偏差时，在缓存 clone 中 fetch 官方仓库 base_branch 最新提交，从该提交建立新的 worktree 和分支 `docs/sync-<日期>`；在工作树内只对文档做最小修改，提交并推送分支到 fork_repo，再向官方仓库提交 1 个 PR（标题说明文档同步，正文列出依据的 PR 编号和改动文档），不合并。没有偏差时不建分支、不提 PR。
6. 将本轮结果写入 reports/<时间>.md：扫描的 PR 编号范围、每份文档的对比结论、PR 链接。把本轮扫描到的最大已合并 PR 编号写入 STATE。
7. worktree 用完移除；clone 放在系统用户缓存目录的 loop-engineering-templates/ 下，按仓库隔离，留待下轮复用。

## STATE + LOG

### State

[STATE.md](STATE.md) 只保存“最后已处理的已合并 PR 编号”，字段为 `last_merged_pr`；为空时按首次运行处理。reports/ 下的报告供用户查看，Agent 不回读。

### Logs

[LOGS.md](LOGS.md) 只记录实际遇到的问题和解决办法。
