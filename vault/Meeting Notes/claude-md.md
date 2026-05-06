# CLAUDE.md

## Overview

`CLAUDE.md` is the project-root file that Claude Code reads at the start of every session. It carries:

1. A short project description (CEO-led content-creation agent team)
2. Documentation of the `.claude/` subdirectories (`agents/`, `skills/`, `commands/`)
3. The mandatory **Vault Workflow** directive — every task must read the matching topic file in `vault/Meeting Notes/` before work and append a Session Log entry after.

This file is the single most important onboarding artifact: it points new sessions at the vault, which then carries the full project memory.

## Open Questions

- none

## Session Log

### 2026-05-06 — created and linked to vault workflow [shipped]
- **What was done:** Initial `CLAUDE.md` written with project description and `.claude/` folder map. Later in this session, added a `## Vault Workflow` section enforcing the [[obsidian-vault-workflow]] skill on every session/task.
- **Decisions:** Kept CLAUDE.md short — heavy guidance lives in skills and the vault, not here. CLAUDE.md is a pointer, not a manual.
- **Notes / Caveats:** Lives at project root. Affects every session. Edit with care; large changes ripple to every future task.
- **Related:** [[project-file-inventory]], [[claude-skills]], [[obsidian-skills]]
