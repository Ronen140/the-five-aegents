# Project File Inventory

## Overview

Top-level map of every file and folder in **the-five-aegents** — a content-creation agent team orchestrated by a CEO agent. The project is in scaffold stage: directory structure and tooling configs are in place, no application code yet. Each top-level component has its own dedicated topic file in this folder; this inventory note links to all of them and serves as the entry point for understanding the project layout.

## Open Questions

- Which CEO/sub-agent definitions go into `.claude/agents/` first?
- Will custom slash-commands be authored, or rely on Superpowers + built-ins?
- Will the project use Python or TypeScript for the agent runtime? (still undecided)

## Session Log

### 2026-05-06 — initial vault inventory [shipped]
- **What was done:** Created `vault/` with Meeting Notes folder. Wrote one topic file per top-level project component, plus an `_index.md` listing them all. Added `## Vault Workflow` directive to `CLAUDE.md` so every future session reads the matching topic file before working and appends a Session Log entry after.
- **Decisions:** Grouped 14 third-party Superpowers skills into a single topic note rather than one note per skill — they self-document via their own `SKILL.md`. Same for the three Obsidian skills. Per-skill notes would be redundant with upstream docs.
- **Notes / Caveats:** `.gitkeep` files inside `.claude/agents/` and `.claude/commands/` are placeholders — those folders are intentionally empty until project-specific agents and commands are defined.
- **Related:** [[claude-md]], [[env-config]], [[claude-settings]], [[claude-agents]], [[claude-commands]], [[claude-skills]], [[superpowers-skills]], [[obsidian-skills]], [[obsidian-config]]
