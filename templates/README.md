# {{FOLDER}}

An LLM Wiki covering **{{TOPIC}}**.

This is a persistent, interlinked markdown knowledge base. You supply sources and
ask questions; the assistant reads, synthesizes, cross-references, and keeps the
whole thing current. Knowledge accumulates here instead of being re-derived from
scratch every time you ask something.

## Layout

- `raw/` — original sources, never edited
- `wiki/` — everything the assistant writes
  - `concepts/` — ideas, techniques, definitions, themes
  - `entities/` — people, organizations, products, places, systems
  - `sources/` — one page per ingested source
  - `index.md` — catalog of every page
  - `log.md` — history of what happened when
  - `overview.md` — evolving big-picture synthesis
- `CLAUDE.md` — the schema: rules and workflows the assistant follows here

## Using it

Open this folder with Claude Code and talk to it:

- **Add a source:** `ingest this: <content, file path, or URL>`
- **Ask a question:** ask normally — answers come with citations
- **Health check:** `lint the wiki`

## Credit

Scaffolded with the [`llm-wiki-init`](https://github.com/vijay-athithyaa-GV/llm-wiki-init)
plugin, which implements the LLM Wiki pattern described by Andrej Karpathy in
[this gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
