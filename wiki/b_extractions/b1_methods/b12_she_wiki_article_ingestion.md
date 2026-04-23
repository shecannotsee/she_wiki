# she_wiki 文章归档方法

## 提取结果

- `she-wiki-maintainer` 适合把文章、讨论记录和资料按三层结构沉淀进 `she_wiki`
- 最有效的指令不是“帮我归档”，而是明确数据源、目标仓库、处理深度和输出要求
- 常用流程是先判断归属主题，再决定更新已有页面还是新增来源页，最后补全 summary 入口和交叉链接

## 新增条目后的改动面

当一篇新文章、链接或整理结果进入 `she_wiki` 时，通常需要检查以下位置：

1. `c_sources`
   - 新建来源页，或补充到已有来源页
   - 保留原始上下文、链接、引用和来源细节
2. `b_extractions`
   - 更新已有知识提取页，或在正确分类下新增提取页
   - 保留稳定结论，不把来源细节写进这一层
3. `a_summaries`
   - 更新对应分类页，使新条目出现在所属分类的摘要入口中
4. `a1_overview`
   - 当总览中的条目清单、分类计数或分类分布发生变化时，同步更新 [分类总览](../../a_summaries/a1_overview.md)
   - 当前总览页承担全量浏览入口的作用，因此新增条目后默认需要检查这一页
5. 交叉链接
   - 检查新增页、被合并页和 summary 页之间的相对链接是否完整

如果条目只是并入现有页面而没有改变分类结构，`a1_overview` 至少仍需要检查一次，以确认总览中的全量清单和计数是否保持准确。

## 推荐操作顺序

1. 提供文章来源，例如 URL、标题或正文
2. 指明目标仓库是 `/home/shecannotsee/Desktop/projects/she_wiki`
3. 指定处理方式：
   - 先学习再归档
   - 直接按 `c_sources -> b_extractions -> a_summaries` 入库
   - 优先并入已有主题
4. 说明输出要求，例如：
   - 是否允许新建页面
   - 是否优先更新已有主题
   - 是否需要返回归类理由和改动文件

## 可直接复用的短提示

- `请按 she-wiki-maintainer 规则，把这篇文章整理进 she_wiki，按 c_sources -> b_extractions -> a_summaries 处理，并保持编号和链接一致：<URL>`
- `请先学习这篇文章，判断它在 she_wiki 中应归属哪个主题，确认后再落库：<URL>`
- `请按 she-wiki-maintainer 规则处理这篇文章，并优先并入现有主题而不是新开大类：<URL>`

## 来源

- [用 she-wiki-maintainer 处理文章归档](../../c_sources/c10_she_wiki_article_ingestion_tutorial.md)
