---
description: Scaffold a new LLM Wiki — a persistent, interlinked markdown knowledge base on any topic
argument-hint: <folder-name> "<topic description>"
allowed-tools: Read, Write, Glob
---

# /wiki-init

Scaffold a new LLM Wiki in a fresh folder.

Arguments:

- `$1` — the target folder name
- `$2` — a short description of the topic this wiki will cover

## Step 0 — Resolve arguments

If `$1` is missing, ask: **"What should the wiki folder be called?"**

If `$2` is missing, ask: **"What topic will this wiki cover? A short phrase is
enough."**

Do not guess either value, and do not assume anything about the kind of topic —
it may be technical, historical, commercial, personal, creative, or anything
else. Wait for the user's answer before continuing.

Throughout the rest of this command, `<FOLDER>` means the value of `$1` and
`<TOPIC>` means the value of `$2`.

## Step 1 — Refuse to overwrite an existing wiki

Before creating anything, check whether `<FOLDER>/CLAUDE.md` exists.

**If it exists, stop immediately.** Create nothing, modify nothing. Tell the
user:

> A wiki already exists at `<FOLDER>/`. I won't overwrite it.
>
> - Run `/wiki-status` inside that folder to see what's already there.
> - Or re-run `/wiki-init` with a different folder name.

Then end the command. Do not proceed to Step 2 under any circumstances.

## Step 2 — Create the directory structure

Only once Step 1 has confirmed `<FOLDER>/CLAUDE.md` does **not** exist:

```text
<FOLDER>/raw/
<FOLDER>/wiki/concepts/
<FOLDER>/wiki/entities/
<FOLDER>/wiki/sources/
```

Create these by writing an empty `.gitkeep` file into each — `Write` creates any
missing parent directories on its own, so no shell is needed. The `.gitkeep`
files also let the empty directories survive a git commit.

Do not shell out to `mkdir`; this command must work identically on Windows,
macOS, and Linux, including in environments with no shell available.

## Step 3 — Write `<FOLDER>/CLAUDE.md`

This is the schema file — the document that tells any future session how to
maintain this wiki. Write it exactly as below, substituting `<FOLDER>` and
`<TOPIC>` with the real values.

````markdown
# <FOLDER> — LLM Wiki

**Topic:** <TOPIC>

This directory is an LLM Wiki: a persistent, interlinked markdown knowledge base
that the assistant builds and maintains from sources the user provides. This file
is the schema — it defines the structure, the rules, and the workflows.

Pattern credit: Andrej Karpathy —
<https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>

## Layout

- `raw/` — original source material, stored exactly as provided
- `wiki/concepts/` — ideas, techniques, definitions, recurring themes
- `wiki/entities/` — people, organizations, products, places, systems
- `wiki/sources/` — one page per ingested source: summary, key claims, provenance
- `wiki/index.md` — the catalog: every page in the wiki, one line each
- `wiki/log.md` — append-only history of ingests, queries, and maintenance
- `wiki/overview.md` — evolving synthesis of what this wiki currently knows

## Rules

1. **Raw sources are immutable.** Never edit, reformat, reorganize, rename, or
   delete anything under `raw/`. It is the record of what was actually provided.

2. **All synthesis lives in `wiki/`**, organized into `concepts/`, `entities/`,
   and `sources/`. Nothing is written anywhere else.

3. **Every page must be linked from `wiki/index.md`** with a one-line summary.
   A page that is not in the index is invisible — treat indexing as part of
   creating the page, not a follow-up chore.

4. **Use `[[wikilink]]` style links** between related pages. Link generously in
   both directions; the accumulated cross-references are the main thing that
   makes this wiki more useful than the sources it was built from.

5. **Flag contradictions — never resolve them silently.** If a new source
   disagrees with something already written, add a `> **Conflict:**` note on the
   affected page naming both sources and what each claims, and raise it with the
   user. Do not overwrite the earlier claim on your own authority.

6. **Never fabricate.** If the wiki does not contain a confident answer, say so
   plainly. Do not guess, and never file a guess back into the wiki where a later
   session would read it as sourced fact. A gap is information; a fabrication is
   contamination.

## Workflows

### Ingest — a new source arrives

1. Save the source verbatim into `raw/`.
2. Read it in full.
3. Discuss the key points with the user *before* writing any wiki pages.
4. Create or update pages across `wiki/concepts/`, `wiki/entities/`, and
   `wiki/sources/`. A single source often touches several pages.
5. Update `wiki/index.md` with every new page and its one-line summary.
6. Append an entry to `wiki/log.md`.

### Query — a question arrives

1. Read `wiki/index.md` first, to see what the wiki actually contains.
2. Read the relevant pages it points to.
3. Answer with citations to specific wiki pages and raw sources.
4. If the answer was substantive, offer to file it back as a new wiki page.
5. If the wiki does not contain the answer, say so — see Rule 6.

### Lint — on request

Check for and report:

- **Orphan pages** — files under `wiki/` not linked from `wiki/index.md`
- **Stale or contradicted claims** — assertions a later source disputed
- **Missing cross-references** — pages covering the same entity or concept
  without `[[linking]]` to each other
- **Index drift** — entries in `index.md` pointing at files that no longer exist

Report findings first. Make changes only after the user confirms.
````

## Step 4 — Write `<FOLDER>/wiki/index.md`

An empty catalog, ready to be filled:

````markdown
# Index

The catalog of every page in this wiki. Each entry gets a one-line summary.

## Concepts

*No pages yet.*

## Entities

*No pages yet.*

## Sources

*No pages yet.*
````

## Step 5 — Write `<FOLDER>/wiki/log.md`

Use **today's actual date** in `YYYY-MM-DD` format for the entry:

````markdown
# Log

Append-only history of ingests, queries, and maintenance. Newest entries at the
bottom.

## [YYYY-MM-DD] created | Wiki initialized
````

## Step 6 — Write `<FOLDER>/wiki/overview.md`

A placeholder for synthesis that will grow as sources are ingested:

````markdown
# Overview

The current synthesis of what this wiki knows about **<TOPIC>** — the big
picture, rewritten as understanding develops.

*Nothing to summarize yet. Ingest a source to get started.*
````

## Step 7 — Write `<FOLDER>/README.md`

````markdown
# <FOLDER>

An LLM Wiki covering **<TOPIC>**.

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
````

## Step 8 — Report

Print the resulting tree:

```text
<FOLDER>/
├── CLAUDE.md
├── README.md
├── raw/
└── wiki/
    ├── concepts/
    ├── entities/
    ├── sources/
    ├── index.md
    ├── log.md
    └── overview.md
```

Then tell the user their next step, verbatim:

> Add a source with: `"ingest this: <content>"`
