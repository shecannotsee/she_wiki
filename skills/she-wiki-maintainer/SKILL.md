---
name: she-wiki-maintainer
description: Maintain the she_wiki repository using its three-layer structure. Use this skill when ingesting new knowledge into she_wiki, extracting stable knowledge from sources, updating category summaries, enforcing the a/b/c naming scheme, or keeping internal links consistent.
---

# she-wiki-maintainer

Use this skill when working inside `she_wiki`.

## Structure

The repository keeps exactly three layers, ordered by reading flow:

1. `a_summaries`
2. `b_extractions`
3. `c_sources`

Meaning:

- `a_summaries`: category and overview pages that users read first
- `b_extractions`: stable knowledge distilled from sources
- `c_sources`: source context, raw facts, links, paths, commands, and original details

## Naming rules

- Use lowercase ASCII filenames with hyphens or underscores only.
- Keep the letter prefix that encodes the layer:
  - `a` for summaries
  - `b` for extractions
  - `c` for sources
- Use natural-number sequences after the prefix.
- In the `b` layer, files inherit the category prefix:
  - `b1_methods/b11_xxx.md`
  - `b3_engineering_troubleshooting/b31_xxx.md`
  - `b3_engineering_troubleshooting/b32_xxx.md`

## Content rules

- Do not recreate extra layers such as entities, standalone analyses, or question folders.
- Do not mention legacy aggregation files from the old knowledge base.
- Keep durable conclusions in `b_extractions`, not in `c_sources`.
- Keep source-specific details in `c_sources`, not in `a_summaries`.
- `a_summaries` should only classify, group, summarize, and guide reading order.

## Ingestion workflow

When new knowledge is added:

1. Create or update a page in `c_sources`.
2. Extract stable conclusions into the correct category under `b_extractions`.
3. Update the relevant summary page in `a_summaries`.
4. Update `a1_overview` when the topic inventory, category counts, or category distribution changes.
5. Update links across all touched pages.

Because `a1_overview` is a full browsing entry page, new topics should be assumed to require an overview check unless it is already certain that the overview inventory remains unchanged.

## Query workflow

When answering questions about the wiki:

1. Read `a_summaries` first.
2. Read the relevant `b_extractions` page next.
3. Read `c_sources` only when source verification or source context is needed.

## Editing workflow

- Prefer editing an existing page over creating near-duplicates.
- If a new topic belongs to an existing category, continue that category's numbering.
- If a new category is needed in `b_extractions`, mirror it in `a_summaries`.
- Keep cross-links explicit and relative.
