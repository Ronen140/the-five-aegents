# Env & Ignore Config

## Overview

Three files at the project root govern environment variables and what Git tracks:

- **`.env`** — local secrets (e.g. `ANTHROPIC_API_KEY`, `DEFAULT_MODEL`). Git-ignored. Filled in by the developer.
- **`.env.example`** — committed template that documents which env vars exist. Anyone cloning the repo copies this to `.env` and fills in real values.
- **`.gitignore`** — currently ignores only `.env`. Will grow as build artifacts, caches, and dependency directories appear.

`DEFAULT_MODEL` is set to `claude-sonnet-4-6` in both `.env` and `.env.example`.

## Open Questions

- Add Python venv / `node_modules` patterns to `.gitignore` once a runtime is chosen
- Decide whether to add other API keys (e.g. social-platform tokens for publishing) to `.env.example`

## Session Log

### 2026-05-06 — initial env scaffolding [shipped]
- **What was done:** Created `.env`, `.env.example`, and `.gitignore`. Committed `.env.example` and `.gitignore`; `.env` excluded from Git.
- **Decisions:** Default model pinned to `claude-sonnet-4-6` for consistency across agents. `.env.example` keeps comments above each variable so future contributors know what each one is for.
- **Notes / Caveats:** If a teammate ever commits a real secret by accident, the value must be rotated — Git history retention makes the leak permanent regardless of follow-up commits.
- **Related:** [[project-file-inventory]], [[claude-md]]
