# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**the-five-aegents** is a content-creation agent team. A CEO agent orchestrates the work and delegates tasks to a team of specialized sub-agents. The agent roster and their responsibilities will be defined as the project evolves.

## Project-Specific Claude Configuration

The `.claude/` directory holds configuration that extends Claude Code for this project:

- `.claude/agents/` — sub-agent definitions (each file describes a specialized agent's role, tools, and instructions)
- `.claude/skills/` — reusable skill prompts that any agent in the team can invoke
- `.claude/commands/` — custom slash-commands for common project workflows
