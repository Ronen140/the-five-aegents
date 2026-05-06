# skill-creator Install

## Overview

`skill-creator` is Anthropic's official skill for **creating, editing, and evaluating other skills**. Source: [`anthropics/skills/skills/skill-creator/`](https://github.com/anthropics/skills/tree/main/skills/skill-creator). Installed at project scope into `.claude/skills/skill-creator/` so it's available to every session in this repo and travels with the codebase.

The skill ships with `SKILL.md` plus five subfolders (`agents/`, `assets/`, `eval-viewer/`, `references/`, `scripts/`). The agents and references are the skill's internal helpers — they are NOT general-purpose `.claude/agents/` definitions. Don't move them.

Triggering: invoked when the user wants to author a new skill, refactor an existing one, run evals, or tune a skill description for better matching.

## Open Questions

- none

## Session Log

### 2026-05-06 — installed via manual clone-and-copy [shipped]
- **What was done:** Tried the user-requested `claude plugin install ...` CLI commands first; they failed because `claude` is not on PATH (`command not found`, exit 127). Fell back to manual install: `git clone --depth=1 https://github.com/anthropics/skills.git`, then `cp -rn skills/skill-creator/ .claude/skills/skill-creator/`, then removed the temp clone. Same pattern as the Superpowers install.
- **Decisions:** Project scope chosen explicitly (per user). `.claude/skills/` IS the project-scope skills directory, so the copy itself satisfies the scope requirement — no separate marketplace registration needed. Did not commit the `agents/`/`references/` subfolders into `.claude/agents/` — they're skill-internal, not standalone agent definitions.
- **Notes / Caveats:** Updates aren't automatic. Future upstream changes require re-running the clone-and-copy. `cp -rn` was used so any local edits would survive a re-run.
- **Related:** [[superpowers-skills]], [[claude-skills]], [[project-file-inventory]]
