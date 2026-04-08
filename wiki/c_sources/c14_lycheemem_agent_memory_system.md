# LycheeMem 项目梳理

## 记录时间

- 2026-04-08 CST

## 原始讨论对象

- GitHub 仓库：https://github.com/LycheeMem/LycheeMem
- README：https://github.com/LycheeMem/LycheeMem/blob/master/README.md
- 项目主页：https://lycheemem.github.io/
- 打包与依赖配置：https://github.com/LycheeMem/LycheeMem/blob/master/pyproject.toml
- 环境变量示例：https://github.com/LycheeMem/LycheeMem/blob/master/.env.example

## 原始信息

- 项目定位：
  - `LycheeMem` 是给 LLM Agent 使用的长期记忆基础设施
  - 目标不是简单保存聊天记录，而是把对话整理成可检索、可复用、可面向行动的记忆
- 记忆分层：
  - `Working Memory` 保存当前会话活跃上下文，并在 token 预算接近阈值时压缩旧轮次
  - `Semantic Memory` 保存类型化长期记忆，例如 `fact`、`preference`、`constraint`、`procedure`、`failure_pattern`
  - `Procedural Memory` 或 `Skill Store` 保存可复用的做事方法，并为技能检索使用 `HyDE`
- 主流水线：
  1. `WMManager` 管理工作记忆和压缩
  2. `SearchCoordinator` 基于当前动作状态规划检索
  3. `SynthesizerAgent` 对检索结果做判断、筛选和融合
  4. `ReasoningAgent` 使用工作记忆与融合背景生成回答
  5. `ConsolidatorAgent` 在后台异步做长期记忆固化与融合
- 检索策略：
  - 不是单一 embedding 相似度检索
  - 结合 `SQLite FTS5`、`LanceDB` 向量召回、tag 检索、temporal filter、missing slots、composite expansion 等多路信号
  - 先由 planner 生成 `SearchPlan`，再召回和重排
- 排序目标：
  - 同时考虑 `semantic relevance`、`action utility`、`slot utility`、`temporal fit`、`recency`、`evidence density`、`token cost penalty`
  - 说明该系统面向的是“帮助 Agent 做下一步动作”，而不只是“找相似历史”
- 存储与部署：
  - README 明确说明当前实现已从 `Neo4j` 收敛到 `SQLite + LanceDB`
  - 这使部署更轻量，也更适合作为 Agent 的可嵌入记忆后端
- 接入方式：
  - 提供 `FastAPI` 服务与分阶段 API，例如 `/memory/search`、`/memory/synthesize`、`/memory/reason`
  - 提供 `MCP` 接口，支持 JSON-RPC 与 SSE
  - 提供 `OpenClaw` 插件和 `web-demo`
- 工程实现：
  - 后端主要使用 `FastAPI`、`LangGraph`、`litellm`、`lancedb`
  - 前端 demo 使用 `React`、`Vite`、`zustand`、`d3`
- 值得注意的工程细节：
  - 长期记忆固化是后台异步执行，避免阻塞主回答链路
  - 有 novelty check，避免每轮都把重复内容写入长期记忆
  - 有针对敏感信息的模式检测，避免把明显的密钥或私密信息固化到长期记忆

## 原始结论

- 一个可用的 Agent memory 系统，关键不在“存更多历史”，而在“把记忆转化为面向动作的结构化上下文”
- 长期记忆系统应与主回答链路解耦，否则写入与融合成本会直接拖慢交互延迟
- 记忆检索不应只按语义相似度排序，还应纳入当前动作、缺失槽位、时间约束和 token 成本
- 工程上，轻量存储、分阶段 API 和宿主集成接口，往往比炫技型图结构更决定项目是否可落地

## 原始注意点

- README 写的是 `Python 3.9+`，但 `pyproject.toml` 中 `requires-python` 为 `>=3.11`，实际使用时应按 `Python 3.11+` 准备环境
- 仓库中仍保留部分 `graph` 命名，但 README 说明当前底层已不再依赖 `Neo4j`

## 相关页面

- [Agent 长期记忆系统设计](../b_extractions/b4_system_design/b43_agent_long_term_memory_systems.md)
- [多Agent系统工程化](../b_extractions/b4_system_design/b41_multi_agent_system_engineering.md)
