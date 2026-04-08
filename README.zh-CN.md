# she_wiki

[英文](./README.md)

如果你想把一篇新文章快速归档进 `she_wiki`，最直接的入口就是让代理按 `she-wiki-maintainer` 处理。

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

如果你想继续看完整归档流程、文件落点和更多提示词变体，可以直接看：

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
