# CUDA 学习的硬件优先与 profiling 顺序

## 提取结果

- CUDA 入门时，最常见的误区不是“练得不够多”，而是先背 API、先抄项目，却没有建立 GPU 执行模型的图景。
- 更稳的顺序不是“先会写一堆 kernel”，而是：
  - 先理解硬件
  - 再配置环境
  - 再做最小示例
  - 再进入 shared memory 和 profiling
  - 最后再扩展到库和更高层语言
- 真正该先刻在脑子里的不是函数名，而是：
  - `SM`
  - `Warp`
  - `SIMT`
  - register / cache / shared memory / global memory 这些层级关系
- 如果连寄存器、缓存、指针、内存和体系结构都还模糊，就很难真正理解为什么 block 大小、访存模式和共享内存会影响性能。
- 环境配置要尽早走工程化路径：
  - Linux 或 `WSL2`
  - `CMake`
  - 明确 `nvcc` 工具链
  - 不要长期依赖 IDE 点选配置
- block 和 grid 的初学起点应建立在 warp 调度事实上：
  - block 大小通常从 `32` 的倍数开始
  - 常用起点是 `128` 或 `256`
  - `grid = (N + block - 1) / block` 这种向上取整公式是基本功
- 练习路径应从最小到复杂逐级推进：
  - `Vector Add`：熟悉 API 和 kernel 启动
  - naive `GEMM`：感受“GPU 不会自动更快”
  - shared-memory `GEMM`：理解 tile 和局部重用
- `Shared Memory` 是 CUDA 学习的分水岭：
  - 它迫使你第一次真正面对数据搬运、重用和局部缓存管理
  - 也是从“会写”转向“会调”的关键一步
- profiling 不能只盯总时间：
  - 要看带宽利用率
  - 要看计算单元利用率
  - 要看瓶颈到底在算、在搬，还是在等待
- 这意味着 `Nsight Compute` 这类工具不是高级附属品，而是学习过程的一部分。
- GPU 访存的稳定原则之一是合并访问：
  - 线程束访问连续地址更容易高效
  - 访问离散地址会放大事务和带宽浪费
  - 有时必须为此调整数据排布，例如 `AoS -> SoA`
- 真实工程里不应把“手写一切”当成美德：
  - `cuBLAS`
  - `cuDNN`
  - `Thrust`
  - 都是必须纳入工具箱的现成能力
- 更高层的 `Triton`、`TVM` 等会继续把 CUDA 细节上卷抽象，但并行计算、同步成本和带宽限制这些底层约束不会因此消失。
- mini project 的价值，不在作品展示，而在迫使你把访存、同步和并行拆分真正做出来：
  - `Ray Tracing`
  - `Bitonic Sort`

## 稳定顺序

1. 先补齐体系结构、缓存、寄存器、指针和内存的基础图景。
2. 再理解 GPU 的执行模型：`SM / Warp / SIMT / memory hierarchy`。
3. 再用 Linux/`WSL2` + `CMake` 配出稳定的 CUDA 工程环境。
4. 再从 `Vector Add` 这类最小 kernel 开始，熟悉 API 与启动方式。
5. 再写 naive `GEMM`，体会为什么 GPU 并不天然更快。
6. 再进入 shared-memory `GEMM`、tile、局部重用和合并访存。
7. 同步学会用 `Nsight Compute` 看瓶颈，再扩展到库与更高层编程模型。

## 可直接复用

- 刚学 CUDA 时，先画出一张图，确保自己能说清：
  - 线程如何组成 warp
  - warp 如何被 SM 调度
  - 数据在 global/shared/register 之间怎么流
- block 大小时，先从 `128/256` 这种 `32` 的倍数起步，不要先拍脑袋乱试。
- kernel 练习时，不要只追“跑出来”，要同步问：
  - 数据怎么搬
  - 有无局部重用
  - 是否合并访存
  - stall 在哪里
- 学到 shared memory 时，不要停在“会写语法”，要追到：
  - tile 为什么能减少 global memory 访问
  - 什么时候值得换取更多 shared memory 占用
- 学 CUDA 时，越早接受“profiling 是学习的一部分”，后面越不容易走成玄学调参。
- 工程里先查有没有现成库：
  - 能调库就先调库
  - 需要手写 kernel 时，再针对真实瓶颈写
- 如果目标是长期竞争力，不要把自己锁死在某个语言表面：
  - CUDA C++ 只是入口
  - 并行拆解、同步控制、带宽利用才是更稳定的能力

## 来源

- [知乎回答：CUDA 初学者的学习路径](../../c_sources/c42_zhihu_cuda_beginner_learning_path.md)
