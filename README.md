# My Skills

My personal collection of agent skills (slash commands and behaviors) for Claude Code.

## Install

These skills are used **only with Claude Code**, so this repo is the single source of truth: each
skill folder under `skills/` is linked into `~/.claude/skills/` rather than copied. Editing a skill
here updates the live skill instantly (the link points back to this repo).

To set up a fresh machine: clone this repo, then ask Claude to link every skill folder under
`skills/` into your global `~/.claude/skills/`. Claude will create the right junction (Windows) or
symlink (macOS/Linux) for each one.

## Skills

- **[writing-skills](./skills/writing-skills/SKILL.md)** — Create new agent skills with proper structure, progressive disclosure, and bundled resources.
- **[research-wiki](./skills/research-wiki/SKILL.md)** — Read, search, and contribute to your personal Obsidian research wiki (LLM-maintained, Karpathy pattern) from any repo.
- **[commit](./skills/commit/SKILL.md)** — Bump the project version, commit, and push in a clean OSS-friendly way, auto-detecting the version file across languages.
