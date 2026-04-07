# she_wiki

[中文](./README.zh-CN.md)

This is a local wiki organized into three layers:

1. `a_summaries`
2. `b_extractions`
3. `c_sources`

Numbering rules:

- Use letter prefixes for sorting by reading order: `a / b / c`
- `a` is for summaries, `b` is for extractions, and `c` is for sources
- Use natural-number sequences after the prefix, such as `a1`, `a2`, `b3`, and `c9`
- Files under the `b` layer inherit their category prefix, for example `b31`, `b32`, and `b33` under category `b3`
- Both directories and files should continue incrementing naturally; fixed-width numbers are unnecessary

Entry points:

- [Overview](./wiki/a_summaries/a1_overview.md)
- [Article ingestion tutorial](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [Skill: she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md)

How to use this repository with the skill:

1. Start from a source such as a URL, article title, or raw text.
2. Ask the agent to process it with `she-wiki-maintainer`.
3. Prefer the full flow `c_sources -> b_extractions -> a_summaries`.
4. Tell the agent whether it should update existing pages first or create new ones.

Recommended prompt:

```text
Please process this article into she_wiki following she-wiki-maintainer and the flow c_sources -> b_extractions -> a_summaries, while keeping numbering and links consistent:
<URL>
```
