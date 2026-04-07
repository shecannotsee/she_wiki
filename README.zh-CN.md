# she_wiki

[English](./README.md)

这是一个按三层结构整理的本地 wiki：

1. `a_summaries`
2. `b_extractions`
3. `c_sources`

编号规则：

- 层级使用字母前缀，按阅览顺序排序：`a / b / c`
- `a` 用于 summaries，`b` 用于 extractions，`c` 用于 sources
- 后续数字直接使用自然数序列，例如 `a1`、`a2`、`b3`、`c9`
- `b` 层的文件继承所属分类前缀，例如 `b3` 分类下的文件使用 `b31`、`b32`、`b33`
- 目录和文件都按自然数往后递增即可，不需要固定四位数字

当前入口：

- [分类总览](./wiki/a_summaries/a1_overview.md)
- [文章归档方法](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [Skill：she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md)

如何使用这个仓库配合 skill：

1. 准备文章来源，例如 URL、标题或正文。
2. 明确要求代理按 `she-wiki-maintainer` 处理。
3. 优先采用 `c_sources -> b_extractions -> a_summaries` 的完整流程。
4. 说明应优先更新已有页面，还是允许新建页面。

推荐提示词：

```text
请按 she-wiki-maintainer 规则，把这篇文章整理进 she_wiki，按 c_sources -> b_extractions -> a_summaries 处理，并保持编号和链接一致：
<URL>
```
