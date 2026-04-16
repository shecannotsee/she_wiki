# 系统设计类

## 包含的知识提取页

- [多Agent系统工程化](../b_extractions/b4_system_design/b41_multi_agent_system_engineering.md)
- [反馈链路可靠性](../b_extractions/b4_system_design/b42_feedback_loop_reliability.md)
- [Agent 长期记忆系统设计](../b_extractions/b4_system_design/b43_agent_long_term_memory_systems.md)
- [线程池的最小系统骨架](../b_extractions/b4_system_design/b44_thread_pool_minimal_architecture.md)
- [DDIA 分布式系统设计基础框架](../b_extractions/b4_system_design/b45_ddia_distributed_system_design_foundations.md)
- [游戏服务端架构的类型分化与演进](../b_extractions/b4_system_design/b46_game_server_architecture_evolution.md)

## 这一类的共同点

- 关注系统如何搭骨架、控风险、保收敛
- 关注关键控制环路是否可验证、可维护、可持续运行
- 也关注组件是否定义了清晰的任务模型、状态语义和控制边界
- 也关注系统骨架是否真正匹配业务交互形态、世界模型和负载现实

## 核心总结

- 系统设计里，协议、锁、评估器、停止条件的优先级高于角色包装和智能想象
- 关键反馈链路如果不可验证，系统看起来在闭环，实际上仍可能失控
- Agent 的长期记忆系统要按时间尺度和用途分层，并把检索目标从“找相似历史”提升到“服务当前动作”
- 线程池这类基础组件的关键，不是把任务跑起来，而是先定义提交、返回、休眠和停机这几类系统承诺
- 分布式系统设计要先定义一致性和故障边界，再讨论复制、分片、事务与批流处理的具体方案
- 游戏服务端设计要先按交互强度、世界连续性、同步压力和全区/分服形态选骨架，再决定拆分层级与中间件
