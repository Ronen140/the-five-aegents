# יעל — Yael Content-Writer Agent

## Overview

`יעל` (Yael) is the project's **content-writing sub-agent**, sitting under [[reuven-ceo-agent|Reuven]] as Agent 2 in the four-agent pipeline. She owns the textual pipeline: every rewrite, edit, summary, or translation request in the project routes through her. Her job is voice consistency — she scans `yael/style-guide.md` and `yael/reference/**/*.md` once per session, internalizes the project's voice, then rewrites raw articles from `Content/<name>.<ext>` into that voice and saves the result to `Output/<original-name>.md`.

**LLM-only by design.** Toolset: `Read, Write, Edit, Glob, Grep` — explicitly **no Bash**, no API access, no internet. She cannot move, copy, or delete files. When she finishes, she reports the source path back to Reuven so Reuven can move `Content/<name>.<ext>` → `Content/Ready/`.

**Image placeholders, not direct calls.** Where the rewrite would benefit from an image, Yael drops `{{IMAGE_NEEDED: "<English description>"}}` markers in the text. She does not call Yuval — Claude Code does not allow sub-agents to invoke other sub-agents, only Reuven can. Reuven's protocol picks up the placeholders post-rewrite, calls Yuval for each one, and substitutes the image references back into the final document.

**Hybrid layout.** Canonical agent definition at `.claude/agents/yael.md` (flat file — required by Claude Code's discovery rules). Working directory at `yael/` holds `style-guide.md`, `reference/`, plus `agent.md` — pointer doc for humans that does **not** affect Claude behavior. Same pattern as Yuval.

Pipeline-level folders (`Content/`, `Content/Ready/`, `Output/`) sit at project root with capitalized names — distinguishing shared pipeline directories from agent-private working dirs (`yael/`, `yuval/`).

## Open Questions

- **Reference subfolders.** Should `yael/reference/` allow subfolders by content type (`reference/blog/`, `reference/linkedin/`, `reference/newsletter/`) so Yael can pull style banks per request type? Currently flat. Decide once reference count grows past ~5 samples.
- **Image-density default.** Is there a default rule for how many images per article (e.g. "one per ~400 words", "hero + section dividers")? Currently — Yael uses judgment. Define after a few real runs reveal patterns.
- **Yuval mid-pipeline error.** If Yuval fails on one of N placeholders, what's the playbook — partial output with placeholder retained? Skip and continue? Currently Reuven decides case-by-case. Define after the first real failure.
- **Non-markdown input formats.** Does Yael support PDF or `.docx` in `Content/`? Currently markdown/txt only — `Read` does support PDF up to 10 pages, but Yael's hard rules don't address it. Revisit if a PDF source comes in.
- **Output overwrite behavior.** If `Output/<name>.md` already exists (re-run), Yael overwrites. Should iterations save as `Output/<name>-v2.md` instead? Currently overwrite — simpler. Reconsider once we hit a case where the v1 was needed for diff.
- **Image alt-text length.** Reuven's protocol says alt = "3-6 words from Yael's description." That's a rule of thumb, not a constraint. Tighten or loosen after a few real runs.

## Session Log

### 2026-05-06 — agent created, full pipeline integration shipped [shipped]
- **What was done:** Authored `.claude/agents/yael.md` (Hebrew body, bilingual description with Hebrew + English triggers, full 7-step workflow, hard rules, placeholder protocol, role boundaries; `tools: Read, Write, Edit, Glob, Grep` — no Bash). Created `yael/` working directory with `style-guide.md` (full stub: Voice & Tone / Vocabulary / Structure / Openings & Closings / Do / Don't / Examples — humans fill manually), `reference/.gitkeep`, and `agent.md` pointer doc. Created pipeline folders at project root: `Content/`, `Content/Ready/`, `Output/` (each with `.gitkeep`). Updated `.gitignore` to ignore `Output/*` (matching the `yuval/outputs/*` pattern). Updated [[reuven-ceo-agent|ראובן]]: added Yael to the `Sub-Agents Under Your Command` table (Agent 2) with full bilingual triggers; updated Status to "Agents 1-2 active, 3-4 TBD"; added a new `## פרוטוקול טיפול ב-placeholders של תמונות מ-יעל` section spelling out the 8-step flow Reuven runs after Yael returns (read output → regex-extract placeholders → loop-call Yuval → swap in markdown image refs → save final → move source to Ready/ → report → log to vault); softened the "single agent direct routing" note to also allow text-only requests directly to Yael. Added line for Yael to `vault/Meeting Notes/_index.md`.
- **Decisions:**
  1. **LLM-only is the load-bearing rule.** No Bash means Yael cannot move files, even though the user's brief said she should. Resolved by making file-move a Reuven responsibility (it fits Reuven's orchestrator role anyway, and keeps Yael's tool surface minimal). Yael's report explicitly tells Reuven: `⚠️ NEXT STEP for Reuven: move Content/<x> → Content/Ready/`.
  2. **Placeholders, not direct sub-agent calls.** Claude Code architecturally does not let a sub-agent invoke another sub-agent — only the top-level (Reuven) can. The `{{IMAGE_NEEDED: "..."}}` placeholder pattern is the workaround: Yael marks intent, Reuven materializes it. This is documented prominently in both Yael's `agent.md` and Reuven's protocol.
  3. **Placeholder description = English, content-only, no style.** Yael describes *what should be visible* (subject, action, mood); Yuval owns *how it looks* (style, palette, composition). Drawing this line cleanly is what keeps the agents' responsibilities non-overlapping. Examples included in `yael.md` show good vs. bad descriptions.
  4. **Output filename = `<original-name>.md`, no date.** Lets the source ↔ rewrite mapping be obvious by name alone. Different from Yuval's `<YYYY-MM-DD>-<slug>.png` convention because rewrites have a 1:1 mapping with sources, while images don't.
  5. **Style guide as a stub, not empty.** User explicitly chose this — the section headers (Voice & Tone / Vocabulary / Structure / Openings & Closings / Do / Don't / Examples) are scaffolding so the user knows what to fill, even when reference/ is empty. Yael's first-session instruction handles all three states (style-guide present, references present, both empty).
  6. **Pipeline folders capitalized at root (`Content/`, `Output/`).** Per user spec. Reads as "shared pipeline I/O" vs. lowercase agent-private dirs (`yael/`, `yuval/`). Note: on Windows Git is case-insensitive by default — folder naming is cosmetic but consistent.
  7. **Reuven's protocol is in his own file, not Yael's.** Yael does not need to know what Reuven does next; she just needs to leave a clean handoff. Keeps each agent's concerns local.
- **Notes / Caveats:**
  - The `{{IMAGE_NEEDED: "..."}}` format is convention, not enforced by tooling. If a future agent uses a slightly different format (`{IMAGE: ...}`, or curly braces with single-quotes), Reuven's regex won't match and the placeholders will leak into the final output. The regex `\{\{IMAGE_NEEDED:\s*"([^"]+)"\}\}` requires double-quotes specifically — documented in Reuven.
  - When a placeholder description contains characters that need escaping in markdown (e.g. quotes inside the description), the regex above will under-capture. First real article with quoted dialogue in a placeholder will reveal this; deal with it then.
  - Reuven's protocol assumes Yuval saves to `yuval/outputs/<YYYY-MM-DD>-<slug>.png` and returns that path. Yuval's contract already does this, but if Yuval's output protocol changes, Reuven's path-resolution at step 4 needs updating.
  - The relative path `../yuval/outputs/<file>.png` from `Output/<name>.md` works when both are at project root. If `Output/` ever gets moved (e.g. nested under `pipeline/`), the path math changes — Reuven's protocol would need a recompute.
  - First end-to-end test (a real article in `Content/`, with at least one placeholder) hasn't run yet. Until it does, the contract between Yael → Reuven → Yuval is design-only. Plan to run a smoke test on a tiny article (e.g. 200-word note) before any real content goes through.
- **Related:** [[reuven-ceo-agent]], [[yuval-creative-agent]], [[claude-agents]], [[obsidian-vault-workflow]], [[gpt-image-gen-skill]]
