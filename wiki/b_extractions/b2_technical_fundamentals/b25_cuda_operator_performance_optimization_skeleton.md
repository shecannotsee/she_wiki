# CUDA/NVIDIA 算子性能优化骨架

## 提取结果

- CUDA 算子比手写 naive kernel 快，通常不是因为“官方库更底层”这一句空话，而是因为它们把算法、数据排布、访存、调度、专用指令、编译器和 profiling 闭环一起做到了位
- GPU 优化的第一步不是上技巧，而是先判断 workload 是 `memory bound` 还是 `compute bound`，并继续追到具体瓶颈落在 `DRAM`、`L2`、`L1`、`shared memory`、register 还是 scheduler
- 对 GPU 来说，真正决定效率的常常不是“发了多少条 load 指令”，而是一个 request 背后触发了多少 transaction、多少 replay、多少 bank conflict 和多少 stall
- 现代 NVIDIA GPU 上，Tensor Core 抬高了理论算力上限，也让更多 workload 更容易变成带宽受限或供数受限，所以理解 memory hierarchy 和 execution model 比单看 FLOPS 更重要
- GEMM 这类高强度算子的优化路径通常是分阶段累积的：
  - 先选对 tile 和寄存器/shared memory 预算
  - 再解决 shared memory 排布和 bank conflict
  - 再用 `cp.async`、double buffer 之类手段隐藏访存延迟
  - 再调 block 调度和 cache 局部性
  - 最后再给编译器和汇编级指令选择更多发挥空间

## 稳定结构

1. 先建立硬件和 workload 的上限模型
2. 再把瓶颈定位到具体 memory / execution 层级
3. 先解决 transaction、对齐、局部性和 bank conflict
4. 再解决 occupancy、eligible warp、stall 和 tail effect
5. 再引入 Tensor Core、`ldmatrix`、`cp.async`、double buffer 等专用机制
6. 最后用编译器展开、代码生成检查和 microbenchmark 继续逼近上限

## 可直接复用

- 面对“为什么库比我手写 kernel 快很多”，先追问这些问题：
  - 它是 `memory bound` 还是 `compute bound`
  - 当前 throughput 到底是多少
  - 编译结果是否真的实现了你的设计意图
  - 瓶颈是在 `DRAM`、`L2`、`L1`、`shared memory` 还是 scheduler
  - 理论上限是多少
  - 你有没有用 microbenchmark 证明这个上限可达
- 看 GPU 访存时，不只看 load/store 次数，还要看：
  - global memory 是否合并访存
  - 地址是否对齐
  - shared memory 是否发生 bank conflict
  - local memory / register spilling 是否把数据打回慢内存
  - instruction replay 是否在悄悄放大成本
- 看 GPU 执行效率时，要把这些概念拆开：
  - active warp
  - eligible warp
  - stall warp
  - selected warp
  - occupancy
  - tail effect
- 评估 Tensor Core 算子时，不要只背接口名：
  - 要知道 fragment 结构、warp 协作方式、寄存器排布、`wmma` / `mma` / `ldmatrix` / `HMMA` 的对应关系
  - 要知道为什么 shared memory 布局会直接影响 `LDSM` / `ldmatrix` 的冲突情况
- GEMM 类算子调优时，可稳定复用的顺序通常是：
  - 按寄存器和 shared memory 预算选 `BM / BN / BK`
  - 让 shared memory 排布避开 bank conflict
  - 把 global memory 到 shared memory 的供数链做成高效、可重叠
  - 用 double buffer 把计算和下一轮取数叠起来
  - 必要时改 block 调度顺序，改善 `L2` 局部性
  - 最后再做循环展开和更细的代码生成调整
- profiling 的价值不是“看报表”，而是把报表变成具体方向：
  - `memory workload analysis` 用来定位 request / transaction / cache / local memory 问题
  - `scheduler statistics` 用来看 warp 是否够多、够能发射
  - `warp state statistics` 用来看 stall 主因
  - `source page` 用来把指标追到具体 SASS 指令

## 来源

- [知乎回答：CUDA 的包比自己写的 Kernel 快 10~20 倍，有什么内在机制呢](../../c_sources/c33_zhihu_cuda_kernel_vs_library_performance.md)
- [知乎专栏：NVIDIA GPU性能优化基础](../../c_sources/c34_zhihu_nvidia_gpu_performance_optimization_basics.md)
- [知乎回答：自己写的CUDA矩阵乘法能优化到多快](../../c_sources/c35_zhihu_cuda_tensor_core_hgemm_optimization.md)
