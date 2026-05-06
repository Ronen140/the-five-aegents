# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**the-five-aegents** is a content-creation agent team. A CEO agent orchestrates the work and delegates tasks to a team of specialized sub-agents. The agent roster and their responsibilities will be defined as the project evolves.

## Project-Specific Claude Configuration

The `.claude/` directory holds configuration that extends Claude Code for this project:

- `.claude/agents/` — sub-agent definitions (each file describes a specialized agent's role, tools, and instructions)
- `.claude/skills/` — reusable skill prompts that any agent in the team can invoke
- `.claude/commands/` — custom slash-commands for common project workflows

## Vault Workflow — MANDATORY

**At the start of every session and every new task, invoke the `obsidian-vault-workflow` skill (in `.claude/skills/obsidian-vault-workflow/SKILL.md`) before doing anything else.**

This is non-negotiable. The skill defines the project's long-term memory protocol:

1. **Before working** — name the topic in one phrase, locate `vault/Meeting Notes/<topic>.md` (or use `_index.md` to find it), and read its Overview + Open Questions + every prior Session Log entry. Also scan recent Meeting Notes / Content Briefs / Brand Guidelines.
2. **After working** — append a `### YYYY-MM-DD — <title> [status]` entry at the bottom of the topic's Session Log, update Open Questions (add new, remove resolved), and refresh the Overview only if scope/status/understanding changed. Verify by reading the file back.

The vault lives at `vault/`. The entry index for architecture/decision topics is `vault/Meeting Notes/_index.md`. Read the [[obsidian-vault-workflow]] skill itself for the full protocol, file format, status tags, and anti-patterns.

**Skip the workflow only for pure read-only questions that touch zero files and produce zero decisions.** Anything else — coding, content, architecture, design, bugfixes, reviews — goes through Phase 1 → work → Phase 2.
