# 技术原理类

## 包含的知识提取页

- [gzip 与 DEFLATE 的分层关系](../b_extractions/b2_technical_fundamentals/b21_gzip_and_deflate_layering.md)
- [数字视频压缩的基础骨架](../b_extractions/b2_technical_fundamentals/b22_digital_video_compression_fundamentals.md)

## 这一类的共同点

- 关注底层机制本身是怎么运作的
- 强调先分层，再看实现
- 关注格式、编码、协议或运行机制之间的职责边界
- 也包括把复杂媒体系统拆回可理解的表示层、压缩层和封装层

## 核心总结

- 学技术原理时，先分清外层封装和内层机制，很多概念混淆都来自这一层没拆开
- 读底层规范时，优先抓结构骨架，再进入位级细节
- 真正稳定的知识通常是分层关系、核心流程和职责边界，而不是某个实现里的局部技巧
- 学数字媒体时，先看图像表示、人眼约束与压缩骨架，再看具体编解码器的代际差异
