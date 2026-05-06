# Claude Settings (Local)

## Overview

`.claude/settings.local.json` is the per-machine permission allowlist for Claude Code in this project. It is **not** committed to Git (settings.local.json is gitignored by Claude Code's default conventions, though it is currently tracked here — see Open Questions).

Current allowlist permits Bash patterns:

- `git add *` — staging files
- `git commit -m ' *` — commits with messages
- `git push *` — pushing to remotes
- `git clone *` — cloning external repos (used for plugin install)

These let Claude run common Git operations without prompting on every invocation.

## Open Questions

- Should `.claude/settings.local.json` be added to `.gitignore`? Conventionally `*.local.json` is per-user and not committed.
- Will need to add patterns for `npm`, `pip`, `python`, etc. once a runtime is chosen.

## Session Log

### 2026-05-06 — documented existing local settings [shipped]
- **What was done:** Documented the four Bash permission patterns currently allowed. No changes made to the file itself.
- **Decisions:** Left settings as-is for now; revisit gitignore policy when the team is more than one person.
- **Notes / Caveats:** Permission grants are pattern-based. `git commit -m ' *` matches any commit message starting with a single quote — that's how multi-line HEREDOC commit messages get through.
- **Related:** [[project-file-inventory]], [[env-config]], [[claude-md]]
