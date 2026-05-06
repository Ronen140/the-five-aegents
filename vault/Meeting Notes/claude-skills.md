# .claude/skills/

## Overview

`.claude/skills/` holds 19 skills available to every Claude Code session in this project:

- **14 from Superpowers** — see [[superpowers-skills]] for the full list and what each does.
- **3 Obsidian-related** — `obsidian-bases`, `obsidian-markdown`, `obsidian-vault-workflow`. See [[obsidian-skills]].
- **1 from anthropics/skills** — `skill-creator`. See [[skill-creator-install]].
- **1 project-internal** — `gpt-image-gen` (the first skill written inside this project). See [[gpt-image-gen-skill]].

Skills are auto-loaded by Claude Code at session start. Each skill is one folder containing a `SKILL.md` (with YAML frontmatter `name` + `description`) plus optional reference files. Claude decides when to invoke a skill based on the description match against the user's task.

The most load-bearing skill in this project is **`obsidian-vault-workflow`** — it gates every task with a "read the relevant vault note before working, append a Session Log after." `CLAUDE.md` enforces it.

## Open Questions

- Will the project author its own skills (e.g. for content-creation flows), or stick to Superpowers + Obsidian for now?
- Any skills from Superpowers that should be removed because they conflict with project conventions? (none flagged yet)

## Session Log

### 2026-05-06 — initial skill installation documented [shipped]
- **What was done:** Documented the two installed skill sets. Cross-linked to per-set notes.
- **Decisions:** Treat skills as third-party reusables — don't fork them inside this project unless we genuinely need a custom variant.
- **Notes / Caveats:** Skill descriptions matter for triggering. If a skill isn't firing when expected, the description text in its frontmatter is usually the place to fix it.
- **Related:** [[project-file-inventory]], [[superpowers-skills]], [[obsidian-skills]], [[claude-md]]

### 2026-05-06 — added skill-creator from anthropics/skills [shipped]
- **What was done:** Installed `skill-creator` into `.claude/skills/skill-creator/` via manual clone-and-copy from `anthropics/skills`. Inventory grew from 17 to 18 skills. Updated this file's Overview count and added a third bullet for the new source.
- **Decisions:** Manual install — `claude` CLI is not on PATH on this machine, so the user-requested `claude plugin install` commands all hit `command not found` (exit 127). Same fallback pattern as the Superpowers install.
- **Notes / Caveats:** This brings the total skill sources to three (Superpowers, Obsidian, anthropics/skills). If the project keeps adding upstream skills one at a time, consider whether a single update script in `scripts/` would beat doing each one by hand.
- **Related:** [[skill-creator-install]], [[superpowers-skills]], [[obsidian-skills]]

### 2026-05-06 — first project-internal skill: gpt-image-gen [shipped]
- **What was done:** Authored `.claude/skills/gpt-image-gen/SKILL.md` — a thin wrapper around OpenAI's Images API used by [[yuval-creative-agent|Yuval]]. Skill count 18 → 19, with a new fourth source category ("project-internal") in the Overview. The skill exposes both `jq` and Python execution paths and documents Python as the project default (Git Bash on Windows has no `jq`).
- **Decisions:** Skill is a wrapper, NOT a creative tool — prompt authoring stays in the calling agent. This keeps style consistency anchored where the reference context lives. Fail-loud on missing `OPENAI_API_KEY` so mistakes don't burn budget.
- **Notes / Caveats:** Model name is `gpt-image-2` per user direction; OpenAI's documented model as of 2026-05 is `gpt-image-1`, so the first live call may need this revisited. End-to-end test pending real key + reference image.
- **Related:** [[gpt-image-gen-skill]], [[yuval-creative-agent]], [[env-config]]
