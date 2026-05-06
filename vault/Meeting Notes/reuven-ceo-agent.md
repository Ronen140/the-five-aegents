# ראובן — Reuven CEO Agent

## Overview

`ראובן` (Reuven) is the project's **CEO orchestrator agent** and the single user-facing entry point. The user talks only to Reuven; Reuven decomposes any task into a fixed serial pipeline of four sub-agents (Agent 1 → Agent 2 → Agent 3 → Agent 4) and is the only one allowed to invoke them. He never executes work himself — only manages, coordinates, summarizes, reports, and handles failures.

File: [`.claude/agents/reuven.md`](../../.claude/agents/reuven.md). Tools granted: `Task, Read, Write, Edit, Bash, Glob, Grep`. Model: `sonnet`.

Reuven's task protocol has 5 stages — הבנה (Understanding, includes mandatory vault read) → בדיקת בהירות (Clarity check) → תכנון (Plan presentation + approval) → ביצוע (Execution) → סיום (Wrap-up, includes mandatory vault write). Stages 1 and 5 fold the [[obsidian-vault-workflow]] protocol directly into Reuven's flow rather than treating it as a separate concern.

Reporting to the user uses a fixed emoji vocabulary: 🔄 starting a stage, ✅ stage done, ⚠️ failure, 🎯 pipeline complete.

## Open Questions

- Define the four sub-agents (Agent 1–4): roles, tools, deliverables. Out of scope for this PRD; awaits separate PRDs.
- Should Reuven persist intermediate state to `.claude/sessions/<session-id>/` once pipelines get long, or is the in-context buffer enough? PRD says session storage is optional for v1.
- CLAUDE.md doesn't yet point at Reuven explicitly. The PRD says the developer (user) will add that pointer manually. Until then, Claude Code may not automatically route every task through him — needs a CLAUDE.md update separately.

## Session Log

### 2026-05-06 — first agent definition created [shipped]
- **What was done:** Wrote `.claude/agents/reuven.md` per PRD v1.0 (Reuven CEO Orchestrator). Hebrew-throughout body content with the full 5-stage protocol, agent-handoff template, reporting vocabulary, and failure-handling tree. Frontmatter exactly per spec (`name: reuven`, Hebrew description, tools list, `model: sonnet`).
- **Decisions:**
  1. **Vault workflow integrated into Reuven's protocol, not stacked on top.** The PRD's stage 1 (הבנה) now explicitly includes the vault-read step; stage 5 (סיום) includes the vault-write + read-back verification. This avoids the conflict where two "every task must..." rules exist in parallel.
  2. **Hebrew body, Hebrew description.** Trade-off: agent description matching may miss English-only task phrasing, but the project is Hebrew-first per the PRD and consistency wins.
  3. **Kept the full tool grant** (`Task, Read, Write, Edit, Bash, Glob, Grep`) even though Reuven "doesn't execute" — he needs Read/Write/Edit for vault notes and for shaping context handoffs, Bash/Glob/Grep for vault navigation. Without these the workflow can't function.
- **Notes / Caveats:**
  - The four sub-agents don't exist yet. Until they do, Reuven will trigger the pipeline and immediately fail at "Agent 1" — that's expected. Test with mock subagents before adding real ones.
  - CLAUDE.md doesn't yet route every task through Reuven. The PRD assigns that update to the developer (user). Without it, sub-agent routing depends on Claude Code's description-based matching of Reuven's frontmatter.
  - The reporting protocol uses emojis (🔄✅⚠️🎯). If a downstream display strips emoji, the messages still parse — the labels (סוכן, תקציר, תוצר) carry the meaning.
- **Related:** [[claude-agents]], [[project-file-inventory]], [[claude-md]], [[obsidian-skills]]
