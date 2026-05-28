# My Skills

My personal collection of agent skills (slash commands and behaviors) for Claude Code.

Inspired by and adapted from [Matt Pocock's skills](https://github.com/mattpocock/skills).

## Install

These skills are used **only with Claude Code**, so this repo is the single source of truth and
each skill is linked directly into `~/.claude/skills/` — no separate installer, no `~/.agents`
copies. Clone the repo, then junction (Windows) or symlink (macOS/Linux) each skill folder into
`~/.claude/skills/`. Editing the repo updates the live skill instantly (the link points back here).

```bash
git clone git@github.com:lebrunthibault/skills.git
```

**Windows (PowerShell)** — directory junctions don't need admin rights:

```powershell
$base = "$env:USERPROFILE\.claude\skills"
$repo = "C:\path\to\skills\skills"   # the inner skills/ folder of the clone
Get-ChildItem $repo -Directory | ForEach-Object {
  New-Item -ItemType Junction -Path (Join-Path $base $_.Name) -Target $_.FullName -Force
}
```

**macOS / Linux**:

```bash
for d in /path/to/skills/skills/*/; do
  ln -s "$d" "$HOME/.claude/skills/$(basename "$d")"
done
```

Then run `/setup-lebrunthibault-skills` once in a repo: it records a small `## Agent skills` block
in that repo's `CLAUDE.md` (or `AGENTS.md`) so the skills know the repo's conventions.

> Note: this repo can also be installed via `npx skills@latest add lebrunthibault/skills`, but that
> tool copies skills into a shared `~/.agents/` and symlinks them — useful for multi-agent setups
> (Cursor, Zed, Codex…), not needed for a Claude-Code-only workflow.

## Reference

- **[writing-skills](./skills/writing-skills/SKILL.md)** — Create new agent skills with proper structure, progressive disclosure, and bundled resources.
- **[setup-lebrunthibault-skills](./skills/setup-lebrunthibault-skills/SKILL.md)** — Run once per repo to record an `## Agent skills` block so the skills know this repo's conventions.
- **[research-wiki](./skills/research-wiki/SKILL.md)** — Read, search, and contribute to your personal Obsidian research wiki (LLM-maintained, Karpathy pattern) from any repo.
- **[commit](./skills/commit/SKILL.md)** — Bump the project version, commit, and push in a clean OSS-friendly way, auto-detecting the version file across languages.
