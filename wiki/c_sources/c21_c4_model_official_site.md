# C4 Model 官网全站整理

## 记录时间

- 2026-04-08 CST

## 原始来源

- 站点首页：https://c4model.com/
- History：https://c4model.com/history
- Introduction：https://c4model.com/introduction
- Abstractions：https://c4model.com/abstractions
- Software system：https://c4model.com/abstractions/software-system
- Container：https://c4model.com/abstractions/container
- Component：https://c4model.com/abstractions/component
- Code：https://c4model.com/abstractions/code
- Microservices：https://c4model.com/abstractions/microservices
- Queues and topics：https://c4model.com/abstractions/queues-and-topics
- Abstractions FAQ：https://c4model.com/abstractions/faq
- Diagrams：https://c4model.com/diagrams
- System landscape diagram：https://c4model.com/diagrams/system-landscape
- System context diagram：https://c4model.com/diagrams/system-context
- Container diagram：https://c4model.com/diagrams/container
- Component diagram：https://c4model.com/diagrams/component
- Code diagram：https://c4model.com/diagrams/code
- Dynamic diagram：https://c4model.com/diagrams/dynamic
- Deployment diagram：https://c4model.com/diagrams/deployment
- Notation：https://c4model.com/diagrams/notation
- Review checklist：https://c4model.com/diagrams/checklist
- Diagrams FAQ：https://c4model.com/diagrams/faq
- Tooling：https://c4model.com/tooling
- FAQ：https://c4model.com/faq

## 原始结构

- History
- Introduction
- Abstractions
  - Software system
  - Container
  - Component
  - Code
  - Microservices
  - Queues and topics
  - FAQ
- Diagrams
  - System context
  - Container
  - Component
  - Code
  - System landscape
  - Dynamic
  - Deployment
  - Notation
  - Review checklist
  - FAQ
- Tooling
- FAQ

## 原始主张

- C4 Model 不是新的软件架构风格，而是一种把软件架构讲清楚的分层建模与画图方法
- 它试图用少量、固定、层次化的抽象，替代行业里大量模糊而随意的“盒子线条图”
- 核心价值是让团队可以围绕同一套抽象、同一套图类型和同一套标注纪律来沟通软件架构

## 原始信息

### 背景与动机

- Simon Brown 在软件架构培训和项目咨询中长期观察到：
  - 许多团队画的架构图难以阅读
  - UML 在很多团队中已经不再是现实工作流的一部分
  - 团队最终往往退回到临时拼凑的 boxes and lines 图
- C4 Model 是对一种更贴近工程现场的做法的形式化：
  - 先画系统与外部环境
  - 再画系统内部的应用和数据存储
  - 再按需要画应用内部的组件
  - 最后才下钻到代码层

### 四层核心抽象

- `Software System`
  - 能向用户提供价值的整体软件系统
  - 常与单一团队的所有权边界接近
- `Container`
  - 运行中的应用或数据存储
  - 是运行时边界，而不是 Docker 容器概念
  - 例如 Web 应用、单页前端、移动端、数据库模式、对象存储、脚本等
- `Component`
  - 容器内部一组通过清晰接口封装的相关职责
  - 不是独立部署单元
- `Code`
  - 类、接口、函数、对象、表等语言级或代码级构件

### 主要图类型

- `System Context Diagram`
  - 站在系统边界外看大图景
  - 重点是人、系统和依赖关系
- `Container Diagram`
  - 站到系统边界内看高层技术形状
  - 重点是职责分配、主要技术选型和容器间通信
- `Component Diagram`
  - 下钻到单个容器内部
  - 描述组件职责与相互协作
- `Code Diagram`
  - 下钻到单个组件的代码实现
  - 通常只在关键或复杂组件上按需使用

### 辅助图类型

- `System Landscape Diagram`
  - 用于展示更大范围内多个软件系统之间的关系
- `Dynamic Diagram`
  - 用于展示特定用例或功能在运行时的交互顺序
- `Deployment Diagram`
  - 用于展示某个部署环境中的实例、节点和基础设施关系

### 画图纪律与检查点

- 每张图都应有标题、图类型和范围
- 每张图都应有图例，说明形状、颜色、线型、箭头等含义
- 每个元素都应显式标注类型
- 每个元素都应有简短职责描述
- 容器和组件应显式写出技术选型
- 每条关系都应是有方向的，并有能说明意图的标签
- 容器之间的关系应写清通信技术或协议
- 官网提供了一张 `review checklist`，把审查点拆成：
  - General
  - Elements
  - Relationships

### 对若干常见问题的具体判断

- 不必机械地画满四层
  - 对大多数团队，`System Context + Container` 已经足够有价值
- `Container` 是运行时边界，不等于 JAR、DLL、模块或包
- `Component` 是功能分组，不等于部署单元
- 微服务如何建模，要看服务是单一软件系统内部的实现细节，还是已经变成独立团队拥有的软件系统
- 消息总线不宜整体画成一个容器
  - 更合适的做法是把 queue/topic 视作数据存储型容器
  - 或者把消息交互隐含到关系标注中
- 若需要更多抽象层，必须先精确定义含义，否则只会重新回到术语混乱

### 工具与长期维护

- 白板适合快速协作和设计讨论
- 长期文档应考虑工具如何支持：
  - 差异对比
  - 重命名传播
  - 查询关系
  - 与源码或模型同步
- 官网明确区分了 `diagramming` 和 `modelling`
  - 前者更像手工画图
  - 后者是先维护非可视化模型，再生成不同视图
- 对容易过时的层级，官网建议：
  - 组件图尽量自动生成
  - 代码图最好按需生成

## 原始结论

- C4 Model 的稳定价值，不在于“又一种图法”，而在于把软件架构图的抽象层级、图类型和标注纪律固定下来
- 它最适合沉淀“软件架构如何被表达与沟通”，而不是替代架构设计本身
- 这套方法特别强调按受众和缩放层级讲故事：
  - 对外先看系统上下文
  - 对内再看容器职责
  - 对实现再看组件和代码
- 长期可维护的关键，不是画得更花，而是抽象稳定、命名一致、关系清晰、能被审查和更新

## 相关页面

- [软件架构图的 C4 分层方法](../b_extractions/b1_methods/b18_c4_software_architecture_diagramming.md)
- [按认知任务分层的软件文档](../b_extractions/b1_methods/b17_divio_documentation_system.md)
