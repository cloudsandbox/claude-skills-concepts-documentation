# Quick Scaffold — Bootstrap Mode

> **When**: `docs/concepts/_index.md` does not exist and user chose "quick"
> **Goal**: Generate 4 meta-files immediately — no questions asked

## Step 1 — Resolve Project Name

1. Read `package.json` → `name` in the working directory.
2. No `package.json`? Use the current directory's basename.
3. Ambiguous name (e.g., `app`, `src`)? Ask: "What should I call this project?"

Store as `{{PROJECT}}`. Use `{{DATE}}` for today (`YYYY-MM-DD`).

## Step 2 — Create Directory

Run `mkdir -p docs/concepts`. Do **not** create sub-folders — concept folders
are added later via `maintain/create-concept.md`.

## Step 3 — Generate Meta-Files

Create all four files in one pass. Use templates exactly as shown.

### 3a. `docs/concepts/_index.md`

```markdown
# {{PROJECT}} — Concepts Index

> **AI Agent Instructions**: Read this file first. Use the routing tables
> below to find the specific file for your task. Each sub-file is <400 lines.

## Quick Navigation by Task

| If you need to... | Read this file |
|-------------------|----------------|
<!-- Add rows as concepts are created -->

## Navigation by Entity

| Entity | Definition | Behavior/Usage |
|--------|------------|----------------|
<!-- Add rows as entities are documented -->

## Concept Folders

| Folder | Summary | Owned Entities |
|--------|---------|----------------|
<!-- Add rows as concept folders are created -->
```

### 3b. `docs/concepts/_contributing.md`

```markdown
# Contributing to Concept Documentation

> **Part of**: [concepts/](./_index.md) · **Last updated**: {{DATE}}

## 8-Step Creation Checklist

1. **Create folder** — `docs/concepts/<name>/` with `README.md` + `data-models.md`
2. **Write README** — Summary, owned entities, referenced-by list
3. **Write data-models** — Entity tables, field definitions, relationships
4. **Declare ownership** — Add entities to `_ownership.md`
5. **Update `_index.md`** — Add rows to all three navigation tables
6. **Add cross-references** — Link to/from related concept files
7. **Run self-audit** — Execute the 7-category audit below
8. **Fix issues** — Resolve anything the audit flags before marking complete

## 7-Category Self-Audit

Run after every concept file creation or edit.

| # | Category | What to Check |
|---|----------|---------------|
| 6.1 | Structure | Required files exist, each <400 lines, headers present |
| 6.2 | Content | No duplicated content, all links resolve |
| 6.3 | Index | `_index.md` tables updated for new/changed concept |
| 6.4 | Cross-Refs | Outbound links work, inbound links exist |
| 6.5 | Consistency | Entity names match across files, no contradictions |
| 6.6 | Global Index | All folders in `_index.md`, all entities present |
| 6.7 | Ownership | README "Owned Entities" matches `_ownership.md` |
```

### 3c. `docs/concepts/_ownership.md`

```markdown
# Entity Ownership Registry

> Canonical source for each entity. When files disagree, the owner wins.
> **Last updated**: {{DATE}}

| Entity | Canonical Owner | Referenced By |
|--------|-----------------|---------------|
<!-- Add rows as concepts declare owned entities -->
```

### 3d. `docs/concepts/_validation.md`

```markdown
# Design Validation & Compliance

> **Part of**: [concepts/](./_index.md) · **Last updated**: {{DATE}}

## Design Validation Checklist

| Area | Key Question | Concept File |
|------|-------------|--------------|
<!-- Add rows as domain rules are documented -->

## Compliance Check

| Step | Check | Priority |
|------|-------|----------|
| 1 | Ownership registry matches README declarations | Minor |
| 2 | Terminology consistent across all files | Moderate |
| 3 | Cross-references and links valid | Minor |
| 4 | Rules don't contradict across files | Critical |
| 5 | Data models aligned with canonical owners | Moderate |
```

## Step 4 — Generate CLAUDE.md Section

Read `skill/claude-md-generator.md` and follow its instructions to append the
concept-docs section to the project's `CLAUDE.md`.

If `claude-md-generator.md` does not exist yet, tell the user:
"CLAUDE.md generator not available yet. Add the section manually later."

## Step 5 — Confirm

Print:

```
Scaffolded docs/concepts/ with 4 meta-files:
  _index.md          — navigation hub (empty tables)
  _contributing.md   — 8-step checklist + 7-category audit
  _ownership.md      — empty entity registry
  _validation.md     — empty design validation checklist

No concept folders created. Add your first: /concept-docs add concept <name>
```

Done. Do not create concept folders or scan the codebase in this mode.
