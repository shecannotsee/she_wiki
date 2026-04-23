# she_wiki

[英文](./README.md)

`she_wiki` 的主入口是 [分类总览](./wiki/a_summaries/a1_overview.md)。
总览页说明仓库的阅读方式、内容分布和各分类下的现有主题。

新文章归档流程由 `she-wiki-maintainer` 对应的规则和方法文档定义。

## 入口说明

- [分类总览](./wiki/a_summaries/a1_overview.md) 是仓库的总入口，用于建立整体认识和快速浏览现有内容
- [分类总览](./wiki/a_summaries/a1_overview.md) 中的“维护检查清单”说明新增条目后通常需要同步检查的页面
- [文章归档方法](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md) 说明新文章进入仓库时的处理流程、文件落点和链接维护方式
- [Skill：she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md) 记录仓库维护时使用的结构规则和编辑约束

## 快速开始

适用场景：你已经有 URL、文章标题或正文，希望代理把内容按仓库规范整理进 `she_wiki`。

推荐流程：

1. 明确要求代理使用 `she-wiki-maintainer`。
2. 给出文章来源。
3. 优先采用 `c_sources -> b_extractions -> a_summaries` 的完整流程。
4. 说明应优先更新已有页面，还是只在必要时新建页面。

最常用提示词：

```text
请按 she-wiki-maintainer 规则，把这篇文章整理进 she_wiki，按 c_sources -> b_extractions -> a_summaries 处理，并保持编号和链接一致：
<URL>
```

```text
请按 she-wiki-maintainer 的阅读顺序，先学习这篇文章，不要立即写文件。

要求：
1. 先给我一个摘要
2. 判断它在 she_wiki 中应该归到哪个主题
3. 告诉我应该落到哪些文件层级（a / b / c）

来源：<URL>
```

```text
请按 she-wiki-maintainer 规则处理这篇文章，并优先并入 she_wiki 里现有主题，除非必要不要新开分类：
<URL>
```

完整归档流程、文件落点和提示词变体集中在以下页面：

- [分类总览](./wiki/a_summaries/a1_overview.md)
- [文章归档方法](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [Skill：she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md)

## Wiki 架构

这是一个按三层结构整理的本地 wiki：

1. `a_summaries`
2. `b_extractions`
3. `c_sources`

编号规则：

- 层级使用字母前缀，按阅览顺序排序：`a / b / c`
- `a` 用于摘要层，`b` 用于知识提取层，`c` 用于来源层
- 后续数字直接使用自然数序列，例如 `a1`、`a2`、`b3`、`c9`
- `b` 层的文件继承所属分类前缀，例如 `b3` 分类下的文件使用 `b31`、`b32`、`b33`
- 目录和文件都按自然数往后递增即可，不需要固定四位数字

当前入口：

- [分类总览](./wiki/a_summaries/a1_overview.md)
- [文章归档方法](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [Skill：she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md)
