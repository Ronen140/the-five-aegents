# Obsidian Skills

## Overview

Three skills in `.claude/skills/` that govern how this project uses Obsidian as Claude's long-term memory:

| Skill | Purpose |
|---|---|
| `obsidian-vault-workflow` | **The mandatory protocol.** Before any task: locate the topic file in `vault/`, read Overview + Session Log + recent Meeting Notes. After any task: append a dated Session Log entry, update Open Questions, refresh Overview if scope changed, verify by reading back. |
| `obsidian-markdown` | Conventions for writing markdown that Obsidian renders correctly — wikilinks, callouts, embeds. |
| `obsidian-bases` | Working with Obsidian Bases (database-style views over notes). |

`obsidian-vault-workflow` is the load-bearing one — `CLAUDE.md` references it explicitly and every task in this project goes through its Phase 1 / Phase 2 cycle.

## Open Questions

- none

## Session Log

### 2026-05-06 — documented existing Obsidian skill set [shipped]
- **What was done:** Catalogued the three Obsidian-related skills already installed in `.claude/skills/`.
- **Decisions:** Don't author project-specific Obsidian skills — the upstream three cover what's needed.
- **Notes / Caveats:** `obsidian-vault-workflow` defines the file shape used across `vault/Meeting Notes/`. If you change the template, every existing note in this folder will look out-of-date until backfilled.
- **Related:** [[claude-skills]], [[superpowers-skills]], [[obsidian-config]], [[project-file-inventory]]
