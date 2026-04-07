# 多Agent团队模式梳理

## 记录时间

- 2026-03-04 19:22:45 CST

## 原始信息

- 核心结论：
  - 本地 Git 即可启动
  - 推荐“业务仓库 + Agent 平台仓库”双仓结构
  - 本质是多会话/多进程 + 任务池 + 消息总线 + 原子任务认领 + 质量门禁
- 角色：
  - Lead / Orchestrator
  - Planner
  - Builder
  - Reviewer
- MVP：
  - 任务池
  - Mailbox
  - 原子抢任务
  - 评估器
  - 停止条件
- 记忆：
  - short_term
  - episodic
  - semantic
- 方法论：
  - 先可控再智能
  - 先协议再实现
  - 先隔离再协作
  - 先评估再迭代
