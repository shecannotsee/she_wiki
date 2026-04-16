# 知乎回答：自己写的CUDA矩阵乘法能优化到多快

## 记录时间

- 2026-04-16 CST

## 原始讨论对象

- 知乎问题：《自己写的CUDA矩阵乘法能优化到多快？》
- 目标回答作者：`nicholaswilde`
- 来源链接：https://www.zhihu.com/question/41060378/answer/2645323107
- 回答 ID：`2645323107`
- 回答创建时间：2022-08-25 21:48:42 CST
- 回答更新时间：2022-08-25 21:48:42 CST
- API 链接：https://www.zhihu.com/api/v4/answers/2645323107?include=content,excerpt,question,author,created_time,updated_time,voteup_count,comment_count,content_need_truncated
- 可交叉核对的全文镜像：https://developer.volcengine.com/articles/7382323676894461962

## 正文获取情况

- 知乎公开 `api/v4` 可返回回答元数据和长正文开头，但返回 `content_need_truncated=true`。
- 当前环境直接访问知乎原页只能拿到反爬壳页面，不能从静态 HTML 抓正文。
- 本次按 she_wiki 的“强前端反爬内容获取”流程，使用真实浏览器渲染 + CDP `Runtime.evaluate` 读取目标回答全文。
- 提取结果可与外部全文镜像交叉核对，章节结构一致。

## 原始信息

- 回答的目标非常具体：
  - 手写 `fp16` 矩阵乘法算子
  - 基于 Tensor Core 把性能尽量逼近甚至超过 `cuBLAS`
  - 在 `RTX 3090` 上把最终 kernel 做到约 `131 TFLOPS`
- 回答先给出一个很重要的性能判断框架：
  - 先估算目标硬件的峰值算力
  - 再看自己的 kernel 达到了峰值的百分之多少
  - 这样才能判断自己离上限还有多远
- 在 Tensor Core 基础部分，回答解释：
  - Tensor Core 从 Volta 开始引入
  - Turing / Ampere 的数据布局和指令形态逐步规整
  - `HMMA.1688`、`HMMA.16816` 之类指令代表了不同代 Tensor Core 的最小运算单元
- 回答还强调：
  - Tensor Core 指令和普通标量/向量指令的工作方式不同
  - 一个 warp 的 32 个线程需要协作，配合寄存器布局共同完成矩阵乘法
  - 所以性能优化既是指令问题，也是数据排布问题
- 在编程方法部分，回答把三层接口关系说得很清楚：
  - C++ `wmma` API
  - PTX 级 `wmma` / `mma` / `ldmatrix`
  - 最终落到 SASS 的 `LDSM` / `HMMA`
- 回答指出：
  - `wmma.load` 按 stride 访问，表达能力有限
  - `ldmatrix` 可以为每 4 个线程指定地址，更适合灵活处理 shared memory 布局和 bank conflict
- 在第一个可运行 HGEMM kernel 中，回答把分块参数选择讲成了约束平衡问题：
  - `BM`、`BN` 越大，计算访存比越好
  - 但 accumulator 要占寄存器，所以不能无限增大
  - `BK` 至少要满足 Tensor Core fragment 的结构要求
  - `BK` 过小会让循环和地址计算开销占比过大
  - shared memory 容量又进一步限制了 `(BM + BN) * BK`
- 回答第一个关键工程点是 shared memory 布局：
  - 为了避免 `LDSM` 取数时发生 bank conflict，需要给每行加 pad
  - 这是一种直接但略显浪费 shared memory 的方案
  - 更高级的无冲突排布需要使用 `ldmatrix`
- 第二个关键工程点是 `global memory -> shared memory` 的异步拷贝：
  - Ampere 之前需要经寄存器中转
  - Ampere 引入 `cp.async` 后可以省掉中间寄存器并更好隐藏延迟
  - 回答给出嵌入式 PTX 使用 `cp.async`、`commit_group`、`wait_group` 的实际写法
- 回答特别提醒：
  - shared memory 指针在嵌入式 PTX 下不能直接拿 generic pointer 用
  - 需要通过 `__cvta_generic_to_shared()` 等方式映射到正确地址空间
- 第三个关键工程点是 `double buffer`：
  - 一边计算当前 tile
  - 一边预取下一轮计算需要的数据
  - 用计算覆盖访存延迟
  - 这是回答里带来最大单次收益的步骤之一
- 第四个关键工程点是提升 `L2 cache` 局部性：
  - 回答发现默认 block 调度顺序会让大矩阵下矩阵 A 与矩阵 B 的局部性不平衡
  - 因此通过调整 grid 维度，把 block 的调度次序改写成更均衡的遍历方式
  - 实际收益不是全区间显著，但在大矩阵样本上能改善尾部性能
- 第五个关键工程点是给编译器更多空间：
  - 对主循环做 `#pragma unroll`
  - 进一步提高 HMMA 主体的密度
  - 在较大矩阵上基本全面超过 `cuBLAS`
- 回答最后也保留了边界：
  - 性能里有一部分来自假设 `M/N/K` 都对齐
  - 没有处理 corner case
  - 所以它展示的是“如何逼近峰值”的工程路线，不是完备生产实现

## 原始结论

- 这篇回答对 `she_wiki` 的稳定价值，不只是“HGEMM 可以做到多快”，而是把一条非常具体的 GPU 算子调优路径写清楚了：
  - 峰值建模
  - Tensor Core 指令与寄存器布局理解
  - tile 参数权衡
  - shared memory 无冲突排布
  - `cp.async`
  - `double buffer`
  - L2 局部性
  - 编译器循环展开
- 更适合作为“CUDA/NVIDIA 算子性能优化骨架”的实例来源页，而不是单纯的 GEMM benchmark 页面。

## 相关页面

- [CUDA/NVIDIA 算子性能优化骨架](../b_extractions/b2_technical_fundamentals/b25_cuda_operator_performance_optimization_skeleton.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [知乎全文获取流程](./c9_zhihu_full_content_retrieval.md)
