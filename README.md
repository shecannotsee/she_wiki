# she_wiki

[Chinese](./README.zh-CN.md)

The main entry to `she_wiki` is the [Overview](./wiki/a_summaries/a1_overview.md).
The overview explains the reading structure, the content distribution, and the current topics across all categories.

The article archival flow is defined by `she-wiki-maintainer` and its related documentation.

## Entry Points

- [Overview](./wiki/a_summaries/a1_overview.md) is the repository entry page for orientation and quick scanning
- [Article ingestion tutorial](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md) defines the archival flow, file placement, and link maintenance rules
- [Skill: she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md) records the maintenance structure and editing constraints for this repository

## Quick Start

Use this when you already have a URL, article title, or raw text and want the agent to archive it into `she_wiki`.

Recommended flow:

1. Tell the agent to use `she-wiki-maintainer`.
2. Give it the source.
3. Prefer the full flow `c_sources -> b_extractions -> a_summaries`.
4. Say whether it should update existing pages first or create new ones only when needed.

Fast prompts:

```text
Please process this article into she_wiki following she-wiki-maintainer and the flow c_sources -> b_extractions -> a_summaries, while keeping numbering and links consistent:
<URL>
```

```text
Please read this article first using she-wiki-maintainer, do not write files yet, and tell me:
1. a short summary
2. which topic it belongs to in she_wiki
3. which a / b / c files should be updated
Source: <URL>
```

```text
Please process this article with she-wiki-maintainer and merge it into existing topics in she_wiki instead of creating a new category unless necessary:
<URL>
```

The full archival workflow, file placement rules, and prompt variants are collected in:

- [Overview](./wiki/a_summaries/a1_overview.md)
- [Article ingestion tutorial](./wiki/b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [Skill: she-wiki-maintainer](./skills/she-wiki-maintainer/SKILL.md)

## Wiki Structure

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
