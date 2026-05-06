# יובל — Yuval Creative Agent

## Overview

`יובל` (Yuval) is the project's **creative/image-generation sub-agent**, sitting under [[reuven-ceo-agent|Reuven]] as Agent 1 in the four-agent pipeline. He owns the visual pipeline: every image request in the project routes through him. His job is consistency — he scans `yuval/reference/`, extracts the visual language (style, palette, composition, mood, repeating elements), fuses it with the user's request, drafts an English prompt, and calls the [[gpt-image-gen-skill|gpt-image-gen skill]] to produce a PNG. Output is saved to `yuval/outputs/<YYYY-MM-DD>-<slug>.png` with a sibling `.txt` containing the exact prompt used (so iteration is possible).

**Hybrid layout.** The canonical agent definition lives at `.claude/agents/yuval.md` (flat file — Claude Code only discovers flat files in `.claude/agents/`, not subdirectories). The working directory at `yuval/` (root of the repo) holds `reference/`, `outputs/`, plus `agent.md` and `skill.md` — pointer docs for humans that do **not** affect Claude behavior.

Tools: `Read, Write, Edit, Bash, Glob, Grep`. Model: `sonnet`.

## Open Questions

- **Smoke test pending.** End-to-end run requires a real `OPENAI_API_KEY` and at least one reference image. Until both are in place, the agent is "shipped but untested with a live API call."
- **Reference convention.** Should `yuval/reference/` allow subfolders (e.g. `reference/portraits/`, `reference/landscapes/`) so different request types can pull different style banks? Currently flat. Decide when reference count grows.
- **Quality / size defaults.** Locked to `1024x1024` + `medium` for cost. Revisit when we know production usage patterns — banner work might need `1536x1024` and `high`.
- **Permission for delete.** Yuval is forbidden from deleting `outputs/`. If `outputs/` becomes large and disk pressure hits, define a manual cleanup playbook (vs. giving Yuval permission).

## Session Log

### 2026-05-06 — agent created, hybrid layout shipped [shipped]
- **What was done:** Authored `.claude/agents/yuval.md` (Hebrew body, English description with bilingual triggers, full 7-step workflow, hard rules, slug protocol, role boundaries). Created `yuval/` working directory with `reference/.gitkeep`, `outputs/.gitkeep`, plus pointer docs `yuval/agent.md` and `yuval/skill.md`. Updated [[reuven-ceo-agent|ראובן]] with a `Sub-Agents Under Your Command` table (Yuval = Agent 1, slots 2-4 marked _TBD_), and softened his "no agents outside the fixed pipeline" rule to allow direct image-only requests to Yuval without running the full pipeline. `.gitignore` now excludes `yuval/outputs/*` (keeping `.gitkeep`).
- **Decisions:**
  1. **Hybrid file layout.** Canonical agent at `.claude/agents/yuval.md`; working dir at `yuval/`. Forced by Claude Code's flat-file discovery in the agents folder. The `agent.md`/`skill.md` files inside `yuval/` are explicitly labeled as human pointers so future contributors don't try to edit Claude behavior in the wrong place.
  2. **Style consistency is the load-bearing rule.** Every request scans `reference/` first; if empty, Yuval reports "no references" rather than inventing a random style. The MVP image goal isn't "best image" — it's "every image looks like a sibling of the others."
  3. **Prompts saved alongside outputs.** Each PNG gets a `.txt` twin with the exact prompt + reference filenames + extracted style lines. Without this, iteration is impossible. This is a hard rule, not a preference.
  4. **Allow direct routing to Yuval.** Standalone image requests bypass the four-agent pipeline. Reuven's PRD is now amended to permit this. When the full content pipeline materializes and Yuval is one of its stages, ordering re-applies.
- **Notes / Caveats:**
  - The `.txt` sibling file convention isn't enforceable by tooling — relies on Yuval following the rule. If a future agent calls `gpt-image-gen` directly without writing a `.txt`, that file is orphaned in `outputs/`.
  - Description in frontmatter mixes Hebrew + English to maximize trigger matching (Hebrew first, English fallback). If matching weakness shows up, switch to all-English description and put the Hebrew triggers in the body.
  - Reuven's table marks Agents 2-4 as `_TBD_` — until those PRDs land, "the pipeline" is effectively a pipeline-of-one.
- **Related:** [[gpt-image-gen-skill]], [[reuven-ceo-agent]], [[claude-agents]], [[env-config]]
