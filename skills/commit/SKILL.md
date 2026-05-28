---
name: commit
description: Bump the project version, commit, and push in a clean OSS-friendly way, auto-detecting the version file across languages. Use when the user asks to "commit", "bump version", "release", "ship", "push a version", or finishes a feature and wants it committed. Do NOT use for plain `git commit` of work-in-progress with no intent to version or push.
allowed-tools: "Bash(git:*) Bash(python:*) Read Edit Write"
metadata:
  author: lebrunthibault
  version: 1.0.0
---

# Commit

Automates the **bump → commit → push** flow, language-agnostic. The version is bumped **only when
shipped changes warrant it** (new or changed functionality), so docs/chore/style commits don't
inflate the version.

## Quick start

```bash
# 1. See what's staged/unstaged and detect the version source
git status --short
python scripts/detect_version.py detect
```

Then follow the Workflow below: classify the changes → decide bump → write version → commit → push.

## Workflow

### Step 1: Inspect the changes

Run, and read the output:

```bash
git status --short
git diff --staged
git diff            # unstaged, if anything isn't staged yet
```

Stage what belongs in this commit (prefer naming files explicitly over `git add -A`, to avoid
sweeping in secrets or stray files).

### Step 2: Classify — does this change warrant a version bump?

**Bump the version when the change affects what the software does or exposes:**

- new feature, new public API/CLI flag/endpoint
- behavior change or removal (breaking)
- bug fix that changes observable behavior

**Do NOT bump for** (commit normally, skip Step 3):

- docs, comments, README
- tests-only changes
- formatting / lint / style
- internal refactor with no behavior change
- CI/build config, dependency bumps with no user-facing effect

If it's mixed (e.g. a feature + docs), the feature wins → bump.

### Step 3: Decide and apply the bump (only if Step 2 said yes)

Detect the current version and propose a level:

```bash
python scripts/detect_version.py detect
```

Choose the level from the change classification:

- **major** — breaking change / removal
- **minor** — new feature, backward-compatible
- **patch** — bug fix, no API change

**Tell the user the proposed bump (old → new) and which file, and get a quick confirmation**
before writing. Then apply:

```bash
python scripts/detect_version.py bump <major|minor|patch>
```

The script edits the detected file in place and prints `{"file", "old", "new"}`.

**If detection returns `{"file": null}`** (no version source found): see
[references/version-files.md](references/version-files.md). The clean default for any project is
to create a top-level `VERSION` file (plain `0.1.0`). Propose this to the user; create it only on
confirmation, then `bump` works against it.

### Step 4: Commit

Write a clean, conventional commit message. Use [Conventional Commits](https://www.conventionalcommits.org)
style so the history stays OSS-readable:

- `feat: ...` (minor), `fix: ...` (patch), `feat!:`/`BREAKING CHANGE:` (major)
- `docs: ...`, `test: ...`, `refactor: ...`, `chore: ...` for non-bump commits

If a version was bumped, the version-file change is part of **this same commit**. Subject in
imperative mood, ≤ ~70 chars; body explains the *why*.

```bash
git add <version-file-if-bumped> <other-files>
git commit -m "feat: add X" -m "Why this change…"
```

Do **not** pass `--no-verify` — let hooks run. If a hook fails, fix the cause and make a new commit.

### Step 5: Tag (only if a version was bumped)

A bumped version is a release marker — tag it so OSS consumers can pin:

```bash
git tag -a "v<new-version>" -m "v<new-version>"
```

### Step 6: Push

Run the safety checks first, then push:

```bash
git rev-parse --abbrev-ref HEAD          # confirm the branch you're on
git push                                 # push the commit
git push --tags                          # if you created a tag in Step 5
```

**Push safety (non-negotiable):**

- **Never** `git push --force` (or `--force-with-lease`) unless the user explicitly asks.
- If pushing directly to `main`/`master`, mention it so the user can object — they may prefer a PR.
- Never push secrets: if `git status` showed `.env`, credentials, keys, etc., stop and confirm.

## Common Issues

- **`detect` prints `{"file": null}`** → no version file. Offer to create a `VERSION` file
  (see [references/version-files.md](references/version-files.md)). Don't guess a location.
- **`bump` returns an `error` about non-semver** → the current version isn't plain `x.y.z`
  (e.g. `2.0.0rc1`). Don't force it; ask the user for the target version and edit the file manually.
- **Multiple version sources disagree** (e.g. `pyproject.toml` says 1.2.0, `__init__.py` says
  1.1.0) → the script bumps the highest-priority one only. Flag the mismatch to the user.
- **Nothing to commit** → report it; do not create an empty commit.

## Notes

- The version detection/bump is delegated to `scripts/detect_version.py` (deterministic, so the
  same logic isn't re-derived each run). Priority order and supported formats live there and in
  [references/version-files.md](references/version-files.md).
- This skill performs visible, hard-to-reverse actions (commit, tag, push). The bump confirmation
  (Step 3) and push checks (Step 6) are the guardrails — keep them.
