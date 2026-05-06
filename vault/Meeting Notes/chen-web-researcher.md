# חן — Chen Web-Researcher Agent

## Overview

`חן` (Chen) is the project's **web-research sub-agent**, sitting under [[reuven-ceo-agent|Reuven]] as Agent 3 in the registry. She owns the external pipeline: every request that needs current information, real sources, or content that doesn't already live inside an LLM's training data routes through her. Her job is *no hallucinations* — every quote, number, and claim comes with a real link.

**Tools:** `WebSearch, WebFetch, Read, Write, Edit, Glob, Grep` — explicitly **no Bash, no API access**. Web access is via Claude Code's WebSearch + WebFetch only.

**Persistent memory.** Chen maintains `chen/Memory/searches.md` — a chronological log of every search she's run with queries, candidates, quality ratings, and the chosen source. Before every new search she greps this file for prior matches. If a recent matching search exists for a *static* topic (concept explainers, evergreen guides), she proposes reusing the existing file rather than re-fetching. *Dynamic* topics (news, prices, statistics, releases) always trigger a fresh search regardless of cache.

**Stages, not invokes.** Chen does not call Yael — Claude Code does not allow sub-agent ↔ sub-agent invocation. She drops the raw article in `Content/<YYYY-MM-DD>-<slug>.md` with a metadata header (source URL, author, publish date, quality stars) and reports the path back to Reuven. Reuven then decides whether to chain Yael (if the user's original ask included rewriting) or stop and return the raw source.

**Pipeline order.** Logically the full content pipeline is `chen → yael → yuval` (research → rewrite → illustrate). The Reuven sub-agent table numbers them by historical onboarding order (1=yuval, 2=yael, 3=chen), not by execution order — a footnote in Reuven's table makes that explicit.

**Hybrid layout.** Canonical agent at `.claude/agents/chen.md` (flat file — required by Claude Code's discovery rules). Working directory at `chen/` holds `Memory/searches.md` plus `agent.md` — pointer doc for humans that does **not** affect Claude behavior. Same pattern as Yuval and Yael.

## Open Questions

- **Memory file size.** `chen/Memory/searches.md` grows monotonically. At what scale does Read start being slow / costly? Plan: split by year (`searches-2026.md`, `searches-2027.md`) when it crosses ~500 entries or ~50K lines, whichever first. Update Chen's Phase 1 to Glob the directory.
- **30-day cache window.** The "recent enough to reuse" threshold is 30 days for static topics. Is that right? Too short → re-fetches things that haven't changed; too long → stale references. Revisit after we have ~10 real searches.
- **Quality threshold.** Chen rejects sources rated below ⭐⭐⭐. If only ⭐⭐ candidates exist she reports "no quality source found." Is that correct, or should she stage with a warning? Currently: reject. Revisit if real-world topics keep coming up empty.
- **Hebrew vs English preference.** Default is English unless content is for Israeli audience. How does Reuven signal "this is for Israeli audience"? Currently — tone of the user request + topic context. May need an explicit flag in Reuven's handoff to Chen.
- **Long article handling.** Chen truncates articles >10K words to "core sections + note." What's "core"? Currently — judgment call. Define after we hit a real long-article case where the truncation matters.
- **Paywalled sources.** WebFetch may fail or return paywall stub. Chen's protocol says "try next candidate." But what if the best source is paywalled? Currently — skip. Could revisit if user specifically wants a paywalled source they have access to (would need a different fetch path).
- **AI-generated detection.** Chen's filter rejects "AI-generated generic content." This is a heuristic, not enforced. False positives/negatives possible. Refine the heuristics after a few real runs.

## Session Log

### 2026-05-06 — agent created, full pipeline researcher slot filled [shipped]
- **What was done:** Authored `.claude/agents/chen.md` (Hebrew body, bilingual description with Hebrew + English triggers, full 7-step workflow, hard rules, memory protocol, source-quality criteria, role boundaries). Tools: `WebSearch, WebFetch, Read, Write, Edit, Glob, Grep` — no Bash. Created `chen/` working directory with `Memory/searches.md` (initialized with header — first entries will append below) and `agent.md` pointer doc. Updated [[reuven-ceo-agent|ראובן]]: added Chen as Agent 3 in the sub-agent table with bilingual triggers, expanded the "agent order" footnote to explain that the registry numbering (1=yuval, 2=yael, 3=chen) is historical, not pipeline order — the actual content flow is `chen → yael → yuval`. Status bumped to "Agents 1-3 active, 4 TBD." Added a new `## פרוטוקול chen → yael — auto-continue או עצור` section to Reuven covering the three branching cases (research-only → stop and return; research+rewrite → auto-chain to Yael; ambiguous → ask user). Edge cases documented: no quality source → stop regardless; memory hit → ask user, don't decide. Added line for Chen to `vault/Meeting Notes/_index.md`.
- **Decisions:**
  1. **Persistent memory as a checked-in file, not a vault entry.** `chen/Memory/searches.md` lives next to the agent, not in `vault/`. Reasoning: vault is for *decisions* and *human-facing context*; the search log is *operational state* the agent reads on every run. Different access patterns, different files. The vault topic file (this file) describes *what* the search log is for, but doesn't duplicate its contents.
  2. **Quality filter is a hard gate.** Below ⭐⭐⭐ = no `Content/` file created. Reasoning: yael will rewrite whatever's in `Content/`, with no way to flag "the source was weak." Better to stop early and ask Reuven to redirect than to launder a bad source through the rest of the pipeline.
  3. **Raw content, not summarized.** The file Chen drops in `Content/` is the article as fetched (lightly cleaned of nav/ads). Reasoning: Yael's value-add is voice + structure rewriting; she can only do that with the full source material. Pre-summarization erases the raw material she works from.
  4. **Memory entry written even on failure.** "No quality source found" still gets logged. Reasoning: avoids re-running the same dead-end search. The Memory entry shows what was tried and why it failed, which informs future searches on related topics.
  5. **Reuven decides chen → yael, not Chen.** Chen reports done; Reuven branches based on the original user intent. Reasoning: keeps Chen's surface area small (she's a researcher, not a router) and concentrates orchestration logic in one place (Reuven). Same pattern as Yael's `{{IMAGE_NEEDED}}` placeholders + Reuven's image-resolution protocol.
  6. **30-day cache window for static topics.** Arbitrary but reasonable starting point. Listed in Open Questions to revisit with real data.
  7. **`Memory/` capitalized.** Mirrors `Content/` and `Output/` (capitalized = "real folders agents read/write to"), distinct from `reference/` inside `yuval/` and `yael/` (lowercase = "agent-private input dir"). Naming convention emerging across the project.
- **Notes / Caveats:**
  - First end-to-end test (a real research request through Chen) hasn't run yet. Until it does, the contract between user → Reuven → Chen → (optionally) Yael is design-only. Plan to smoke-test on a topic with a known good source (e.g. "interpretability research at Anthropic 2026") before any real content goes through.
  - WebSearch and WebFetch are deferred tools in Claude Code's tool registry — they need to be loaded via ToolSearch when invoked. Listed in the agent frontmatter so Claude Code knows to make them available, but at runtime the first call may pay the deferred-load cost.
  - Memory grep may have false negatives if the user's keywords don't match Chen's prior entries. Chen should grep multiple variants of the keyword before concluding "no prior match." Documented in the agent's Phase 1 step but not strictly enforced.
  - Content file naming: `<YYYY-MM-DD>-<slug>.md`. The date prefix helps with chronological sort but means two articles on the same topic on different dates produce different filenames — that's intentional (each fetch is a snapshot in time).
  - Article truncation for >10K words is a judgment call by Chen. Documented as "save core sections + note in metadata." Will revisit when we hit a real case where truncation matters.
- **Related:** [[reuven-ceo-agent]], [[yael-content-writer]], [[yuval-creative-agent]], [[claude-agents]], [[obsidian-vault-workflow]]
