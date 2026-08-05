# {{FOLDER}} — LLM Wiki

**Topic:** {{TOPIC}}

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
