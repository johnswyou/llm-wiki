# LLM Wiki — Research Knowledge Base

This is a personal research knowledge base following the LLM Wiki pattern (Karpathy, 2026). Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase. The LLM writes and maintains all wiki content. The human curates sources, directs analysis, and asks questions.

## Directory Structure

```
.
├── CLAUDE.md           # This file — schema, conventions, workflows
├── raw/                # Immutable source documents (NEVER modify)
│   ├── assets/         # Downloaded images and attachments
│   └── *.md            # Clipped articles, papers, notes
├── wiki/               # LLM-generated and maintained pages
│   ├── index.md        # Content catalog — read this FIRST for any query
│   ├── log.md          # Chronological operation log (append-only)
│   ├── overview.md     # High-level synthesis of the entire wiki
│   └── *.md            # Entity, concept, summary, comparison pages
└── templates/          # Page templates (structural scaffolding)
    ├── entity.md
    ├── concept.md
    ├── source-summary.md
    └── comparison.md
```

## Rules

1. **Never modify files in `raw/`.** They are the immutable source of truth. Read from them; never write to them.
2. **Always update `wiki/index.md`** when creating or significantly modifying a wiki page. The index is the primary navigation tool.
3. **Always append to `wiki/log.md`** after completing any operation (ingest, query filed to wiki, lint pass).
4. **Use YAML frontmatter** on every wiki page, following the schemas defined below.
5. **Use `[[wikilinks]]`** with shortest-path format for all internal links. Example: `[[Machine Learning]]`, not `[[wiki/Machine Learning]]`.
6. **Never delete wiki pages without asking.** Offer to archive or merge instead.
7. **Preserve existing content** when updating pages — add, revise, or extend. Do not replace wholesale unless the user explicitly asks.
8. **Cite sources.** Link wiki claims back to source summary pages. Use inline links: `([[Summary - Article Title|source]])`.
9. **Update the `updated` frontmatter field** on any page you modify.
10. **Reuse existing tags.** Before creating a new tag, check what tags already exist in the wiki. Use lowercase, hyphenated format: `#machine-learning`, `#llm`, `#knowledge-management`.

## Page Types

### Entity (`type: entity`)

People, organizations, products, tools, datasets, repositories.

```yaml
---
type: entity
title: "Entity Name"
aliases: []
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_count: 0
---
```

Sections: Summary, Key Facts, Background, Connections, Sources.

### Concept (`type: concept`)

Ideas, topics, techniques, principles, methodologies.

```yaml
---
type: concept
title: "Concept Name"
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_count: 0
---
```

Sections: Definition, Explanation, Key Points, Related Concepts, Sources.

### Source Summary (`type: source`)

One per ingested raw source document. This is the bridge between `raw/` and `wiki/`.

```yaml
---
type: source
title: "Source Title"
author: ""
date_published: YYYY-MM-DD
date_ingested: YYYY-MM-DD
url: ""
source_type: article | paper | book | video | dataset | repo | other
tags: []
raw_path: "raw/filename.md"
---
```

Sections: Metadata (table), Summary, Key Takeaways (numbered), Notable Quotes (blockquotes), Connections (links to entity/concept pages).

### Comparison (`type: comparison`)

Structured analysis comparing two or more subjects.

```yaml
---
type: comparison
title: "X vs Y"
subjects: ["X", "Y"]
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Sections: Overview, Comparison (markdown table), Analysis, Sources.

## Naming Conventions

- **Wiki page filenames**: Title Case with spaces. Example: `Machine Learning.md`
- **Source summaries**: Prefixed with `Summary - `. Example: `Summary - Attention Is All You Need.md`
- **Comparison pages**: Use `vs` format. Example: `RAG vs LLM Wiki.md`
- **Tags**: Lowercase, hyphenated. Example: `#deep-learning`, `#transformer`
- **Raw source files**: Keep original filename from Web Clipper or use descriptive name

## Workflows

### Ingest

When the user adds a new source to `raw/` and asks you to ingest it:

1. **Read** the raw source document completely.
2. **Discuss** key takeaways with the user — what's important, what to emphasize.
3. **Create a source summary page** in `wiki/` using the source-summary template. Fill in all frontmatter and sections.
4. **Identify entities and concepts** mentioned in the source.
5. **For each entity/concept**: check if a wiki page already exists.
   - If yes: update the existing page with new information from this source. Increment `source_count`. Add the source to its Sources section.
   - If no: create a new page using the appropriate template.
6. **Update cross-references** — ensure new pages link to related existing pages, and existing pages link back.
7. **Update `wiki/index.md`** — add new pages under the correct category with one-line summaries.
8. **Update `wiki/overview.md`** if the new source materially changes the big picture.
9. **Append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] ingest | Source Title

   Ingested [source type]. Created: [list of new pages]. Updated: [list of updated pages].
   ```

A single ingest may touch 10-15 wiki pages. Take your time and be thorough.

### Query

When the user asks a question against the wiki:

1. **Read `wiki/index.md`** to identify relevant pages.
2. **Read those pages** to gather information.
3. **Synthesize an answer** with `[[wikilink]]` citations to wiki pages.
4. **Offer to file** substantial answers as new wiki pages (analysis, comparison, deep dive).
   - If the user agrees: create the page, update index and log.
   - If the user declines: the answer stays in the conversation.
5. If filing: **append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] query | Question Summary

   Filed answer as [[Page Name]]. Query: "[original question]"
   ```

### Lint

Periodic health check over the wiki. Run when the user asks, or suggest it after significant growth.

1. **Scan for**:
   - Contradictions between pages
   - Stale claims superseded by newer sources
   - Orphan pages with no inbound links (check via backlinks)
   - Important concepts mentioned frequently but lacking their own page
   - Missing cross-references between related pages
   - Frontmatter issues (missing fields, wrong types, stale `updated` dates)
   - Index drift (pages exist but aren't in the index, or index entries point to deleted pages)
   - Inconsistent tags (synonyms like `#ml` and `#machine-learning`)
2. **Report findings** to the user in a structured format.
3. **Fix with approval** — never auto-fix without the user's go-ahead.
4. **Suggest** new questions to investigate and new sources to look for.
5. **Append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] lint | Health Check

   Found: [number] issues. Fixed: [number]. Details: [brief summary].
   ```

## Index Format

`wiki/index.md` is a categorized catalog. The LLM reads this first for any query.

```markdown
# Wiki Index

Last updated: YYYY-MM-DD

## Entities
- [[Entity Name]] — Brief description (N sources)

## Concepts
- [[Concept Name]] — Brief description (N sources)

## Source Summaries
- [[Summary - Title]] — Author, date, key takeaway

## Comparisons
- [[X vs Y]] — What is compared and why

## Analyses
- [[Analysis Title]] — Filed query result or deep dive
```

## Log Format

`wiki/log.md` is append-only and chronological. Each entry starts with a consistent prefix for parseability.

```markdown
## [YYYY-MM-DD] action | Title

Brief description of what happened.
```

Actions: `seed`, `ingest`, `query`, `lint`, `update`, `create`.

Parse recent entries: `grep "^## \[" wiki/log.md | tail -5`

## Post-Setup (Manual Steps)

These require action in the Obsidian UI and cannot be done by the LLM:

1. **Install Dataview plugin**: Settings > Community plugins > Browse > search "Dataview" > Install > Enable. This allows frontmatter-based queries like:
   ```dataview
   TABLE source_count AS "Sources", updated AS "Last Updated"
   FROM "wiki"
   WHERE type = "entity"
   SORT source_count DESC
   ```
2. **Optional — Install Marp Slides**: Same process, search "Marp Slides". Enables generating presentations from wiki content.
3. **Optional — Graph View colors**: In Graph View settings, add color groups: `path:wiki` (one color), `path:raw` (another). Visually distinguishes the two layers.
4. **Install Obsidian Web Clipper**: Browser extension for clipping web articles directly to `raw/`. After clipping, tell the LLM to ingest the new source.
5. **Bind "Download attachments" hotkey**: Settings > Hotkeys > search "Download" > bind "Download attachments for current file" to a hotkey (e.g., Ctrl+Shift+D). This downloads images from clipped articles to `raw/assets/` for local access.
