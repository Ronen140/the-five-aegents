# .claude/skills/

## Overview

`.claude/skills/` holds 17 skills available to every Claude Code session in this project:

- **14 from Superpowers** — see [[superpowers-skills]] for the full list and what each does.
- **3 Obsidian-related** — `obsidian-bases`, `obsidian-markdown`, `obsidian-vault-workflow`. See [[obsidian-skills]].

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
