# 仓库开发约定

本仓库用于开发和维护 Markdown Loop 模板。以下约定适用于本项目的修改工作。

- 模板放 templates，使用说明放 docs；每个模板须能独立复制使用，不依赖仓库外层文件。
- LOOP.md 保留 LOOP CONTRACT（Goal、Boundary、SOP）和 STATE + LOG（State、Logs）。
- 只实现必要流程，不引入运行程序、调度代码或额外配置体系。
- 配置只留必要项，状态按具体 Loop 定义；CONFIG、STATE 只存数据，初始状态与日志留空。
- Starter 保持通用占位，不预设业务流程或状态字段。
- 修改后检查链接、JSON 和文档一致性。

.local 是本地资料，默认不读、不引用、不提交。
