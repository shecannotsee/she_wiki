# 用 she-wiki-maintainer 处理文章归档

## 记录时间

- 2026-04-07 CST

## 原始目标

- 解释 `she-wiki-maintainer` 这个 skill 的作用
- 说明用户可以如何下指令，让代理把一篇文章整理进 `she_wiki`
- 给出从“先学习”到“直接入库”的几种常见表达方式

## 原始结论

- 这个 skill 的重点不是单纯读文章，而是把文章按 `c_sources -> b_extractions -> a_summaries` 落进 `she_wiki`
- 下指令时最好明确四类信息：
  - 数据源是什么
  - 目标仓库是什么
  - 处理深度是什么
  - 输出要求是什么
- 常见工作模式有三种：
  - 先学习再决定是否写入
  - 直接按三层结构入库
  - 并入已有主题，避免重复建页

## 原始提示词模板

### 模板一：先学习再归档

```text
请按 she-wiki-maintainer 的阅读顺序，先学习这篇文章，不要立即写文件。
来源：https://www.xxx.com/...

要求：
1. 先给我一个摘要
2. 判断它在 she_wiki 中应该归到哪个主题
3. 告诉我应该落到哪些文件层级（a / b / c）
4. 等我确认后再实际写入
```

### 模板二：直接入库

```text
请把这篇文章按 she_wiki 的三层结构直接归档到 /home/shecannotsee/Desktop/projects/she_wiki：
https://www.xxx.com/...

要求：
1. 新增或更新 c_sources 中的来源页
2. 提炼稳定知识到对应的 b_extractions
3. 更新 a_summaries 的入口说明
4. 检查编号是否需要顺延
5. 最后告诉我修改了哪些文件，以及为什么这样归类
```

### 模板三：优先并入已有主题

```text
请按 she-wiki-maintainer 规则处理这篇文章：
来源：https://www.xxx.com/...
主题：代理协作 / 工程排障 / 方法论 / 系统设计
目标：把它并入现有主题，而不是新开大类

要求：
1. 先找现有相关页面
2. 优先更新已有 b_extractions 和 a_summaries
3. 只在必要时新增 c_sources 页面
```

### 模板四：最短可复用版本

```text
请按 she-wiki-maintainer 规则，把这篇文章整理进 she_wiki，按 c_sources -> b_extractions -> a_summaries 处理，并保持编号和链接一致：
<URL>
```

## 原始注意点

- 如果当前会话没有把这个 skill 注册成系统可用 skill，最好显式指出：
  - `请按 /home/shecannotsee/Desktop/projects/she_wiki/skills/she-wiki-maintainer/SKILL.md 的规则处理`
- 如果暂时不确定文章是否值得沉淀，先用“先学习再归档”模式
- 如果已经确认所属主题，直接说明主题可以减少错误分类

## 相关页面

- [she-wiki-maintainer](../../skills/she-wiki-maintainer/SKILL.md)
- [文章归档的使用方法](../b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
