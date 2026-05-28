---
name: writing-skills
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

## Process

1. **Gather requirements** - ask user about:
   - What task/domain does the skill cover?
   - What specific use cases should it handle?
   - Does it need executable scripts or just instructions?
   - Any reference materials to include?

2. **Draft the skill** - create:
   - SKILL.md with concise instructions
   - Additional reference files if SKILL.md grows large (see When to Split Files)
   - Utility scripts if deterministic operations needed

3. **Review with user** - present draft and ask:
   - Does this cover your use cases?
   - Anything missing or unclear?
   - Should any section be more/less detailed?

4. **Install & register** - make the skill live and versioned:
   - Create the skill folder under this repo's `skills/<name>/` (the repo is the source of truth).
   - Add it to the repo's `README.md` reference list and `.claude-plugin/plugin.json`
     (required by the repo's CLAUDE.md).
   - Link it into `~/.claude/skills/<name>` so Claude Code sees it — Claude Code only scans
     `~/.claude/skills/`. Use a **junction** on Windows (`New-Item -ItemType Junction`, no admin
     needed) or a **symlink** on macOS/Linux (`ln -s`). The link points back at the repo, so future
     edits to the repo update the live skill instantly.
   - If `~/.claude/skills/` already holds skills, mirror whatever convention is already there.

## Skill Structure

```
skill-name/
├── SKILL.md           # Main instructions (required)
├── references/        # Detailed docs loaded on demand (if needed)
├── scripts/           # Utility scripts (if needed)
│   └── helper.js
└── assets/            # Templates, fonts, icons used in output (if needed)
```

### Naming rules (these cause most upload failures)

- **`SKILL.md`**: named exactly that, **case-sensitive**. No variants (`SKILL.MD`, `skill.md`).
  Wrong name → "Could not find SKILL.md in uploaded folder".
- **Skill folder**: **kebab-case** (`pdf-form-filler`). No spaces, no underscores, no capitals.
- **`name` field must match the folder name.**
- **No `README.md` inside the skill folder** — all docs go in SKILL.md or `references/`.
  (A repo-level README for human readers is fine when distributing on GitHub — that's separate
  from the skill folder.)

### Frontmatter security restrictions

- **No XML angle brackets (`< >`)** anywhere in the frontmatter — it's injected into the system
  prompt, so this is an injection-safety restriction.
- **Names cannot start with "claude" or "anthropic"** (reserved).

### Optional frontmatter fields

Beyond the required `name` and `description`:

```yaml
license: MIT                              # if open source
compatibility: requires Python 3.11+      # 1-500 chars: environment needs
allowed-tools: "Bash(python:*) WebFetch"  # restrict tool access
metadata:                                 # arbitrary custom key-values
  author: Your Name
  version: 1.0.0
```

## SKILL.md Template

```md
---
name: skill-name
description: Brief description of capability. Use when [specific triggers].
---

# Skill Name

## Quick start

[Minimal working example]

## Workflows

[Step-by-step processes with checklists for complex tasks]

## Advanced features

[Link to separate files: See [references/advanced.md](references/advanced.md)]
```

## Description Requirements

The description is **the only thing your agent sees** when deciding which skill to load. It's surfaced in the system prompt alongside all other installed skills. Your agent reads these descriptions and picks the relevant skill based on the user's request.

**Goal**: Give your agent just enough info to know:

1. What capability this skill provides
2. When/why to trigger it (specific keywords, contexts, file types)

**Format**:

- Max 1024 chars
- Write in third person
- First sentence: what it does
- Second sentence: "Use when [specific triggers]"

**Good example**:

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**Bad example**:

```
Helps with documents.
```

The bad example gives your agent no way to distinguish this from other document skills.

## When to Add Scripts

Add utility scripts when:

- Operation is deterministic (validation, formatting)
- Same code would be generated repeatedly
- Errors need explicit handling

Scripts save tokens and improve reliability vs generated code.

## When to Split Files

Keep SKILL.md focused on core instructions and move detail into `references/`. This is progressive
disclosure: the frontmatter is always loaded, the SKILL.md body loads when relevant, and linked
files load only when needed.

Split into separate files when:

- SKILL.md gets long — keep it well under ~5,000 words (oversized skills slow responses and defeat
  progressive disclosure)
- Content has distinct domains (finance vs sales schemas)
- Advanced features are rarely needed

Keep references **one level deep** (SKILL.md links to a file; that file doesn't chain to more).

## Test Triggering

The description controls whether the skill loads. After drafting, test it:

- **Triggers on obvious requests** and on **paraphrased** ones.
- **Does NOT trigger** on unrelated topics.
- Quick check: ask the agent *"When would you use the [skill-name] skill?"* — it will quote the
  description back. Adjust based on what's missing.

Tune from real use:

- **Under-triggering** (skill doesn't load when it should) → add detail and keywords to the
  description.
- **Over-triggering** (loads for irrelevant queries) → be more specific, add negative triggers
  (e.g. `Do NOT use for simple data exploration`).

For higher-stakes skills, also verify functional correctness (valid outputs, error handling, edge
cases) and, where it matters, compare results with vs. without the skill.

## Review Checklist

After drafting, verify:

- [ ] Folder is kebab-case; file is exactly `SKILL.md`; `name` matches the folder
- [ ] No `README.md` inside the skill folder
- [ ] No XML angle brackets (`< >`) in frontmatter; name not prefixed "claude"/"anthropic"
- [ ] Description includes both what it does and triggers ("Use when..."), under 1024 chars
- [ ] SKILL.md stays focused (well under ~5,000 words); detail moved to `references/`
- [ ] Triggering tested (fires on relevant + paraphrased, not on unrelated)
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete examples included
- [ ] References one level deep
