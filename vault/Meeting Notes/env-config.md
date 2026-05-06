# Env & Ignore Config

## Overview

Three files at the project root govern environment variables and what Git tracks:

- **`.env`** — local secrets (e.g. `ANTHROPIC_API_KEY`, `DEFAULT_MODEL`). Git-ignored. Filled in by the developer.
- **`.env.example`** — committed template that documents which env vars exist. Anyone cloning the repo copies this to `.env` and fills in real values.
- **`.gitignore`** — ignores `.env` and Yuval's generated `outputs/` (keeps the folder via `.gitkeep`). Will grow as build artifacts, caches, and dependency directories appear.

`DEFAULT_MODEL` is set to `claude-sonnet-4-6` in both `.env` and `.env.example`. `OPENAI_API_KEY` was added on 2026-05-06 for the [[gpt-image-gen-skill]].

## Open Questions

- Add Python venv / `node_modules` patterns to `.gitignore` once a runtime is chosen
- Decide whether to add other API keys (e.g. social-platform tokens for publishing) to `.env.example`

## Session Log

### 2026-05-06 — initial env scaffolding [shipped]
- **What was done:** Created `.env`, `.env.example`, and `.gitignore`. Committed `.env.example` and `.gitignore`; `.env` excluded from Git.
- **Decisions:** Default model pinned to `claude-sonnet-4-6` for consistency across agents. `.env.example` keeps comments above each variable so future contributors know what each one is for.
- **Notes / Caveats:** If a teammate ever commits a real secret by accident, the value must be rotated — Git history retention makes the leak permanent regardless of follow-up commits.
- **Related:** [[project-file-inventory]], [[claude-md]]

### 2026-05-06 — added OPENAI_API_KEY + yuval/outputs ignore [shipped]
- **What was done:** Added `OPENAI_API_KEY` block to `.env` (empty) and `.env.example` (with `your_openai_api_key_here` placeholder). Updated `.gitignore` to keep `yuval/outputs/.gitkeep` while ignoring everything else under `yuval/outputs/`. Used by the new [[gpt-image-gen-skill]] (called by [[yuval-creative-agent|Yuval]]).
- **Decisions:** OpenAI key is fail-loud — the skill exits before the API call if the key is empty. `yuval/outputs/*` stays out of git because generated images are reproducible artifacts, not source.
- **Notes / Caveats:** User must paste their real OpenAI key into `.env` before Yuval can produce images. Until then the skill exits with `OPENAI_API_KEY missing`.
- **Related:** [[gpt-image-gen-skill]], [[yuval-creative-agent]]
