---
name: research-wiki
description: Read, search, and contribute to the user's personal Obsidian research wiki (an LLM-maintained "wiki" in the Karpathy pattern). Use when the user asks to look something up in their wiki/vault/notes, reference past research, ingest a new source (article/PDF) into the wiki, or ask a question that prior research likely covers — from ANY repo. Triggers: "mon wiki", "le vault", "ingère cette source", "qu'est-ce que j'ai sur X", "ajoute au wiki".
---

# Research Wiki

The user keeps a persistent, compounding research wiki maintained by an LLM
(Karpathy's "LLM wiki" pattern). It lives **outside any code repo** and is meant to be
referenced from anywhere.

**Vault root:** `D:\dev\obsidian\dev`

This skill is a thin pointer. The vault is **self-describing**: its own
`CLAUDE.md` is the authoritative schema (architecture, page conventions, ingest/query/lint
workflows, frontmatter, naming). This skill never duplicates those rules — it routes you there.

## Step 0 — Always read the schema first

Before any operation, read the vault's schema and entry points:

1. `D:\dev\obsidian\dev\CLAUDE.md` — the schema. Conventions and workflows. **Authoritative.**
2. `D:\dev\obsidian\dev\index.md` — content catalog. Start every lookup here, then drill in.
3. `D:\dev\obsidian\dev\log.md` (optional) — chronological history of ingests/queries/lints.

If the schema and this file ever disagree, **the vault's `CLAUDE.md` wins.** Follow it exactly.

## Workflows

All workflows are defined in the vault's `CLAUDE.md` (sections "Opérations"). Summary of when to do what:

- **Query / look something up** → read `index.md`, drill into the relevant `wiki/` pages,
  answer **with `[[wikilink]]` citations**. If the answer is a new synthesis worth keeping,
  ask before filing it as a page (per the schema's Query workflow).
- **Ingest a new source** → follow the schema's Ingest workflow: source goes in `raw/`
  (immutable, never edited), write a `wiki/sources/` summary, update touched
  entity/concept/synthesis pages, update `index.md`, append to `log.md`.
- **Lint** → on request, run the schema's Lint checklist (contradictions, orphans, stale claims).

## Hard rules (from the vault schema — do not violate)

- **Never modify `raw/`.** It is the immutable source of truth.
- **Never overwrite a contradiction** — note both versions in a `## Points en tension` section, dated.
- **Cite sources** in the body with `[[...]]`. Link generously (dead wikilinks are fine — they mark TODO pages).
- **One idea = one page.** Filenames kebab-case, no accents; readable name goes in frontmatter `title:`.
- After any write: update `index.md` and append a `log.md` entry. The agent does the bookkeeping.

## Notes when working from another repo

- The vault path is absolute — use it directly; no need to `cd` into it or open it as a workspace.
- Wikilinks (`[[page]]`) resolve by **filename** within the vault, not by repo-relative path.
- The vault is read in **Obsidian** by the user; keep YAML frontmatter and `[[wikilinks]]` valid.
