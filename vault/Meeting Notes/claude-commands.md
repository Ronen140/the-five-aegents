# .claude/commands/

## Overview

`.claude/commands/` is the folder for custom slash-commands scoped to this project. Each `.md` file becomes a `/<name>` command available in Claude Code sessions.

**Currently empty** — only a `.gitkeep` placeholder. Likely candidates for future commands: a content-brief generator, a publish trigger, a CEO-agent kickoff.

The Superpowers plugin in `.claude/skills/` already covers a wide swath of dev workflows (TDD, plans, code review, debugging). New slash-commands should be reserved for project-specific orchestration that those skills don't already provide.

## Open Questions

- Which orchestration steps benefit from a slash-command vs. just being baked into the CEO agent's prompt?
- Will commands be authored once the agent roster stabilizes, or earlier as scaffolding?

## Session Log

### 2026-05-06 — folder created, awaiting commands [planned]
- **What was done:** Folder created with `.gitkeep` placeholder. No command files yet.
- **Decisions:** Defer custom commands until at least one workflow is concrete enough to warrant the abstraction.
- **Notes / Caveats:** Slash-commands take arguments via `$ARGUMENTS`. Keep commands short — heavy logic belongs in skills, not commands.
- **Related:** [[project-file-inventory]], [[claude-agents]], [[claude-skills]], [[superpowers-skills]]
