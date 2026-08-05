---
description: Report the health of an existing LLM Wiki — page counts, recent activity, and orphan pages
argument-hint: [wiki-folder]
allowed-tools: Read, Glob, Grep
---

# /wiki-status

Report on an existing LLM Wiki. **This command is strictly read-only** — it does
not create, edit, move, or delete a single file. If you find problems, report
them; do not fix them.

## Step 0 — Locate the wiki

If `$1` was given, treat it as the wiki folder. Otherwise use the current
directory.

Confirm the folder is a wiki by checking for both `CLAUDE.md` and `wiki/index.md`.

If either is missing, stop and tell the user:

> That doesn't look like an LLM Wiki — I couldn't find `CLAUDE.md` and
> `wiki/index.md`.
>
> Run this from inside a wiki folder, pass the folder as an argument, or create a
> new wiki with `/wiki-init <folder-name> "<topic>"`.

## Step 1 — Count pages per category

Count the `.md` files in each of:

- `wiki/concepts/`
- `wiki/entities/`
- `wiki/sources/`

Exclude `.gitkeep` and any non-markdown files. Also note the total.

## Step 2 — Read recent activity

Read `wiki/log.md` and pull the **last 5 entries** (the most recent ones — the log
appends newest at the bottom). If there are fewer than 5, show all of them. If
`wiki/log.md` is missing, say so and continue.

## Step 3 — Find orphan pages

An **orphan** is a page under `wiki/` that nothing links to from `wiki/index.md`.

1. Read `wiki/index.md`.
2. List every `.md` file under `wiki/concepts/`, `wiki/entities/`, and
   `wiki/sources/`.
3. A page counts as linked if `index.md` references it by filename, by relative
   path, or by `[[wikilink]]` matching its name or title.
4. Everything else is an orphan.

Do not count `index.md`, `log.md`, or `overview.md` — those are structural files,
not catalog entries.

## Step 4 — Report

Print a compact report:

```text
Wiki: <folder>
Topic: <topic from CLAUDE.md>

Pages
  concepts   <n>
  entities   <n>
  sources    <n>
  total      <n>

Recent activity (last 5 log entries)
  <entry>
  <entry>
  ...

Orphan pages (in wiki/, not linked from index.md)
  <path>
  ...
```

If there are no orphans, print `Orphan pages: none` instead of an empty list.

Close by stating plainly that nothing was modified. If orphans were found, offer
to add them to `index.md` — but only as an offer. Wait for the user to ask before
touching anything.
