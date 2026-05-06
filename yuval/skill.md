# gpt-image-gen Skill (pointer)

> **This is a human-facing pointer file. Do not edit skill behavior here.**
>
> Canonical skill definition: [`.claude/skills/gpt-image-gen/SKILL.md`](../.claude/skills/gpt-image-gen/SKILL.md).
> That is the file Claude Code loads as a skill.

## What the skill does

Thin wrapper around `POST https://api.openai.com/v1/images/generations`. It does NOT author prompts — it accepts a finalized prompt + output path and saves the resulting PNG to disk. Reads `OPENAI_API_KEY` from `.env`.

## Who calls it

Currently: [Yuval](agent.md). Future agents that need to generate images should also call it (instead of hitting the API directly) — the skill centralizes auth, error handling, and the `jq`-vs-Python fallback.

## Environment notes

The project's dev environment is Git Bash on Windows. `jq` is not installed, so the skill's **Path B (Python fallback)** is the default. `python` and `base64` are available out of the box.
