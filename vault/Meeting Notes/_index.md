# Meeting Notes — Index

Architecture, config, and decision logs for the-five-aegents project. Each topic file documents one component of the project: what it is, what files it contains, and how it relates to the rest of the system.

## Topics

- [[project-file-inventory]] — top-level map of every file and folder in the project
- [[claude-md]] — `CLAUDE.md`, the entry-point instructions Claude Code reads at session start
- [[env-config]] — `.env`, `.env.example`, `.gitignore` — environment variables and ignore rules
- [[claude-settings]] — `.claude/settings.local.json` — local permission allowlist for Bash commands
- [[claude-agents]] — `.claude/agents/` — project-specific agent definitions (currently: Reuven CEO)
- [[reuven-ceo-agent]] — Reuven, the CEO orchestrator agent (`.claude/agents/reuven.md`)
- [[yuval-creative-agent]] — Yuval, the creative/image-generation sub-agent (`.claude/agents/yuval.md` + `yuval/`)
- [[yael-content-writer]] — Yael, the content-writer sub-agent (`.claude/agents/yael.md` + `yael/`)
- [[chen-web-researcher]] — Chen, the web-researcher sub-agent (`.claude/agents/chen.md` + `chen/`)
- [[guy-qa-agent]] — Guy, the QA sub-agent that closes the content pipeline loop (`.claude/agents/guy.md` + `guy/`)
- [[gpt-image-gen-skill]] — Project-internal skill wrapping the OpenAI Images API
- [[claude-commands]] — `.claude/commands/` — placeholder for custom slash-commands
- [[claude-skills]] — `.claude/skills/` — overview of all installed skills
- [[superpowers-skills]] — 14 skills installed from obra/superpowers
- [[obsidian-skills]] — `obsidian-bases`, `obsidian-markdown`, `obsidian-vault-workflow`
- [[skill-creator-install]] — `skill-creator` from anthropics/skills, project-scope install
- [[obsidian-config]] — `.obsidian/` — Obsidian app config files for the vault
