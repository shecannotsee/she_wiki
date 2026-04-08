# Divio Documentation System 全页整理

## 记录时间

- 2026-04-08 CST

## 原始来源

- 总览页：https://docs.divio.com/documentation-system/
- Introduction：https://docs.divio.com/documentation-system/introduction/
- Tutorials：https://docs.divio.com/documentation-system/tutorials/
- How-to guides：https://docs.divio.com/documentation-system/how-to-guides/
- Reference guides：https://docs.divio.com/documentation-system/reference/
- Explanation：https://docs.divio.com/documentation-system/explanation/
- About the structure：https://docs.divio.com/documentation-system/structure/
- Who is using the system?：https://docs.divio.com/documentation-system/adoption/

## 原始主张

- Divio 的核心主张是：软件文档并不是一种东西，而是四种不同功能的内容
- 这四种内容分别是：
  - `tutorials`
  - `how-to guides`
  - `reference guides`
  - `explanation`
- 它们分别面向不同阶段、不同目的的读者需求，因此必须明确区分

## 各章节要点

### About

- 文档系统被描述为一个简单、完整、几乎普适的方案
- 它强调文档写作里存在一些很少被明确说出的原则
- 该系统已经在不同类型项目中被反复验证

### Introduction

- 大多数团队并不是不重视文档，而是不知道正确的结构应该是什么
- 文档问题往往不是“写得不够努力”，而是“写法不对”
- 四分结构同时帮助作者和读者：
  - 作者更容易判断写什么、怎么写、写到哪里
  - 读者在不同阶段都能找到匹配当下需求的内容
- 如果不显式承认这四种功能，文档很难长期维护好

### Tutorials

- `tutorial` 的目标是让新手通过做成一件事获得信心
- 它是学习导向的，偏向 `learning by doing`
- 好教程要满足：
  - 手把手
  - 面向新手
  - 尽快看到结果
  - 强可重复
  - 少讲抽象概念
  - 只给完成当前步骤所必需的解释
- Divio 特别强调：
  - 教程不是最佳实践手册
  - 新手的第一步可以是被强引导的“婴儿步”
  - 只要能顺利启动，就比一开始追求完整正确更重要

### How-to guides

- `how-to guide` 是回答“如何完成某个具体任务”
- 它是目标导向的，像菜谱或操作指引
- 与教程的区别是：
  - 教程由作者决定新手该学什么
  - how-to 由用户提出自己要解决的问题
- 好 how-to 要满足：
  - 以步骤组织
  - 聚焦结果
  - 解决一个具体问题
  - 不解释概念
  - 允许一定灵活性
  - 删掉不必要内容
  - 标题直接回答“How to ...”

### Reference guides

- `reference` 的唯一职责是准确描述系统、接口和行为
- 它是信息导向的
- 好 reference 要满足：
  - 按代码或对象结构组织
  - 风格、结构、格式高度一致
  - 只做描述，不做讨论、教学或意见表达
  - 保持准确并持续更新
- 它可以给使用示例，但这些示例只是为了说明描述本身

### Explanation

- `explanation` 是讨论、背景和原理说明
- 它是理解导向的
- 它的职责包括：
  - 提供上下文
  - 解释历史原因、设计决策和技术约束
  - 比较备选方案
  - 讨论不同意见
- 它不负责告诉用户一步步做什么，也不负责提供技术条目式描述

### About the structure

- 四个象限之所以常被混淆，是因为相邻类型在表面上确实相似：
  - 教程和 how-to 都写步骤
  - how-to 和 reference 都常发生在工作中
  - reference 和 explanation 都偏理论
  - tutorials 和 explanation 都常出现在学习阶段
- 正是这种重叠，导致文档结构有自然塌缩倾向
- 文档一旦塌缩，作者和读者都会失去清晰边界
- 该页还强调这套分析并不是纯主观偏好，而是有实践、教学和可维护性上的基础

### Adoption

- 文档系统已有不少项目和组织采用，即便有些采用还不完整
- 采用列表包括：
  - Divio
  - Django
  - django CMS
  - NumPy
  - Cloudflare Workers docs
  - BeeWare
  - Bosch 内部文档
  - Tesla Motors 内部文档
  - Zalando 内部文档
- 这一页的意义主要是证明：
  - 这套系统不是纯理论框架
  - 它已经在不同规模和不同类型的文档实践中落地

## 原始结论

- Divio 真正提供的不是“四个栏目名字”，而是一套按读者认知任务分层的文档方法
- 教程、操作指南、参考手册和背景解释各自只有一个主职责，混写会让文档在目标、语气和结构上互相拉扯
- 好文档的关键不是“多写一点”，而是先分清当前内容究竟在服务学习、做事、查阅还是理解
- 这种结构既提升写作可维护性，也改善读者在不同阶段的查找路径

## 相关页面

- [按认知任务分层的软件文档](../b_extractions/b1_methods/b17_divio_documentation_system.md)
