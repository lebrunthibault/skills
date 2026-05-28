# My Skills

My personal collection of agent skills (slash commands and behaviors) for Claude Code.

Inspired by and adapted from [Matt Pocock's skills](https://github.com/mattpocock/skills).

## Quickstart (30-second setup)

1. Run the skills.sh installer:

```bash
npx skills@latest add lebrunthibault/skills
```

2. Pick the skills you want, and which coding agents you want to install them on. **Make sure you select `/setup-lebrunthibault-skills`**.

3. Run `/setup-lebrunthibault-skills` in your agent. It records a small `## Agent skills` block in your repo's `CLAUDE.md` (or `AGENTS.md`) so the skills know this repo's conventions.

4. You're ready to go.

## Reference

- **[writing-skills](./skills/writing-skills/SKILL.md)** — Create new agent skills with proper structure, progressive disclosure, and bundled resources.
- **[setup-lebrunthibault-skills](./skills/setup-lebrunthibault-skills/SKILL.md)** — Run once per repo to record an `## Agent skills` block so the skills know this repo's conventions.
- **[research-wiki](./skills/research-wiki/SKILL.md)** — Read, search, and contribute to your personal Obsidian research wiki (LLM-maintained, Karpathy pattern) from any repo.
