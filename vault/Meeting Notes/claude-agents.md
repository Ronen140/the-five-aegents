# .claude/agents/

## Overview

`.claude/agents/` is the folder where project-specific sub-agent definitions live. Each agent file describes one specialized agent's role, tools, and instructions — Claude Code loads these as available `subagent_type` options for the Agent tool.

**Currently empty** — only a `.gitkeep` placeholder. The CEO + sub-agent roster will be defined later as the content-creation team takes shape.

Conceptual roster (to be authored): a CEO orchestrator agent that delegates to specialized sub-agents (e.g. researcher, writer, editor, publisher). The exact split is an open question.

## Open Questions

- Define the CEO agent first, or define the sub-agents first and let the CEO agent's prompt aggregate them?
- How many sub-agents — 3, 5, or more?
- Will sub-agents share tools, or each get a restricted toolkit?

## Session Log

### 2026-05-06 — folder created, awaiting agents [planned]
- **What was done:** Folder created with `.gitkeep` placeholder. No agent files yet.
- **Decisions:** Postpone agent design until the content workflow is sketched out. Cart before horse otherwise.
- **Notes / Caveats:** Agent files use `.md` with frontmatter `name`, `description`, `tools`. See Anthropic docs for full schema before authoring the first one.
- **Related:** [[project-file-inventory]], [[claude-commands]], [[claude-skills]], [[claude-md]]
