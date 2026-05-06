# gpt-image-gen — Skill

## Overview

`gpt-image-gen` is a **project-internal skill** that wraps `POST https://api.openai.com/v1/images/generations`. It is the first skill written *inside* this project (vs. the 18 third-party skills installed from Superpowers / anthropics / Obsidian). The skill does NOT author prompts — it accepts a finalized prompt + output path and writes a PNG to disk. Reads `OPENAI_API_KEY` from `.env`.

Two execution paths: **Path A** uses `jq` (Linux/macOS), **Path B** uses Python's stdlib (`urllib.request` + `base64`) and is the project default because Git Bash on Windows ships without `jq`. Both paths POST the same JSON body (`model: gpt-image-2`, `size: 1024x1024`, `quality: medium`, `output_format: png`), parse `data[0].b64_json`, decode, and verify the file exists with size > 0.

Currently called by: [[yuval-creative-agent|Yuval]]. Designed for reuse — any future agent that needs an image calls this skill rather than reaching for the OpenAI endpoint directly.

File: `.claude/skills/gpt-image-gen/SKILL.md`.

## Open Questions

- **Model name = `gpt-image-2`** per user direction. As of 2026-05, OpenAI's documented model is `gpt-image-1`; `gpt-image-2` may be a project-internal alias, an unreleased model, or a typo. If the API returns `model_not_found` on first live call, this is the place to revisit. User explicitly chose `gpt-image-2` after being asked.
- **No retry logic.** A single transient 429/500 fails the whole call. Add a small retry loop (max 2, exponential backoff) once we see a real failure rate.
- **Cost control.** No limit on calls per session. Worth defining once usage patterns emerge.
- **Variations.** Single-image-per-call only. The OpenAI API supports `n: 1..10`; not exposed yet.

## Session Log

### 2026-05-06 — skill created, Python-fallback default [shipped]
- **What was done:** Wrote `.claude/skills/gpt-image-gen/SKILL.md` with frontmatter (English description, "use when an agent has a ready-to-send prompt"), inputs spec (`PROMPT`, `OUT`, optional `SIZE`, `QUALITY`), pre-flight checks, both execution paths (jq + Python), post-flight file verification, error-handling table, and a worked example. Added `OPENAI_API_KEY` to `.env` and `.env.example`. Brought the skill count from 18 to 19, with the new bullet noting it's the first project-authored one.
- **Decisions:**
  1. **Python fallback is the recommended path on this machine.** Confirmed during planning: `which jq` returns nothing on Git Bash (Windows). `python` and `base64` are present. The skill documents both paths but instructs callers to default to Path B until the project moves to a different shell.
  2. **Skill stays a wrapper, not a creative tool.** Yuval (or any caller) is responsible for the prompt. The skill does NOT do prompt engineering. Reasoning: composition is what makes images consistent across the project — it must live in the agent that has reference context, not in a generic skill that any caller can hit.
  3. **Fail loud on missing key.** If `OPENAI_API_KEY` is empty, the skill exits 1 *before* the API call (no silent default, no env-vars-from-elsewhere fallback). Mistakes shouldn't burn budget.
  4. **Detailed error surfaces.** On `HTTPError`, the skill prints the response body. On a 200 with no `data` (i.e. an `error` envelope), it prints `error.message`. Generic stack traces are useless when debugging API calls.
- **Notes / Caveats:**
  - The skill embeds Python via heredoc (`python - <<'PY'`). The single-quoted heredoc prevents shell expansion of `$` inside the script, so the script reads env vars via `os.environ` rather than shell interpolation. Keeps the script paste-able as-is.
  - `SIZE` and `QUALITY` defaults are encoded twice (frontmatter table + Path A/B blocks). If they change, all three places must update.
- **Related:** [[yuval-creative-agent]], [[claude-skills]], [[env-config]]
