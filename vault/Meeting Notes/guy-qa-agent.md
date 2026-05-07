# Guy — QA Agent

## Overview

גיא (Guy) is the project's Quality Assurance sub-agent — Agent 4 in the four-agent roster, but **last in the logical content pipeline**. Every final output produced by the pipeline (chen → yael → yuval → yael) routes through him before delivery to the user. He runs a fixed 5-section checklist (brief relevance, style/branding, structural completeness, image integrity, technical correctness), writes a structured report to `guy/QA_Reports/<YYYY-MM-DD-HHMM>-<slug>.md`, and returns ✅ approved or ❌ revision required to Reuven. He is read-mostly: tools are `Read`, `Glob`, `Grep`, plus `Write` (reports only) — he never edits the deliverable itself. He is also the **only agent in the system permitted to reject a deliverable** — without his approval nothing reaches the user. Maximum 3 review rounds; if round 3 still fails, Reuven escalates to the user for manual decision.

## Open Questions

- מה קורה אם בריף מקורי מעורפל ולא ניתן לאמת רלוונטיות? כרגע ההגדרה כוללת מסלול חריג: "אישור מותנה — דרושה הבהרה מהמשתמש". לבחון בפועל אם ראובן יודע לטפל בזה או צריך פרוטוקול נוסף.
- האם להוסיף מאוחר יותר `Output/Approved/` folder שראובן מזיז אליו תוצרים שעברו QA, מקביל ל-`Content/Ready/`? כרגע לא — נחיה בלי, נראה אם נוצר צורך.
- האם פורמט הדוח (markdown עם checkboxes) ימשיך להיות מתאים אם נבנה מאוחר יותר אוטומציה שצורכת אותו (למשל אגרגציה של מגמות איכות)? אם כן, נצטרך להוסיף frontmatter YAML.

## Session Log

### 2026-05-07 — agent created, pipeline closed-loop shipped [shipped]

- **What was done:** Created `.claude/agents/guy.md` (canonical sub-agent definition: name=guy, model=sonnet, tools=Read/Write/Glob/Grep), `guy/` working directory (`QA_Reports/.gitkeep` + `agent.md` pointer doc), and updated `.claude/agents/reuven.md` with four targeted edits — hard-rule #2 rewritten to reflect the new logical pipeline order (`chen → yael → yuval → yael → guy`), table row #4 filled with guy's full row, status line updated to "כל ארבעת הסוכנים מוגדרים ופעילים", and a new `## פרוטוקול QA Loop — סוגר הלולאה` section appended before the closing rule. Added `[[guy-qa-agent]]` line to `_index.md`.
- **Decisions:**
  - Guy is read-mostly **by design** — his judgment role is kept clean from his (non-existent) editing role. Mixing them would cause confused responsibility. Tools are deliberately `Read/Glob/Grep + Write` (Write only for reports).
  - Guy returns reports to Reuven, never to Yael directly. This is forced by the Claude Code architectural constraint that sub-agents cannot invoke sub-agents — but it's also the correct design: keeps Reuven as the sole point-of-control for pipeline coordination.
  - Cap at 3 rounds. Round-3 fail → Reuven asks the user (not auto-retry to round 4). Prevents infinite Yael↔Guy churn on disputes they can't converge on.
  - Guy runs **automatically** at the end of every content pipeline (no explicit user trigger required), but **only** for deliverables in `Output/` — pure research requests that stop after Chen skip QA entirely.
  - Trigger keywords on Guy are bilingual (Hebrew + English) and double as both routing keywords and explicit user-invokable verbs ("בדוק", "QA", "verify", etc.).
- **Notes / Caveats:**
  - The QA report format is hand-written markdown with checkboxes, not JSON. Chosen for human readability — the reports are meant to be skimmed by the user when reviewing a rejection. If we later build automation that consumes reports (e.g., quality-trend dashboard), we'll add a YAML frontmatter prefix without changing the body format.
  - The ⚠️ "ambiguous brief" outcome (neither ✅ nor ❌) is a third path Reuven needs to handle — flagged in Open Questions for follow-up validation in the first real run.
- **Related:** [[reuven-ceo-agent]], [[yael-content-writer]], [[chen-web-researcher]], [[yuval-creative-agent]], [[claude-agents]]
