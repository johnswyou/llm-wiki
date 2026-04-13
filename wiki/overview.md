---
type: overview
updated: 2026-04-13
---

# Research Wiki — Overview

This is a personal research knowledge base maintained by an LLM. It follows the LLM Wiki pattern: raw source documents are ingested, compiled into structured wiki pages, and kept current as new sources arrive and new questions are asked. The knowledge compounds over time.

## Current State

| Metric | Count |
|--------|-------|
| Source documents ingested | 0 |
| Entity pages | 0 |
| Concept pages | 0 |
| Comparisons | 0 |
| Total wiki pages | 3 (index, log, overview) |

## Getting Started

1. **Add a source**: Drop an article, paper, or note into `raw/`. Use the Obsidian Web Clipper extension to clip web articles directly.
2. **Ingest it**: Tell the LLM "ingest `raw/filename.md`" — it will read the source, create a summary, extract entities and concepts, and update cross-references.
3. **Ask questions**: Query the wiki on any topic. The LLM reads the [[index]] to find relevant pages and synthesizes answers with citations.
4. **File answers**: Substantial query results can be saved as new wiki pages, so your explorations compound in the knowledge base.
5. **Lint periodically**: Ask the LLM to run a health check — it will find contradictions, orphan pages, missing cross-references, and suggest improvements.

## Key Pages

- [[index]] — Full catalog of all wiki pages, organized by type
- [[log]] — Chronological record of all operations
