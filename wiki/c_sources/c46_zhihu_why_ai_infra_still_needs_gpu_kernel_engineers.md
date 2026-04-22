# 知乎回答：为什么 AI Infra 里还在大量招聘 GPU Kernel Engineer

## 记录时间

- 2026-04-22 CST

## 原始讨论对象

- 知乎问题：《为什么 AI Infra 里还在大量招聘 GPU Kernel Engineer？》
- 目标回答作者：`匿名用户`
- 来源链接：https://www.zhihu.com/question/1998102573333385508/answer/1998357017128093045
- 回答 ID：`1998357017128093045`

## 正文获取情况

- 当前环境直接请求知乎回答 API 会返回 `40362` 风控，不能依赖公开接口直接获取全文。
- 本次按 she_wiki 的“强前端反爬内容获取”流程，使用真实浏览器渲染页面，再通过 CDP `Runtime.evaluate` 读取回答正文。
- 目标回答正文已完整拿到，正文从“很多人都有这个疑问”起，完整覆盖：
  - 通用库与长尾 shape 的矛盾
  - memory wall
  - 算子融合
  - 编译器自动生成的边界
  - 量化推理与 `cp.async`
  - profiling 驱动协作
  - 算法-硬件协同设计
  - 架构迭代与岗位稀缺性
  - 护城河与职业布局
  - CUTLASS 二次开发边界
  - `MoE` 中 `gather/scatter` 融合场景
- 页面主体是长文本回答，没有依赖关键图片才能成立的核心论证。

## 原始信息

- 回答先反驳一种表面直觉：
  - 既然已有 `cuBLAS`、`CUTLASS`、`Triton`、Inductor
  - 为什么还要养高薪 kernel 团队
- 作者给出的第一层答案是商业账而不是工具账：
  - 官方库解决的是通用性
  - 生产场景常有固定且极端的 shape
  - 在万卡集群上，某些关键 kernel 提升 `1% ~ 30%` 都足以覆盖团队成本
- 回答认为通用库的结构性限制在于：
  - 必须兼容大量尺寸和 workload
  - 无法为某个特定模型、固定隐藏维度、固定 batch/seq 组合写死参数
  - 因而常常只能给出“足够好”的平均表现
- 在大模型训练和推理中，作者强调真正的瓶颈经常不是 GEMM 算得不够快，而是：
  - `HBM -> SRAM -> register` 的数据搬运
  - `cast / transpose / add / norm / activation` 这些前后处理带来的额外读写
- 回答把这条主线收束为 `memory wall`：
  - GPU 算力增长速度快于带宽增长
  - 许多 kernel 的真实上限先被带宽而不是算术吞吐卡住
- 因此 kernel engineer 的关键工作之一不是“重写矩阵乘法”，而是：
  - 针对特定数据流做算子融合
  - 让数据读入寄存器或 shared memory 后尽量一次算完
  - 减少中间结果反复写回显存
- 对编译器路线，回答的判断较克制：
  - 自动代码生成上限很高
  - 但搜索空间巨大、成本模型不准、结果稳定性差
  - 在关键路径上，仍会差最后一口气
- 作者认为这类差距常出现在微观硬件事实上：
  - 多几个寄存器可能让 occupancy 掉下去
  - 某些访存指令组合在特定架构上才吃满带宽
  - 调试自动生成代码通常比手写代码更难建立可控反馈回路
- 量化推理是回答重点举的场景：
  - `W8A8`、`W4A16` 等混合精度需要解压权重、处理 scaling factor、适配特殊内存布局
  - 现有编译器在这类 workload 上容易退化
  - 资深工程师会结合 `cp.async`、software pipeline、双缓冲等手工管理数据搬运与计算重叠
- 协作模式上，回答反对“岗位细分成纯写代码/纯测试”的想象：
  - 团队通常由 profiling 驱动
  - 先找 Pareto 意义上的大热点
  - 再由不同资历的人分别攻坚核心 attention、融合算子、框架集成、数值校验与回归
- 回答特别强调 kernel engineer 不是孤立岗位：
  - 需要与算法团队讨论某个结构在硬件上是否友好
  - 有时要把理论上 FLOPs 更低的方案，改成在 GPU 上更快的等价实现
  - 高级形态是算法-硬件协同设计
- 对岗位不会饱和的原因，作者给出三条持续驱动：
  - GPU 架构持续迭代
  - 模型结构持续变化
  - 摩尔定律放缓，必须靠更精细的软件优化追性能
- 护城河部分，回答把能力分成三层：
  - 体系结构理解：内存层级、cache line、warp 调度、指令流水
  - 数值敏感度：混合精度、累加器溢出、加法顺序改变结果
  - 工程能力：模板代码组织、API 设计、测试与回归
- 回答后半段继续补充 CUTLASS 的边界：
  - CUTLASS 更像表达能力很强的模板积木
  - 深度跨算子融合、特殊 reduce、定制 iterator 往往仍需二次开发甚至重写 main loop
  - 当模型参数固定时，人工 hardcode 某些常量有时能换到寄存器和指令层面的真实收益
- 在 `MoE` 案例里，回答用 `gather / scatter / top-k` 路由说明：
  - 动态索引和不规则数据搬运非常容易把显存带宽打碎
  - 需要通过专门的 fused kernel、片上重排、block mapping、原子操作策略来重新组织数据流
- 最后，回答把长期职业价值收束到“跨栈理解”上：
  - 不只会写 CUDA
  - 还要判断该不该融合、放 CPU/GPU 哪边、瓶颈是 compute/memory/latency 哪一种
  - 系统级 dataflow 视角与微观硬件 insight 的结合，比单点手工技巧更稳

## 原始结论

- 这篇回答对 she_wiki 的稳定价值，不在某个具体库“是否过时”，而在于它说明了为什么 GPU kernel 岗位在 AI Infra 中仍长期稀缺：
  - 通用库服务平均情况
  - 生产优化常发生在固定 shape、特殊数据流和高成本集群的长尾场景
  - memory wall 与算子融合让手工优化继续有明确商业回报
- 更稳定的结论有四条：
  - 现代 GPU 优化里，很多关键收益先来自减少数据搬运，而不是单独抠算术吞吐
  - 自动编译与代码生成会越来越重要，但在特殊 workload 和关键路径上仍常需要人工兜底
  - 真正稀缺的能力不是“会调库”，而是能把体系结构、数值稳定性和系统 dataflow 连起来看
  - 岗位需求的持续性来自硬件更新、模型演化和成本压力三者叠加，而不是一时风口
- 这篇回答更适合作为“GPU Kernel Engineer 角色价值与护城河”来源页，而不是 CUDA 原理页。

## 相关页面

- [GPU Kernel Engineer 岗位的长期价值与护城河](../b_extractions/b5_technical_judgment/b56_gpu_kernel_engineer_role_and_moat.md)
- [CUDA/NVIDIA 算子性能优化骨架](../b_extractions/b2_technical_fundamentals/b25_cuda_operator_performance_optimization_skeleton.md)
- [性能优化要优先处理 IO、布局与减法空间](../b_extractions/b5_technical_judgment/b55_performance_optimization_judgment_order.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [知乎全文获取流程](./c9_zhihu_full_content_retrieval.md)
