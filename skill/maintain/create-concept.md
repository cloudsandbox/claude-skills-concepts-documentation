# Create Concept — 8-Step Workflow

> **When**: `docs/concepts/_index.md` exists and user says "new concept" or "add concept"
> **Goal**: Add a new domain concept to an existing `docs/concepts/` system

## Before You Start

1. **Ask for the concept name** if the user has not provided one.
2. **Ask for owned entities** if the user has not listed them. You need at least
   one entity name to proceed. If the concept is purely behavioral (no database
   entities), confirm this explicitly — `data-models.md` can be omitted.
3. **Check `_ownership.md`** — verify none of the proposed entities already exist.
   If any do, tell the user which concept already owns them and ask how to proceed
   (extend existing concept vs. transfer ownership).
4. **Read an existing concept folder** (e.g., the most recently created one) to
   match the format conventions already in use in this project.

Use `{{NAME}}` for the folder name (kebab-case), `{{TITLE}}` for the display
name, and `{{DATE}}` for today (`YYYY-MM-DD`) in all templates below.

---

## Step 1 — Create Folder

```bash
mkdir -p docs/concepts/{{NAME}}
```

Create two files inside: `README.md` and `data-models.md`. If the concept has no
database entities (purely runtime, like `security-layers/`), `data-models.md` may
be omitted — confirm with the user first.

---

## Step 2 — Write README.md

Use the project's existing README format. The standard structure is:

```markdown
# {{TITLE}} System

> **Part of**: [concepts/](../_index.md) · **Lines**: ~XXX · **Last updated**: {{DATE}}

## Summary

<2-3 sentences: what this concept covers and why it exists>

## Owned Entities

These entities are canonically defined in this concept folder:

- Entity1
- Entity2

## Referenced By

- <concept-folder-1>/
- <concept-folder-2>/

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Entity1** | Brief description |
| **Entity2** | Brief description |

## When to Read Which File

| If you need to... | Read |
|-------------------|------|
| Understand entity schemas | [data-models.md](data-models.md) |
| Understand <behavior> | [<behavior>.md](<behavior>.md) |

## Related Files

| Topic | File |
|-------|------|
| <Related concept> | [../<concept>/README.md](../<concept>/README.md) |
```

**Key rules:**

- "Part of" links to `../_index.md` (the parent index).
- "Owned Entities" must list every entity this concept defines. This list will be
  cross-checked against `_ownership.md` during the audit.
- "Referenced By" lists concept folders that reference these entities (can be
  empty initially — populate during Step 8).

---

## Step 3 — Write data-models.md

Document each entity using the ASCII-box tree format (`---`/`--`). Match the
style already used in the project's existing `data-models.md` files.

```markdown
# {{TITLE}} Data Models

> **Part of**: [{{NAME}}/](./README.md) · **Lines**: ~XXX · **Last updated**: {{DATE}}

## Summary

<1-2 sentences>

---

## Entity: <EntityName>

<Short description of purpose>

```
<EntityName>
--- id: UUID (PK)
--- field1: Type [required]
--- field2: Type [optional]
--- tenantId: UUID (FK -> Tenant)
`-- createdAt: DateTime
```

### Constraints

- Unique: `[tenantId, apiName]`
- FK: `tenantId` -> `Tenant.id` (cascade delete)

### Relations

| Relation | Type | Target | Description |
|----------|------|--------|-------------|
| tenant | Many-to-One | Tenant | Owner tenant |

---

## Code References

| Component | Location |
|-----------|----------|
| Entity | `path/to/<entity>.entity.ts` |
| Migration | `prisma/migrations/...` |

## Related Files

| Topic | File |
|-------|------|
| Behavior | [<behavior>.md](<behavior>.md) |
| Parent concept | [../<related>/README.md](../<related>/README.md) |
```

**Key rules:**

- Entity schemas live in exactly ONE `data-models.md`. Never duplicate a schema
  that another concept already owns — link to it instead.
- Use the tree characters already established in the project (`---`/`--`).
- Include constraints, relations, and code references.

---

## Step 4 — Write api.md (Optional)

Only create `api.md` if the concept exposes REST or GraphQL endpoints. Skip this
step if the concept is purely internal.

```markdown
# {{TITLE}} API

> **Part of**: [{{NAME}}/](./README.md) · **Lines**: ~XXX · **Last updated**: {{DATE}}

## Access Control

| Operation | Required Role | Scope |
|-----------|---------------|-------|
| List | TENANT_ADMIN+ | Tenant |
| Create | TENANT_ADMIN+ | Tenant |
| Update | TENANT_ADMIN+ | Tenant |
| Delete | TENANT_ADMIN+ | Tenant |

## Endpoints

### List <Entities>

GET /api/v1/tenant/{tenantId}/<entities>

### Create <Entity>

POST /api/v1/tenant/{tenantId}/<entities>

...

## Code References

| Component | Location |
|-----------|----------|
| Controller | `apps/api/src/.../<entity>.controller.ts` |
| Service | `apps/api/src/.../<entity>.service.ts` |
```

---

## Step 5 — Add Behavior Files (As Needed)

Create one file per distinct behavioral topic. Examples: `resolution.md`,
`lifecycle.md`, `provisioning.md`, `execution.md`.

**Rules for behavior files:**

- Standard header: `> **Part of**: [{{NAME}}/](./README.md) · **Lines**: ~XXX · **Last updated**: {{DATE}}`
- Each file under 400 lines. If a topic exceeds 400 lines, split it.
- End every file with a "Related Files" table linking to sibling files and
  cross-concept files.
- Do not duplicate content from `data-models.md` — link to it.

---

## Step 6 — Update `_index.md`

Open `docs/concepts/_index.md` and update ALL of these sections:

1. **Quick Navigation by Task** — add rows mapping user tasks to the new files.
2. **Navigation by Entity** — add one row per new entity with Definition and
   Behavior/Usage links.
3. **Cross-Cutting Concerns** — add a row if the concept spans multiple areas.
4. **Concept Overviews** — add the new README to the table.
5. **File Structure** — add the new folder and its files to the tree diagram.

If `_index.md` would exceed 400 lines after updates, ask the user before
proceeding. The index may need restructuring.

---

## Step 7 — Update `_ownership.md`

Open `docs/concepts/_ownership.md` and:

1. **Add new entities** to the Ownership Table. Set "Canonical Owner" to
   `{{NAME}}/` and "Referenced By" to the list of concepts that will reference
   these entities (can be empty initially).
2. **Update existing rows** — if the new concept references entities owned by
   other concepts, add `{{NAME}}/` to their "Referenced By" column.

---

## Step 8 — Update Cross-References

This is a two-direction update:

**Outbound (new -> existing):** Add "Related Files" links in the new concept's
files pointing to related existing concept files.

**Inbound (existing -> new):** Open each related existing concept and add a link
back to the new concept in their "Related Files" table or "Referenced By" list.

Common places to update in existing concepts:

- `README.md` "Referenced By" or "Related Files" section
- `data-models.md` "Related Files" footer
- Behavior files that discuss topics the new concept interacts with

---

## Post-Creation Audit

After completing all 8 steps, **immediately run the 7-category self-audit**. Use
the audit checklist from the project's `_contributing.md` (or from
`maintain/audit.md` if available).

### Audit Categories

| # | Category | What to Check |
|---|----------|---------------|
| 6.1 | Structure | Files exist, each <400 lines, headers present |
| 6.2 | Content | No duplicated schemas, all links resolve |
| 6.3 | Index | `_index.md` tables updated for new concept |
| 6.4 | Cross-Refs | Outbound links work, inbound links added |
| 6.5 | Consistency | Entity names match across all files |
| 6.6 | Global Index | All folders in tree, all entities in entity table |
| 6.7 | Ownership | README declarations match `_ownership.md` |

**If any check fails:** fix immediately, re-run that check, then continue. Do
NOT report completion until all 7 categories pass.

---

## Common Mistakes

| Mistake | Prevention |
|---------|------------|
| Entity already exists in `_ownership.md` | Always check ownership registry BEFORE creating (Step 0) |
| Duplicated entity schema in `data-models.md` | Entity schemas live in exactly one file. Link, do not copy. |
| Forgot to update `_index.md` | Step 6 is mandatory. Check all 5 sections. |
| Forgot inbound cross-references | Step 8 requires updating EXISTING concept files, not just the new one |
| File exceeds 400 lines | Split into two files. One topic per behavior file. |
| "Part of" header points to wrong file | README links to `../_index.md`; all other files link to `./README.md` |
| Missing "Referenced By" on existing concepts | When adding to `_ownership.md`, also update affected READMEs |
| Skipped the post-creation audit | Audit is mandatory. Completion is not valid until all 7 checks pass. |
