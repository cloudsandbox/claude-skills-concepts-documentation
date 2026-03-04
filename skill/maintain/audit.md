# Audit & Compliance — Maintenance Mode

> **When**: `docs/concepts/_index.md` exists and user says "audit" or "check compliance"
> **Also triggered**: Automatically after any concept file edit (self-audit only)

This file contains two modes:

1. **Self-Audit** — 7-category checklist run after every concept file edit
2. **Compliance Check** — Cross-file consistency scan run on demand

---

## Mode 1: Self-Audit

Run this after every creation or edit of files in `docs/concepts/`. The audit
covers the concept that was just modified. If triggered automatically (after a
file edit), run silently and report only failures. If triggered by user request,
report all results.

### How to Run

1. Identify the concept folder that was just edited.
2. Run all 7 categories below in order.
3. For each failure: attempt an automatic fix. If auto-fix is not possible,
   report the issue with a proposed fix and wait for user approval.
4. After all categories pass, confirm: "Self-audit passed for `<concept>/`."

---

### 6.1 Structure

**What to check:**
- Every file in the concept folder is under 400 lines.
- Required files are present:
  - `README.md` — always required.
  - `data-models.md` — required if the concept owns database entities.
  - `api.md` — required if the concept has API controllers.
- Header format is correct on every file:
  `> **Part of**: [<parent>](<path>) . **Lines**: ~XXX . **Last updated**: YYYY-MM-DD`

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| File size | `wc -l` each file in folder | Every file < 400 lines |
| Required files | `ls docs/concepts/<concept>/` | README.md exists; data-models.md if concept lists Owned Entities; api.md if controllers exist |
| Header format | Read lines 1-3 of each `.md` file | Line 2 or 3 matches header pattern with Part of, Lines, Last updated |

**On failure:**
- File over 400 lines: report which file and current line count. Suggest where
  to split (look for `---` section dividers near the midpoint).
- Missing required file: create it using the template from `_contributing.md`.
- Bad header: auto-fix by inserting or correcting the header line.

---

### 6.2 Content

**What to check:**
- No entity schema is duplicated across multiple `data-models.md` files. Each
  entity's field-level definition must appear in exactly one file.
- All markdown links (`[text](path)`) resolve to existing files.
- Every file's "Related Files" table has at least one entry.

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| No duplicate schemas | Search for `## <EntityName>` across all `data-models.md` files | Each entity heading appears in exactly one file |
| Links resolve | Extract all `[text](path)` links, resolve relative paths | Every target file exists on disk |
| Related Files populated | Read "Related Files" table in each file | Table has at least one data row (not just headers) |

**On failure:**
- Duplicate schema: identify which file is the canonical owner (check
  `_ownership.md`). Remove the duplicate from the non-owner file and replace
  with a link: `See [<owner>/data-models.md](../<owner>/data-models.md)`.
- Broken link: check for typos in path. Auto-fix if the target file exists
  under a slightly different name. Otherwise report the broken link.
- Empty Related Files: add at least a link to the parent `_index.md`. Suggest
  related concepts based on entity cross-references.

---

### 6.3 Index

**What to check:**
- `_index.md` has entries for this concept in ALL navigation tables:
  - Quick Navigation by Task
  - Navigation by Entity
  - Cross-Cutting Concerns (if the concept spans multiple areas)
  - Concept Overviews / Concept Folders table
  - File Structure tree diagram

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Task navigation | Search `_index.md` for concept-related task rows | At least one "If you need to..." row links to this concept |
| Entity navigation | Search `_index.md` "Navigation by Entity" table | All owned entities have rows with Definition + Behavior links |
| Concept overview | Search `_index.md` "Concept Folders" or "Concept Overviews" table | Concept README listed in the table |
| File structure | Search `_index.md` file tree | Concept folder appears in the tree |

**On failure:**
- Missing task row: auto-add a row. Derive the task description from the
  concept's README summary.
- Missing entity row: auto-add a row for each owned entity with links to
  `data-models.md` (Definition) and `README.md` (Behavior).
- Missing from overview table: auto-add a row with folder name, summary (from
  README), and owned entities list.
- Missing from file tree: auto-add the folder and its files to the tree.

---

### 6.4 Cross-References

**What to check:**
- Outbound links: the concept's "Related Files" tables link to all concepts it
  depends on or relates to.
- Inbound links: related concepts have links back to this concept in their
  "Related Files" tables.
- Reachability: the concept is navigable from `_index.md` (at least one link
  path leads from the index to this concept).

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Outbound links | Read "Related Files" tables in this concept's files | Links exist to every concept listed in README "Referenced By" |
| Inbound links | For each related concept, read its "Related Files" tables | At least one file in each related concept links back here |
| Reachable from index | Follow links from `_index.md` | Can reach this concept's README in 1-2 link hops |

**On failure:**
- Missing outbound link: auto-add a row to the appropriate "Related Files"
  table in this concept.
- Missing inbound link: auto-add a row to the related concept's "Related Files"
  table. Update the "Last updated" date on that file.
- Not reachable from index: this means 6.3 also failed. Fix the index entries
  first (6.3), then re-check reachability.

---

### 6.5 Consistency

**What to check:**
- Entity names are spelled and cased identically everywhere they appear (across
  all files that reference the entity, not just the owner).
- No contradictions between the owner's definition and how referencing files
  describe the entity or its behavior.

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Entity naming | Search all `.md` files for each owned entity name | Same spelling and casing in every occurrence |
| Field naming | Compare field names in data-models.md vs behavior files | Fields referenced by exact name (no typos, no aliases) |
| No contradictions | Read descriptions of this entity in referencing files | No behavioral claims that conflict with owner's definition |

**On failure:**
- Name mismatch: auto-fix by replacing the incorrect name with the canonical
  form from the owner's `data-models.md`.
- Contradiction found: **cannot auto-fix**. Report both the owner's definition
  and the conflicting text. Ask the user which is correct. Update the wrong one
  after confirmation.

---

### 6.6 Global Index

**What to check:**
- All concept folders in `docs/concepts/` are listed in `_index.md` Concept
  Overviews table (exclude `archive/` if present).
- All entities (from all `data-models.md` files) appear in the Navigation by
  Entity table.
- No duplicate entity rows in the Navigation by Entity table.

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| All folders listed | Compare `ls docs/concepts/` (dirs only, exclude `_*` and `archive/`) to Concept Overviews table | Every concept folder has a row |
| All entities present | Collect `## <Entity>` headings from all `data-models.md` files; compare to Navigation by Entity table | Every entity has a row |
| No duplicate entities | Scan Navigation by Entity table for repeated entity names | Each entity appears exactly once |
| `_index.md` size | `wc -l docs/concepts/_index.md` | Under 400 lines |

**On failure:**
- Missing folder row: auto-add to Concept Overviews table.
- Missing entity row: auto-add to Navigation by Entity table with links.
- Duplicate entity: remove the duplicate row, keeping the one with correct links.
- `_index.md` over 400 lines: report to user. Suggest moving a table to a
  separate file (e.g., `_entity-index.md`) and linking from `_index.md`.

---

### 6.7 Ownership

**What to check:**
- README.md has an "Owned Entities" section listing every entity this concept
  canonically owns.
- README.md has a "Referenced By" section listing every concept that references
  entities owned by this concept.
- Both sections match the entries in `_ownership.md`.

**How to check:**

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Owned Entities declared | Read README.md "Owned Entities" section | Section exists with a bulleted entity list |
| Referenced By declared | Read README.md "Referenced By" section | Section exists with a bulleted concept list |
| Matches ownership registry | Compare README lists to `_ownership.md` rows where this concept is Canonical Owner | Entity lists match exactly |
| References match | Compare README "Referenced By" to `_ownership.md` "Referenced By" column | Concept lists match exactly |
| New entities registered | Check `_ownership.md` for all entities in this concept's `data-models.md` | Every entity has a row |

**On failure:**
- Missing "Owned Entities" section: auto-add section with entities from
  `_ownership.md`.
- Mismatch between README and `_ownership.md`: report both lists side by side.
  Auto-fix if one is clearly a subset of the other (add the missing entries).
  If they conflict, ask the user which is correct.

---

### Self-Audit Summary Template

After running all 7 categories, output a summary:

```
Self-audit for <concept>/:
  6.1 Structure:    PASS | FIXED (<what>) | FAIL (<what>)
  6.2 Content:      PASS | FIXED (<what>) | FAIL (<what>)
  6.3 Index:        PASS | FIXED (<what>) | FAIL (<what>)
  6.4 Cross-refs:   PASS | FIXED (<what>) | FAIL (<what>)
  6.5 Consistency:  PASS | FIXED (<what>) | FAIL (<what>)
  6.6 Global index: PASS | FIXED (<what>) | FAIL (<what>)
  6.7 Ownership:    PASS | FIXED (<what>) | FAIL (<what>)
```

If all PASS or FIXED: "Self-audit passed." Proceed.
If any FAIL: list the failures and proposed fixes. Wait for user input.

---

## Mode 2: Compliance Check

**Trigger:** User says "check concept compliance" or "consistency check."

This mode scans ALL files in `docs/concepts/` — not just one concept. It checks
cross-file consistency, contradictions, and alignment with the actual codebase.

### How to Run

1. Read `docs/concepts/_index.md` to get the list of all concept folders.
2. Read `docs/concepts/_ownership.md` for the entity registry.
3. Scan every concept folder. For each file, run the checks below.
4. Collect all violations, grouped by priority.
5. Present the compliance report.
6. For each violation, propose a fix. Wait for user approval before applying.

---

### Priority Levels

| Priority | Criteria | Impact |
|----------|----------|--------|
| Critical | Would cause an AI agent to implement something incorrectly | Contradictory rules, wrong behavior, entity in wrong owner's data-models |
| Moderate | Would cause confusion or slow down an AI agent | Inconsistent terminology, stale field names, missing API docs, wrong route syntax |
| Minor | Cosmetic or structural issues that reduce navigability | Missing cross-refs, ownership registry drift, inaccurate line counts |

---

### Critical Checks

Run these first. Any critical finding should be reported immediately.

**C1. Rule contradictions across files**
- For each behavioral rule (constraints, access control, invariants) defined in
  an owner's file, search all other files for descriptions of the same behavior.
- If two files state different constraints for the same entity or rule, flag it.
- Example: File A says "bypassOLS grants full access" but File B says
  "bypassOLS does not override read-only state."

**C2. Entity defined in wrong owner's data-models.md**
- For each entity in `_ownership.md`, verify its schema (field table or tree
  diagram) appears ONLY in the canonical owner's `data-models.md`.
- If a non-owner file has a full schema definition (not just a reference or
  summary), flag it.

**C3. Contradictory entity relationships**
- Check that relationship descriptions (belongs-to, has-many) are consistent
  between the two ends of the relationship.
- Example: `features/data-models.md` says "TenantFeature belongs to Tenant" but
  `groups/data-models.md` says "TenantFeature belongs to Group."

---

### Moderate Checks

**M1. Terminology inconsistency**
- Scan all files for entity name variations (e.g., "User" vs "user" vs "Users"
  vs "AppUser"). The canonical form is whatever appears in `_ownership.md`.
- Flag any file that uses a non-canonical form in headings or table cells.

**M2. Data model fields vs actual code**
- If the project has a Prisma schema or entity files, compare field names in
  `data-models.md` to the actual code.
- Flag fields that exist in docs but not in code (stale), or in code but not
  in docs (undocumented).

**M3. API endpoints vs actual routes**
- If concept has an `api.md`, extract documented routes.
- Search the codebase for actual controller routes.
- Flag routes that are documented but don't exist, or exist but aren't documented.

**M4. Route syntax consistency**
- All path parameters in concept files must use `{param}` format (OpenAPI style).
- Flag any use of `:param` format (Express style).

**M5. Missing API documentation**
- For each concept, check if it has controllers in the codebase.
- If controllers exist but no `api.md` file is present, flag it.

---

### Minor Checks

**N1. Ownership registry drift**
- Compare every README "Owned Entities" section to `_ownership.md`.
- Flag any mismatch (entity listed in README but not registry, or vice versa).

**N2. Cross-references missing or broken**
- Validate all `[text](path)` links across all concept files.
- Flag broken links (target does not exist) and orphaned concepts (no inbound
  links from other concepts).

**N3. Line count headers inaccurate**
- For each file with a `Lines: ~XXX` header, compare to actual `wc -l`.
- Flag if the difference exceeds 20 lines.

**N4. Last updated dates stale**
- For each file, check if the "Last updated" date is older than the file's
  git last-modified date.
- Flag if the header date is more than 30 days behind the git date.

---

### Compliance Report Format

```markdown
# Concept Compliance Report
Generated: YYYY-MM-DD

## Summary
- Critical: N issues
- Moderate: N issues
- Minor: N issues

---

## Critical Issues (N found)

| # | Location | Issue | Canonical Source | Proposed Fix |
|---|----------|-------|------------------|--------------|
| 1 | file.md:line | Description | owner/file.md says X | Change Y to X |

---

## Moderate Issues (N found)

| # | Location | Issue | Expected | Proposed Fix |
|---|----------|-------|----------|--------------|
| 1 | file.md:line | Description | What it should be | How to fix |

---

## Minor Issues (N found)

| # | Location | Issue | Proposed Fix |
|---|----------|-------|--------------|
| 1 | file.md | Description | How to fix |

---

## Fix Workflow
1. Review each issue above.
2. Approve or reject proposed fixes.
3. Approved fixes will be applied.
4. Re-run compliance check to verify.
```

---

### After the Report

1. Present the report to the user.
2. Ask: "Should I apply the proposed fixes? You can approve all, or specify
   which ones to apply by number."
3. Apply approved fixes.
4. Run the self-audit (Mode 1) on every concept that was modified.
5. If the self-audit finds additional issues, report and fix those too.
6. Confirm: "Compliance check complete. N issues found, M fixed."
