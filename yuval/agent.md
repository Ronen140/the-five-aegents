# Yuval — Creative Agent (pointer)

> **This is a human-facing pointer file. Do not edit Claude behavior here.**
>
> Canonical agent definition: [`.claude/agents/yuval.md`](../.claude/agents/yuval.md).
> That is the file Claude Code loads as a sub-agent — frontmatter + body, single source of truth.

## What lives in this folder

| Path | Purpose |
|---|---|
| `reference/` | Inspiration images. Drop reference assets here. Yuval scans this folder before every generation and extracts style/palette/composition. |
| `outputs/` | Generated images. Each one is `<YYYY-MM-DD>-<slug>.png` plus a sibling `.txt` containing the exact prompt used (for iteration). |
| `agent.md` | This file. |
| `skill.md` | Pointer to the OpenAI image-gen skill Yuval calls. |

## What Yuval does (one paragraph)

Receives a request like "צור תמונה של חתול קורא ספר", scans `yuval/reference/`, extracts the visual language (style, palette, composition, mood), drafts an English prompt that fuses the request with the extracted style, calls the `gpt-image-gen` skill, saves the PNG + a `.txt` with the prompt to `outputs/`, and reports back which references influenced the result.

## Why hybrid layout (`.claude/agents/yuval.md` + `yuval/`)

Claude Code only discovers **flat files** in `.claude/agents/`, not subdirectories — so the canonical agent definition has to live as a flat file. But Yuval needs a working directory for inputs/outputs, and that directory is most discoverable at the project root. The two halves are linked by convention, not by code.
