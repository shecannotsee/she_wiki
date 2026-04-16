# 知乎专栏：NVIDIA GPU性能优化基础

## 记录时间

- 2026-04-16 CST

## 原始讨论对象

- 知乎专栏文章：《NVIDIA GPU性能优化基础》
- 目标作者：`起名困难症`
- 来源链接：https://zhuanlan.zhihu.com/p/577412348
- 文章 ID：`577412348`
- 文章发布时间：2022-11-15 18:43:27 CST
- 文章更新时间：2022-11-16 19:02:47 CST

## 正文获取情况

- 当前环境直接访问知乎专栏原页时，静态 HTML 只能拿到反爬壳页面，不能直接抽取正文。
- 专栏接口在当前环境下不可稳定获取，不能作为可靠全文来源。
- 本次按 she_wiki 的“强前端反爬内容获取”流程，使用真实浏览器渲染 + CDP `Runtime.evaluate` 读取 `.Post-RichText`，拿到完整章节正文。
- 页面 `meta` 中可交叉验证标题、发布时间、更新时间和作者信息，正文节点与目录结构一致。

## 原始信息

- 文章目标很明确：
  - 给出 NVIDIA GPU 性能优化的基础知识骨架
  - 把 `SM structure`、`memory hierarchy`、`execution model` 和 `nsight compute` 放进同一个体系里
- 文章先从 `SM` 结构入手：
  - 以 Pascal GP100 为例解释 `GPC -> TPC -> SM` 的分层结构
  - 说明 GPU 算力最终可被拆成“单个 SM 的能力 × SM 数量”
  - 强调计算单元、调度单元、寄存器组和访存单元的职责分工
- 在单个 SM 内部，文章强调：
  - 不同功能单元数量和吞吐不一样
  - 理论算力需要按 `主频 × 计算单元数量 × 每周期操作数` 去算
  - Tensor Core 的引入会显著抬高计算峰值，但并不会同步抬高 memory bandwidth
- 文章对 Tensor Core 的落点不是“它更快”这么简单，而是：
  - Tensor Core 把现代 workload 更明显地推向 `memory bound`
  - 所以现代 GPU 开发里，memory 访问优化的重要性比以往更高
- 在 `memory hierarchy` 一章，文章给出几个稳定概念：
  - `request` 是一条 memory access instruction
  - `transaction` 是不同 memory 区域之间的一次单元数据搬运
  - 高效访存的本质，是减少每次 request 平均需要的 transaction 数量
- 对 `global memory`，文章重点解释：
  - `sector` 粒度
  - 合并访存为什么能减少 transaction
  - strided 访问为什么会浪费带宽
  - 对齐与向量化加载如何进一步改善带宽利用率
- 对 `shared memory`，文章重点解释：
  - bank 组织方式
  - bank conflict 为什么会把一个 request 拆成多个串行请求
  - broadcast 场景为什么不一定冲突
- 对 `local memory`，文章强调一个容易被忽略的点：
  - 它本质上是放在 global memory 里的 thread-local 空间
  - 动态索引数组、register 不够导致的 spilling、大结构体等都可能把数据打到 local memory
  - 这会同时增加访存量和指令量
- 对 `cache`，文章说明：
  - `L2` 是全局共享
  - `L1` 往往是每个 SM 私有
  - 不同 compute capability 下 `global memory` 是否默认经过 `L1` 有细节差异
- 在 `execution model` 一章，文章建立了几个很稳定的概念层级：
  - thread 被组织进 warp
  - warp 被组织在 block 中
  - block 被调度到 SM 上运行
  - 性能问题要分别看 thread、warp、block 这几个层面
- 对 thread 级执行，文章强调：
  - GPU 本质上仍是 SIMD/SIMT
  - 一个 warp 的操作未必能在一个 cycle 内做完
  - 资源不足时要分多拍完成
  - 所以“warp-wide 指令”与“单周期完成”不是一回事
- 文章把 warp 级低效拆成两个常见来源：
  - 分支 `divergence`
  - memory divergence 导致的 `instruction replay`
- 在 warp / block / SM 调度层，文章给出几个关键概念：
  - active warp
  - eligible warp
  - stall warp
  - selected warp
  - tail effect
  - occupancy
- 关于 `occupancy`，文章强调：
  - register、shared memory、block size 都会约束理论 occupancy
  - achieved occupancy 又会受到负载不平衡和 launch 规模等运行时因素影响
  - 所以 occupancy 不是越高越好，而是要和真实 bottleneck 放在一起看
- 在 profiling 部分，文章把 `Nsight Compute` 的价值说得很具体：
  - `memory workload analysis` 用来看 request、transaction、cache 命中和 local memory 等问题
  - `scheduler statistics` 用来看 active / eligible / issued warp
  - `warp state statistics` 用来看 stall 原因
  - `source page` 用来看具体 SASS 与 metrics 的对应关系

## 原始结论

- 这篇文章对 `she_wiki` 的稳定价值，是把 GPU 性能优化还原成一个完整骨架：
  - 先理解硬件结构和理论上限
  - 再理解 memory hierarchy 与 transaction 成本
  - 再理解 execution model 与调度/隐藏延迟机制
  - 最后再用 profiling 工具把抽象判断落到具体指标
- 更适合作为“CUDA/NVIDIA 算子性能优化骨架”的基础来源页，而不是单一架构速查表。

## 相关页面

- [CUDA/NVIDIA 算子性能优化骨架](../b_extractions/b2_technical_fundamentals/b25_cuda_operator_performance_optimization_skeleton.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [知乎全文获取流程](./c9_zhihu_full_content_retrieval.md)
