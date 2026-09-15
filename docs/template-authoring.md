# 编写新的 Loop

`templates/starter/` 是新 Loop 的空白模板。复制后，把具体任务写进去，再交给 Agent 执行。

## 文件怎么用

| 文件 | 填什么 |
|---|---|
| LOOP.md | 你填写：本轮目标、操作边界和具体步骤 |
| CONFIG.md | 你按需填写：本 Loop 的可配置参数 |
| STATE.md | Agent 按 Loop 定义保存必要状态|
| LOGS.md | Agent 按 Loop 定义记录异常、经验和解决办法 |

Loop 需要 Agent 跨轮传递清单（如未结束的条目）时，另建一个 md 文件保存，并在 LOOP.md 的 State 中说明；面向用户的报告只供人查看，不让 Agent 回读。

## 三步开始

1. 复制 `templates/starter/`，改成自己的模板目录名。
2. 替换 LOOP.md 中的 `【…】`，定义目标、约束、步骤，以及要保存的状态和日志；按需填写 CONFIG.md。
3. 告诉本地 Agent：`读取 <模板目录>/LOOP.md 并执行一轮`。
