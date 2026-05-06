# Yael — Content Writer (pointer)

> **This is a human-facing pointer file. Do not edit Claude behavior here.**
>
> Canonical agent definition: [`.claude/agents/yael.md`](../.claude/agents/yael.md).
> That is the file Claude Code loads as a sub-agent — frontmatter + body, single source of truth.

## What lives in this folder

| Path | Purpose |
|---|---|
| `style-guide.md` | The team's writing style guide. Yael reads it at session start. Owned by humans — fill manually. |
| `reference/` | Sample articles in our voice (`.md` files). Yael reads them at session start to internalize the style. Drop reference texts here. |
| `agent.md` | This file. |

## What Yael does (one paragraph)

Pulls a raw article from `Content/<name>.<ext>`, scans `yael/style-guide.md` and `yael/reference/` once per session to internalize the project's voice, rewrites the article in that voice, drops `{{IMAGE_NEEDED: "..."}}` placeholders wherever an illustration would help the reader, saves the rewrite to `Output/<original-name>.md`, and reports back to Reuven with a list of placeholders. She is **LLM-only** — no Bash, no API, no internet. She does not move the source file; Reuven does that after she reports done.

## Why hybrid layout (`.claude/agents/yael.md` + `yael/`)

Claude Code only discovers **flat files** in `.claude/agents/`, not subdirectories — so the canonical agent definition has to live as a flat file. But Yael needs a working directory for her style guide and reference samples, and that directory is most discoverable at the project root. The two halves are linked by convention, not by code. Same pattern as Yuval's `.claude/agents/yuval.md` + `yuval/`.

## Why placeholders, not direct calls to Yuval

In Claude Code, **a sub-agent cannot invoke another sub-agent** — only Reuven (running as the top-level agent) can. So Yael leaves `{{IMAGE_NEEDED: "..."}}` markers in the rewrite, and Reuven walks the placeholders, calls Yuval for each one, and substitutes the resulting image references back into the final document.
