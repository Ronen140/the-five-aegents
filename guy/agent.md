# Guy — Quality Assurance (pointer)

> **This is a human-facing pointer file. Do not edit Claude behavior here.**
>
> Canonical agent definition: [`.claude/agents/guy.md`](../.claude/agents/guy.md).
> That is the file Claude Code loads as a sub-agent — frontmatter + body, single source of truth.

## What lives in this folder

| Path | Purpose |
|---|---|
| `QA_Reports/` | Persistent log of every QA review Guy has run. Each report is an atomic record — written once, never edited in place, never deleted. Filename pattern: `<YYYY-MM-DD-HHMM>-<slug>.md`. Greppable for trend analysis ("how often did round 1 fail in May?"). Owned by Guy — humans should not edit (but reading is fine). |
| `agent.md` | This file. |

## What Guy does (one paragraph)

Receives a final deliverable from Reuven (always at `Output/<filename>.md`), the original brief that the user gave Reuven, and a round number (#1, #2, or #3). Reads the deliverable, optionally reads `yael/style-guide.md` and `chen/Memory/searches.md` for cross-checks, runs a fixed 5-section checklist (brief relevance, style/branding, structural completeness, image integrity, technical correctness), writes a structured report to `guy/QA_Reports/<YYYY-MM-DD-HHMM>-<slug>.md`, and returns one of two outcomes to Reuven: `✅ approved` (deliverable goes to the user) or `❌ revision required` (Reuven re-runs Yael with Guy's notes, then re-submits to Guy as round N+1).

## Why he doesn't fix things himself

Two reasons stack on top of each other:

1. **Architectural constraint.** In Claude Code, a sub-agent cannot invoke another sub-agent — only Reuven (the top-level agent) can. So even if Guy wanted to send the deliverable back to Yael for a fix, he can't. He hands the report to Reuven; Reuven re-runs Yael.
2. **Separation of concerns.** Guy is read-mostly **on purpose**. His tools are `Read, Glob, Grep, Write` — `Write` only for reports, never for the deliverable. Mixing judgment with editing causes confused responsibility ("did Guy approve it because it was good, or because he fixed it himself?"). Keeping the loop clean — Yael edits, Guy judges, Reuven coordinates — makes failures easy to attribute and fix.

## Round limit

Up to 3 rounds. If round 3 still fails QA, **Reuven does NOT auto-rerun Yael for a 4th time.** Instead, Reuven shows the user the deliverable + Guy's report and asks for a manual decision: force-approve, hand-fix, or cancel. This caps the worst-case loop at 3 attempts and prevents infinite churn when Yael and Guy can't converge.

## Why his approval is binding

Guy is the only agent in the system permitted to reject a deliverable. Without his ✅, the loop doesn't close — Reuven will not show the deliverable to the user. This is intentional: the project's quality bar is enforced at one well-defined gate, not negotiated across the pipeline.
