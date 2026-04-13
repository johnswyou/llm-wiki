# llm-wiki

A personal research knowledge base powered by LLMs and [Obsidian](https://obsidian.md/). Instead of re-deriving knowledge from raw documents on every query (like RAG), an LLM incrementally builds and maintains a persistent, cross-linked wiki of markdown files that compounds over time.

Based on Andrej Karpathy's LLM Wiki pattern:

- [Original tweet](https://x.com/karpathy/status/2039805659525644595?s=20)
- [Full idea document (llm-wiki.md)](https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw/ac46de1ad27f92b28ac95459c782c07f6b8c964a/llm-wiki.md)

## How It Works

The system has three layers:

| Layer | Directory | Owner | Purpose |
|-------|-----------|-------|---------|
| **Raw Sources** | `raw/` | You | Immutable source documents — articles, papers, images, notes |
| **Wiki** | `wiki/` | LLM | Generated and maintained markdown — entities, concepts, summaries, cross-links |
| **Schema** | `CLAUDE.md` | You + LLM | Conventions, workflows, and rules that govern how the LLM operates |

You curate sources and ask questions. The LLM does all the bookkeeping — summarizing, cross-referencing, filing, and maintaining consistency across pages. Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.

## Prerequisites

- [Obsidian](https://obsidian.md/) (free, available on macOS/Windows/Linux)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (or another LLM agent with filesystem access)
- [Git](https://git-scm.com/)

## Getting Started

### 1. Clone the repo

```bash
git clone <your-remote-url>/llm-wiki.git
```

### 2. Open as an Obsidian vault

1. Open Obsidian.
2. Click **Open folder as vault** (or Open > Open folder as vault).
3. Select the cloned `llm-wiki` directory.
4. When prompted about community plugins, click **Trust author and enable plugins** (or enable them later in Settings).

Obsidian settings are already configured in `.obsidian/`:
- Attachments save to `raw/assets/`
- Links use `[[wikilink]]` shortest-path format
- Templates folder is set to `templates/`

### 3. Install recommended Obsidian plugins

These require manual installation through the Obsidian UI:

**Dataview** (recommended):
1. Settings (Cmd+, / Ctrl+,) > Community plugins > Browse
2. Search "Dataview" > Install > Enable
3. Enables frontmatter-based queries across wiki pages

**Marp Slides** (optional):
- Same process, search "Marp Slides"
- Generates slide deck presentations from wiki content

### 4. Install the Obsidian Web Clipper

Install the [Obsidian Web Clipper](https://obsidian.md/clipper) browser extension. This lets you clip web articles directly into `raw/` as markdown files.

After installing, configure it to save clipped files into your vault's `raw/` directory.

**Optional — bind the "Download attachments" hotkey:**
Settings > Hotkeys > search "Download" > bind "Download attachments for current file" to a hotkey (e.g., Ctrl+Shift+D). This downloads images from clipped articles to `raw/assets/` for local reference.

### 5. Point Claude Code at the vault

```bash
cd /path/to/llm-wiki
claude
```

Claude Code reads `CLAUDE.md` automatically and understands the wiki's structure, conventions, and workflows.

### 6. Ingest your first source

Drop an article, paper, or note into `raw/`, then tell the LLM:

```
Ingest raw/my-article.md
```

The LLM will:
1. Read the source document
2. Discuss key takeaways with you
3. Create a source summary page in `wiki/`
4. Extract entities and concepts into their own pages
5. Update cross-references, the index, and the log

A single ingest typically touches 10-15 wiki pages.

## Usage

### Core Operations

| Operation | What it does | How to invoke |
|-----------|-------------|---------------|
| **Ingest** | Process a new source into the wiki | `"Ingest raw/filename.md"` |
| **Query** | Ask questions against the wiki | Ask any question naturally |
| **Lint** | Health-check the wiki for issues | `"Run a lint pass on the wiki"` |

### Query examples

```
What are the main themes across all my sources?
Compare X and Y based on what the wiki knows.
What concepts are mentioned most but have the thinnest coverage?
Create a slide deck summarizing topic Z.
```

Substantial query answers can be filed back into the wiki as new pages, so your explorations compound over time.

### Lint checks

The LLM scans for contradictions between pages, orphan pages with no links, stale content superseded by newer sources, missing cross-references, and frontmatter inconsistencies. It reports findings and fixes them with your approval.

## Directory Structure

```
.
├── CLAUDE.md              # Schema — LLM conventions, workflows, rules
├── README.md              # This file
├── raw/                   # Source documents (immutable, never modified by LLM)
│   ├── assets/            # Downloaded images and attachments
│   └── *.md               # Clipped articles, papers, notes
├── wiki/                  # LLM-generated and maintained pages
│   ├── index.md           # Content catalog (LLM reads this first)
│   ├── log.md             # Chronological operation log
│   ├── overview.md        # High-level synthesis
│   └── *.md               # Entity, concept, summary, comparison pages
└── templates/             # Page templates for new wiki pages
    ├── entity.md           # People, orgs, tools, products
    ├── concept.md          # Ideas, topics, techniques
    ├── source-summary.md   # One per ingested source
    └── comparison.md       # Structured X vs Y analysis
```

## Wiki Page Types

All wiki pages carry YAML frontmatter for Dataview queries and LLM navigation.

| Type | Filename Pattern | Use |
|------|-----------------|-----|
| Entity | `Name.md` | People, organizations, products, tools |
| Concept | `Name.md` | Ideas, topics, techniques, principles |
| Source Summary | `Summary - Title.md` | One per ingested raw source |
| Comparison | `X vs Y.md` | Structured side-by-side analysis |

## Tips

- **Graph View** is the best way to see the shape of your wiki. Open it with Cmd+G / Ctrl+G. Optionally add color groups in Graph View settings: `path:wiki` in one color, `path:raw` in another.
- **You rarely touch the wiki directly.** The LLM writes and maintains all of it. You read it in Obsidian and guide the LLM through conversation.
- **Answers compound.** When the LLM produces a good analysis or comparison, file it back into the wiki as a new page. Your explorations become part of the knowledge base.
- **The index is the navigation hub.** At moderate scale (under a few hundred pages), `wiki/index.md` is sufficient for the LLM to find relevant content without embedding-based RAG.
- **Git gives you history for free.** The wiki is just a git repo of markdown files — you get version history, diffing, and branching.

## References

- Andrej Karpathy, ["LLM Knowledge Bases"](https://x.com/karpathy/status/2039805659525644595?s=20) — original tweet describing the pattern
- Andrej Karpathy, [llm-wiki.md](https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw/ac46de1ad27f92b28ac95459c782c07f6b8c964a/llm-wiki.md) — full idea document with architecture, operations, and tips
- Vannevar Bush, ["As We May Think"](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/) (1945) — the Memex vision that inspired this pattern

## License

This is a personal knowledge base template. Use it however you like.
