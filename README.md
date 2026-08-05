# llm-wiki-init

Scaffold a persistent, self-maintaining markdown knowledge base on any topic —
with one command.

Built on the **LLM Wiki** pattern described by Andrej Karpathy in
[this gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
The pattern is his; this is a Claude Code plugin that sets it up for you.

## Plugin vs. command

Two different names, easy to mix up:

| | Name | What it is |
|---|---|---|
| **The plugin** | `llm-wiki-init` | The installable package. You install this once. |
| **The commands** | `/wiki-init`, `/wiki-status` | What you type in Claude Code. Provided by the plugin. |

You install `llm-wiki-init`. You then run `/wiki-init`.

## Install

```
/plugin marketplace add vijay-athithyaa-GV/llm-wiki-init
/plugin install llm-wiki-init@llm-wiki-init
```

The first line registers this repo as a plugin marketplace. The second installs
the plugin from it.

## Usage

Create a wiki — pass a folder name and whatever topic you want it to cover:

```
/wiki-init my-wiki "topic of your choice"
```

The topic is entirely up to you. Nothing in this plugin assumes a domain.

Then work with it in plain language:

```
ingest this: <paste content, a file path, or a URL>
```

Ask questions normally — answers come back with citations to specific pages.
Ask it to `lint the wiki` for a health check.

Check on an existing wiki at any time:

```
/wiki-status
```

Read-only. Reports page counts per category, the last 5 log entries, and any
orphan pages that aren't linked from the index. It changes nothing.

## What you get

`/wiki-init my-wiki "topic of your choice"` produces:

```text
my-wiki/
├── CLAUDE.md          # the schema — rules and workflows for maintaining this wiki
├── README.md          # what this folder is and how to use it
├── raw/               # original sources, never edited
└── wiki/
    ├── concepts/      # ideas, techniques, definitions, themes
    ├── entities/      # people, organizations, products, places, systems
    ├── sources/       # one page per ingested source
    ├── index.md       # catalog of every page, one line each
    ├── log.md         # append-only history of what happened when
    └── overview.md    # evolving big-picture synthesis
```

`CLAUDE.md` is the important one. It's what makes the folder self-maintaining:
any future Claude Code session opened here reads it and knows the rules — raw
sources are immutable, every page gets indexed, contradictions get flagged rather
than quietly overwritten, and gaps get reported instead of filled with guesses.

Re-running `/wiki-init` against a folder that already has a `CLAUDE.md` is
refused. It will never overwrite an existing wiki.

## Why a wiki instead of RAG

RAG re-reads your documents on every question and throws the reasoning away when
it's done — each query starts from zero, and the connections it discovers die
with the response. A wiki does that work once and writes it down, so the tenth
question benefits from everything the first nine established.

The reason this is newly practical is that the expensive part of a knowledge base
was never the reading or the thinking — it was the bookkeeping. Updating twelve
cross-references because one new fact arrived is exactly the kind of tedium a
model will do indefinitely without complaint, and exactly the kind humans abandon
by week three.

## Credit

The LLM Wiki pattern is Andrej Karpathy's:
<https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>

This repo packages it as a Claude Code plugin. Read the gist — it explains the
reasoning far better than a README can.

## License

MIT — see [LICENSE](LICENSE).
