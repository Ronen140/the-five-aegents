# .obsidian/

## Overview

`.obsidian/` is the per-vault config directory created automatically when Obsidian opens a folder as a vault. Contents:

- `app.json` — global app preferences for this vault
- `appearance.json` — theme/font preferences
- `core-plugins.json` — which built-in plugins are enabled
- `workspace.json` — current pane layout and open files

These are user-machine-local preferences, not project-essential. They got committed to Git in the initial scaffolding pass — likely should be moved to `.gitignore` so each developer keeps their own UI state.

## Open Questions

- Add `.obsidian/workspace.json` (and possibly the whole `.obsidian/` folder) to `.gitignore`? `workspace.json` churns every time the user opens a different note.
- Is there a baseline plugin set we want to commit so every contributor has the same Obsidian experience?

## Session Log

### 2026-05-06 — documented existing config [shipped]
- **What was done:** Catalogued the four `.obsidian/` config files. No config changes made.
- **Decisions:** Defer the gitignore decision until a second contributor joins — at one developer the noise is harmless.
- **Notes / Caveats:** If a future committer's Obsidian crashes, `workspace.json` may contain corrupted JSON. Recovery: delete the file; Obsidian regenerates it on next launch.
- **Related:** [[obsidian-skills]], [[env-config]], [[project-file-inventory]]
