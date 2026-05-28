---
name: setup-lebrunthibault-skills
description: Run once per repo after installing lebrunthibault's skills. Records an `## Agent skills` block in CLAUDE.md (or AGENTS.md) so the skills know this repo's basic conventions. Run before first use of the skills, or if they appear to be missing repo context.
disable-model-invocation: true
---

# Setup lebrunthibault's Skills

Scaffold the minimal per-repo configuration that the skills assume. This is a
prompt-driven skill, not a deterministic script: explore, present what you
found, confirm with the user, then write.

Right now the only shipped skill (`writing-skills`) needs no per-repo config, so
this setup is intentionally light. As skills that require repo-level
configuration are added, extend this skill to capture that config here.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read what exists;
don't assume:

- `AGENTS.md` and `CLAUDE.md` at the repo root — does either exist? Is there
  already an `## Agent skills` section in either?
- `git remote -v` — is this a GitHub repo? Which one?

### 2. Confirm

Summarise what's present and what's missing. Confirm with the user which file to
record the `## Agent skills` block in before writing.

### 3. Write

**Pick the file to edit:**

- If `CLAUDE.md` exists, edit it.
- Else if `AGENTS.md` exists, edit it.
- If neither exists, ask the user which one to create — don't pick for them.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa) —
always edit the one that's already there.

If an `## Agent skills` block already exists in the chosen file, update its
contents in-place rather than appending a duplicate. Don't overwrite user edits
to the surrounding sections.

The block:

```markdown
## Agent skills

This repo uses [lebrunthibault's skills](https://github.com/lebrunthibault/skills).

- `writing-skills` — create new agent skills with proper structure and progressive disclosure.
```

### 4. Done

Tell the user the setup is complete and which skills are now installed. Mention
they can re-run this skill after installing more skills that need per-repo
config.
