# 技术原理类

## 包含的知识提取页

- [gzip 与 DEFLATE 的分层关系](../b_extractions/b2_technical_fundamentals/b21_gzip_and_deflate_layering.md)
- [数字视频压缩的基础骨架](../b_extractions/b2_technical_fundamentals/b22_digital_video_compression_fundamentals.md)
- [低延时系统的端到端成本链路](../b_extractions/b2_technical_fundamentals/b23_low_latency_system_cost_path.md)
- [C++ 运行时多态的虚表骨架](../b_extractions/b2_technical_fundamentals/b24_cpp_runtime_polymorphism_vtable.md)
- [CUDA/NVIDIA 算子性能优化骨架](../b_extractions/b2_technical_fundamentals/b25_cuda_operator_performance_optimization_skeleton.md)

## 这一类的共同点

- 关注底层机制本身是怎么运作的
- 强调先分层，再看实现
- 关注格式、编码、协议或运行机制之间的职责边界
- 也包括把复杂媒体系统拆回可理解的表示层、压缩层和封装层
- 也包括把高性能或低延时问题拆回网络、内核、调度、复制和应用计算这些真实成本层
- 也包括把语言特性背后的运行时机制拆回对象布局、指针和分派路径
- 也包括把 GPU 算子性能问题拆回硬件上限、访存事务、调度与专用计算单元这些真实机制层

## 核心总结

- 学技术原理时，先分清外层封装和内层机制，很多概念混淆都来自这一层没拆开
- 读底层规范时，优先抓结构骨架，再进入位级细节
- 真正稳定的知识通常是分层关系、核心流程和职责边界，而不是某个实现里的局部技巧
- 学数字媒体时，先看图像表示、人眼约束与压缩骨架，再看具体编解码器的代际差异
- 看低延时系统时，先画出端到端成本链路，再讨论语言、框架或硬件取舍
- 学语言运行机制时，先抓对象布局和运行时分派骨架，再进入某个 ABI 的槽位细节
- 看 GPU 性能时，先分清算力上限、带宽上限、访存事务和调度约束，再讨论某条指令或某个库为什么更快
