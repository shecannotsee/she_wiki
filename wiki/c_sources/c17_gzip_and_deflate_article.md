# 文章来源：gzip 格式和 DEFLATE 压缩算法

## 记录时间

- 2026-04-08 CST

## 原始来源

- 文章标题：`Gzip 格式和 DEFLATE 压缩算法`
- 作者：Luyu Huang
- 来源链接：https://luyuhuang.tech/2020/04/28/gzip-and-deflate.html

## 原始结构

- 引言
- LZ77 算法
  - 基本原理
  - 使用哈希表
  - 滑动窗口
  - 惰性匹配
- Huffman 编码
  - 前缀编码
  - 对距离和长度进行编码
  - Huffman 编码表的存储
- Gzip 格式
  - 未压缩区块
  - 压缩区块
- 总结

## 原始信息

- 文章明确区分了 `tar`、`gzip` 和 `DEFLATE`：
  - `tar` 是归档格式
  - `gzip` 是压缩文件格式
  - `DEFLATE` 是 `gzip` 常用的压缩数据格式
- 文章将 `DEFLATE` 拆成两部分解释：
  1. 用 `LZ77` 风格的重复串匹配，把重复内容改写成距离和长度
  2. 用 `Huffman` 编码压缩普通字节、长度和距离的表示成本
- 对 `LZ77` 部分，文章强调了几个实现要点：
  - 用哈希表加速匹配起点查找
  - 用滑动窗口限制搜索范围，典型窗口大小是 `32KB`
  - 用惰性匹配在压缩率和速度之间取平衡
- 对 `Huffman` 部分，文章强调：
  - `DEFLATE` 不是直接写出 `<距离,长度>` 这样的文本标记
  - 它把字面量、长度、距离映射到编码空间里，再用前缀码表示
  - 为了降低字典描述成本，会使用规范化的 Huffman 编码长度序列来重建编码表
- 对 `gzip` 格式部分，文章强调：
  - `gzip` 由头部、压缩主体和尾部构成
  - 头尾携带的是时间、压缩方法、操作系统、校验和、原始大小等元信息
  - 压缩主体由一个或多个区块组成
  - 区块可以是不压缩、静态 Huffman 压缩或动态 Huffman 压缩

## 原始结论

- 这篇文章的核心价值不是教人背位级细节，而是把一个常见混淆讲清楚：
  - `gzip` 不是压缩原理本身
  - 真正负责压缩表示的是 `DEFLATE`
- 文章还给出了一种更可靠的阅读顺序：
  - 先分清容器格式
  - 再分清压缩数据格式
  - 再进入匹配策略与编码策略

## 相关规范

- RFC 1951：`DEFLATE Compressed Data Format Specification version 1.3`
- RFC 1952：`GZIP file format specification version 4.3`

## 相关页面

- [gzip 与 DEFLATE 的分层关系](../b_extractions/b2_technical_fundamentals/b21_gzip_and_deflate_layering.md)
