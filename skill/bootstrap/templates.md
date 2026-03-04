# File Generation Templates

> **Part of**: [SKILL.md](../SKILL.md) · **Lines**: ~391 · **Last updated**: 2026-03-04

Templates use `{{placeholder}}` syntax. Agents substitute placeholders and write files.

## Universal Formatting Rules

- **Header**: `> **Part of**: [<scope>/](<path>) · **Lines**: ~XXX · **Last updated**: YYYY-MM-DD`
- Root meta-files link to `./_index.md`; sub-concept READMEs to `../_index.md`; other sub-files to `./README.md`
- Max 400 lines. ASCII-box tree for schemas (`├──` / `└──`). All links relative
- API routes use `{param}` (NOT `:param`). Entity names **bold** in tables
- `---` between major sections. Every file ends with Related Files (Topic | File)

---

## 1. `_index.md`

~~~markdown
# {{project_name}} — Concepts Index

> **AI Agent Instructions**: Read this file first. Use the routing tables below to find the specific file for your task. Each sub-file is <400 lines.

## Quick Navigation by Task

| If you need to... | Read this file |
|-------------------|----------------|
{{task_nav_rows}}

## Navigation by Entity

| Entity | Definition | Behavior/Usage |
|--------|------------|----------------|
{{entity_nav_rows}}

## Cross-Cutting Concerns

| Topic | Primary File | Also See |
|-------|--------------|----------|
{{cross_cutting_rows}}

## Concept Overviews

| Concept | README | Description |
|---------|--------|-------------|
{{concept_overview_rows}}

## Meta Documentation

| Purpose | File |
|---------|------|
| Entity ownership & conflict resolution | [_ownership.md](_ownership.md) |
| Creating & editing concepts | [_contributing.md](_contributing.md) |
| Validation & compliance checks | [_validation.md](_validation.md) |

## File Structure

```
docs/concepts/
├── _index.md
├── _ownership.md
├── _contributing.md
├── _validation.md
{{file_tree}}
```

## Design Rules

1. Each file stays under 400 lines
2. Entity schemas defined in exactly ONE `data-models.md`
3. All links are relative — never absolute
4. API routes use `{param}` syntax (NOT `:param`)
5. Every file has the standard header (Part-of, Lines, Last-updated)
6. Every file ends with a Related Files table
7. Ownership defined in `_ownership.md` is canonical

## Version History

> Keep only the last 2 entries.

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | {{date}} | @claude | Initial concept documentation |
~~~

---

## 2. `_contributing.md`

~~~markdown
# Contributing to Concept Documentation

> **Part of**: [concepts/](./_index.md) · **Lines**: ~250 · **Last updated**: {{date}}

## 8-Step Creation Checklist

1. **Create folder** — `docs/concepts/<name>/` with `README.md` + `data-models.md`
2. **Write README** — Summary, owned entities, referenced-by list
3. **Write data-models** — Entity schemas, field definitions, relationships
4. **Declare ownership** — Add entities to `_ownership.md`
5. **Update `_index.md`** — Add rows to all navigation tables
6. **Add cross-references** — Link to/from related concept files
7. **Run self-audit** — Execute the 7-category audit below
8. **Fix issues** — Resolve anything the audit flags before marking complete

## Rules

| Rule | Requirement |
|------|-------------|
| File size | Each file MUST be <400 lines |
| Single source | Entity schemas only in `data-models.md`, link elsewhere |
| No duplication | Link to authoritative source, never copy content |
| Header format | `> **Part of**: ... · **Lines**: ~XXX · **Last updated**: YYYY-MM-DD` |
| Ownership | Every README must declare "Owned Entities" and "Referenced By" |

---

## 7-Category Self-Audit

| # | Category | What to Check |
|---|----------|---------------|
| 6.1 | Structure | Files <400 lines; README.md exists; data-models.md if entities; api.md if endpoints; headers present |
| 6.2 | Content | Entity defined in ONE data-models.md only; all links resolve; Related Files populated |
| 6.3 | Index | _index.md tables updated: task nav, entity nav, concept overviews, file tree |
| 6.4 | Cross-Refs | Outbound links to related concepts; inbound links exist; reachable from _index.md |
| 6.5 | Consistency | Entity names identical everywhere; field names match; no contradictions |
| 6.6 | Global Index | _index.md <400 lines; all folders in tree; all entities in nav; overviews complete |
| 6.7 | Ownership | README has "Owned Entities" + "Referenced By"; matches _ownership.md exactly |

**On failure:** Fix immediately → re-run check → continue audit → report fixes.

---

## Sub-Templates

Use Templates 5, 6, 7 (below) for the full parameterized versions. The following are minimal skeletons for reference in _contributing.md itself.

### README.md skeleton

Sections required: Summary, Owned Entities, Referenced By, Key Concepts (table),
When to Read Which File (table), Related Files (table), Code References (table).
Header links to `../_index.md`.

### data-models.md skeleton

Sections required: one `## EntityName` per entity with ASCII-box schema + Constraints,
Entity Relationships diagram, Code References (table), Related Files (table).
Header links to `./README.md`.

### api.md skeleton

Sections required: Access Control (table), Endpoints (one subsection per endpoint
with method + path in fenced block), Code References (table), Related Files (table).
Header links to `./README.md`.

---

## Related Files

| Topic | File |
|-------|------|
| Navigation hub | [_index.md](./_index.md) |
| Ownership | [_ownership.md](./_ownership.md) |
| Validation | [_validation.md](./_validation.md) |
~~~

---

## 3. `_ownership.md`

~~~markdown
# Entity Ownership Registry

> Canonical source for each entity. When files disagree, the owner wins.
> **Last updated**: {{date}}

## Ownership Table

| Entity | Canonical Owner | Referenced By |
|--------|-----------------|---------------|
{{entity_rows}}

## Ownership Rules

1. **Canonical owner defines**: entity name, fields, types, relationships, constraints
2. **Referencing files must**: use exact entity names, link to owner, not redefine behavior
3. **New entities**: add to this table, declare in owner README, update referencing concepts

## Conflict Resolution

1. Identify the canonical owner from the table above
2. Owner's definition wins — no exceptions
3. Update the referencing file to match the owner
4. If the owner is wrong, fix the owner first, then propagate

## Cross-Concept Entities

| Scenario | Resolution |
|----------|------------|
{{cross_concept_rows}}

## Adding New Concepts

1. Identify all entities the new concept will own
2. Add entries to the Ownership Table
3. Identify which existing concepts it references
4. Update those concepts' "Referenced By" columns
5. Declare ownership in the new README.md

## Related Files

| Topic | File |
|-------|------|
| Navigation hub | [_index.md](./_index.md) |
| Contributing | [_contributing.md](./_contributing.md) |
| Validation | [_validation.md](./_validation.md) |
~~~

---

## 4. `_validation.md`

~~~markdown
# Design Validation & Compliance

> **Part of**: [concepts/](./_index.md) · **Lines**: ~170 · **Last updated**: {{date}}

## Compliance Check

| Step | Check | Priority |
|------|-------|----------|
| 1 | Ownership registry matches README declarations | 🟢 Minor |
| 2 | Terminology consistent across all files | 🟡 Moderate |
| 3 | Cross-references and links valid | 🟢 Minor |
| 4 | Rules don't contradict across files | 🔴 Critical |
| 5 | Data models aligned with canonical owners | 🟡 Moderate |

### Priority Levels

| Priority | Criteria | Examples |
|----------|----------|----------|
| 🔴 **Critical** | Contradictory rules; wrong implementation | Two files describe opposite behavior |
| 🟡 **Moderate** | Inconsistent terminology, outdated names | Entity renamed but not everywhere |
| 🟢 **Minor** | Missing links, declarations, formatting | Broken cross-reference |

## Design Validation Checklist

| # | Area | Key Question | Concept File |
|---|------|-------------|--------------|
{{validation_checklist_rows}}

### Validation Workflow

1. **STOP** — Do not implement the violating change
2. **DOCUMENT** — Explain which rule is violated and why
3. **ASK** — Request explicit permission to proceed
4. **Only proceed** if the user explicitly confirms the override

## Full Codebase Audit

Trigger: "Conduct a full platform concepts audit"

1. Read all concept files via `_index.md`
2. For each checklist item, search codebase for implementations
3. Verify they follow documented rules
4. Report: `| Area | File:Line | Issue | Concept Violated |`

### Key Search Patterns

{{search_patterns}}

## Related Files

| Topic | File |
|-------|------|
| Navigation hub | [_index.md](./_index.md) |
| Entity ownership | [_ownership.md](./_ownership.md) |
| Creating concepts | [_contributing.md](./_contributing.md) |
~~~

---

## 5. Concept `README.md`

~~~markdown
# {{concept_name}}

> **Part of**: [concepts/](../_index.md) · **Lines**: ~XX · **Last updated**: {{date}}

## Summary

{{summary}}

## Owned Entities

{{owned_entities}}

## Referenced By

{{referenced_by}}

## Key Concepts

| Concept | Description |
|---------|-------------|
{{key_concepts_rows}}

## When to Read Which File

| If you need to... | Read |
|-------------------|------|
{{file_routing_rows}}

## Related Files

| Topic | File |
|-------|------|
{{related_files_rows}}

## Code References

| Component | Location |
|-----------|----------|
{{code_ref_rows}}
~~~

---

## 6. Concept `data-models.md`

~~~markdown
# {{concept_name}} Data Models

> **Part of**: [{{concept_name_lower}}/](./README.md) · **Lines**: ~XX · **Last updated**: {{date}}

{{entity_sections}}

## Entity Relationships

```
{{relationship_diagram}}
```

## Code References

| Component | Location |
|-----------|----------|
{{code_ref_rows}}

## Related Files

| Topic | File |
|-------|------|
{{related_files_rows}}
~~~

---

## 7. Concept `api.md`

~~~markdown
# {{concept_name}} API

> **Part of**: [{{concept_name_lower}}/](./README.md) · **Lines**: ~XX · **Last updated**: {{date}}

## Access Control

| Operation | Required Role | Scope |
|-----------|---------------|-------|
{{access_control_rows}}

---

## Endpoints

{{endpoint_sections}}

---

## Code References

| Component | Location |
|-----------|----------|
{{code_ref_rows}}

## Related Files

| Topic | File |
|-------|------|
{{related_files_rows}}
~~~
