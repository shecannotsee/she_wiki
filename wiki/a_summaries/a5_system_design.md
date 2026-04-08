# 系统设计类

## 包含的知识提取页

- [多Agent系统工程化](../b_extractions/b4_system_design/b41_multi_agent_system_engineering.md)
- [反馈链路可靠性](../b_extractions/b4_system_design/b42_feedback_loop_reliability.md)
- [Agent 长期记忆系统设计](../b_extractions/b4_system_design/b43_agent_long_term_memory_systems.md)

## 这一类的共同点

- 关注系统如何搭骨架、控风险、保收敛
- 关注关键控制环路是否可验证、可维护、可持续运行

## 核心总结

- 系统设计里，协议、锁、评估器、停止条件的优先级高于角色包装和智能想象
- 关键反馈链路如果不可验证，系统看起来在闭环，实际上仍可能失控
- Agent 的长期记忆系统要按时间尺度和用途分层，并把检索目标从“找相似历史”提升到“服务当前动作”
