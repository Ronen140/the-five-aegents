# Superpowers Skills

## Overview

14 skills installed manually from [obra/superpowers](https://github.com/obra/superpowers) into `.claude/skills/`. They cover planning, debugging, testing, code review, and Git workflows. Owned upstream — don't fork unless we need a project-specific variant.

| Skill | Purpose |
|---|---|
| `brainstorming` | Visual collaborative brainstorming server with frame templates |
| `dispatching-parallel-agents` | Patterns for fanning out work across multiple subagents |
| `executing-plans` | Execute a plan file step-by-step, verifying after each step |
| `finishing-a-development-branch` | Wrap-up checklist before merging a feature branch |
| `receiving-code-review` | How to respond to review comments productively |
| `requesting-code-review` | Prepare a branch for review; includes a `code-reviewer` sub-prompt |
| `subagent-driven-development` | Implementer + spec-reviewer + quality-reviewer triad |
| `systematic-debugging` | Root-cause tracing, defense-in-depth, condition-based waiting |
| `test-driven-development` | TDD discipline + testing anti-patterns reference |
| `using-git-worktrees` | Use git worktrees to parallelize work without branch-thrash |
| `using-superpowers` | Meta-skill — how the Superpowers system fits together |
| `verification-before-completion` | Don't claim done until you've verified end-to-end |
| `writing-plans` | Author plan files with reviewer prompt |
| `writing-skills` | Author new skills with anthropic-best-practices reference |

The `using-superpowers` skill is the entry point — read it once to understand how the others compose.

## Open Questions

- none

## Session Log

### 2026-05-06 — installed via manual clone-and-copy [shipped]
- **What was done:** Cloned `obra/superpowers` to a temp dir, copied `skills/` contents into `.claude/skills/`, removed the temp clone. Commit `7ce46c4`.
- **Decisions:** Manual install rather than `/plugin` because the plugin system isn't available on this machine. No agents or commands were copied — the source repo doesn't ship those folders.
- **Notes / Caveats:** Skills updates aren't automatic. If upstream releases changes, re-running the clone-and-copy is required (and any local edits would be overwritten).
- **Related:** [[claude-skills]], [[obsidian-skills]], [[project-file-inventory]]
