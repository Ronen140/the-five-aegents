# .claude/agents/

## Overview

`.claude/agents/` is the folder where project-specific sub-agent definitions live. Each agent file describes one specialized agent's role, tools, and instructions — Claude Code loads these as available `subagent_type` options for the Agent tool.

**Current roster:**

- `reuven.md` — **the CEO orchestrator** ([[reuven-ceo-agent]]). Single user-facing entry point. Delegates every task into a fixed serial pipeline of four sub-agents. Never executes work himself.

**Pending:** Agent 1, Agent 2, Agent 3, Agent 4 — the four sub-agents that Reuven orchestrates. Awaiting separate PRDs.

## Open Questions

- Define the four sub-agents (Agent 1–4): roles, tools, deliverables. PRDs pending.
- Once sub-agents exist, decide which (if any) are flagged as "skippable" in Reuven's failure-handling tree.
- CLAUDE.md doesn't yet point sessions at Reuven. PRD assigns that update to the developer; needs to land before pipeline routing works automatically.

## Session Log

### 2026-05-06 — folder created, awaiting agents [planned]
- **What was done:** Folder created with `.gitkeep` placeholder. No agent files yet.
- **Decisions:** Postpone agent design until the content workflow is sketched out. Cart before horse otherwise.
- **Notes / Caveats:** Agent files use `.md` with frontmatter `name`, `description`, `tools`. See Anthropic docs for full schema before authoring the first one.
- **Related:** [[project-file-inventory]], [[claude-commands]], [[claude-skills]], [[claude-md]]

### 2026-05-06 — first agent landed: Reuven CEO [shipped]
- **What was done:** Authored `.claude/agents/reuven.md` per the v1.0 PRD — Hebrew-throughout, 5-stage protocol (הבנה / בדיקת בהירות / תכנון / ביצוע / סיום), agent-handoff template, emoji reporting vocabulary, failure-handling tree. Folder is no longer empty.
- **Decisions:** Folded the [[obsidian-vault-workflow]] read/write protocol directly into Reuven's stage 1 and stage 5, rather than running them as parallel "every task must..." rules. Single source of truth for the workflow.
- **Notes / Caveats:** Reuven references "Agent 1–4" but those don't exist yet — running a real pipeline will fail at the first hop until the sub-agent PRDs land. Mock first.
- **Related:** [[reuven-ceo-agent]], [[claude-md]], [[obsidian-skills]]
