# Chen — Web Researcher (pointer)

> **This is a human-facing pointer file. Do not edit Claude behavior here.**
>
> Canonical agent definition: [`.claude/agents/chen.md`](../.claude/agents/chen.md).
> That is the file Claude Code loads as a sub-agent — frontmatter + body, single source of truth.

## What lives in this folder

| Path | Purpose |
|---|---|
| `Memory/searches.md` | Persistent log of every web search Chen has run, with queries, candidates, quality ratings, and the chosen source. Chen greps this before every new search to avoid repeating work. Owned by Chen — humans should not edit (but reading it is fine). |
| `agent.md` | This file. |

## What Chen does (one paragraph)

Receives a topic from Reuven (e.g. "מצא לי מאמר על interpretability ב-LLMs"), checks `chen/Memory/searches.md` for a recent matching search, and either reuses the prior file or runs a fresh search. A fresh run is: 2-4 web queries (`WebSearch`), filter by quality (primary sources, recognized publications, recent date), fetch the best candidate (`WebFetch`), save the raw content to `Content/<YYYY-MM-DD>-<slug>.md` with a metadata header (source URL, author, publish date, quality stars), append an entry to `Memory/searches.md`, and report back to Reuven with the file path + 1-2 sentence source summary. She does **not** rewrite content (that's Yael) and does **not** trigger Yael directly — she just stages the input file and Reuven decides next steps.

## Why hybrid layout (`.claude/agents/chen.md` + `chen/`)

Claude Code only discovers **flat files** in `.claude/agents/`, not subdirectories — so the canonical agent definition has to live as a flat file. But Chen needs persistent state (her search log), and that lives in `chen/Memory/searches.md`. The two halves are linked by convention, not by code. Same pattern as Yuval (`yuval/`) and Yael (`yael/`).

## Why she stages, not invokes

In Claude Code, **a sub-agent cannot invoke another sub-agent** — only Reuven (running as the top-level agent) can. So Chen drops the article in `Content/`, reports the path back to Reuven, and Reuven decides whether to chain Yael for a rewrite or stop and return the raw source to the user. The same architectural constraint applies to Yael's `{{IMAGE_NEEDED: "..."}}` placeholders for Yuval.

## Memory hygiene

`Memory/searches.md` grows monotonically. It is checked into git so future sessions inherit Chen's history. If it ever gets large enough to slow down `Read` — split by year (`searches-2026.md`, `searches-2027.md`) and update Chen's Phase 1 to Glob the directory rather than a single file. Don't compact or summarize — the value of the log is precisely its raw chronological detail.
