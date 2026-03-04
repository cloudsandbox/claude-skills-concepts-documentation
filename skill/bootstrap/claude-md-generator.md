# CLAUDE.md Section Generator — Concept Documentation

This file tells the agent how to generate a CLAUDE.md section that provides
always-on behaviors for concept documentation maintenance. The generated block
is ready to append (or merge) into the project's existing `CLAUDE.md`.

Called by `guided-conversation.md` (after file generation) and
`quick-scaffold.md` (Step 4).

---

## Instructions for the Agent

1. **Determine tailoring inputs.** Gather these from interview data (guided
   mode) or fall back to defaults (quick-scaffold mode):
   - `{{PROJECT}}` — project name
   - `{{HAS_AUTH}}` — does the project have authentication/authorization?
   - `{{HAS_MULTI_TENANCY}}` — is the project multi-tenant?
   - `{{FILE_EXT}}` — primary source file extension (e.g., `.ts`, `.py`, `.go`)
   - `{{ENTITY_PATTERN}}` — glob for entity/model files (e.g., `*.entity.ts`,
     `models/*.py`, `*_model.go`)
   - `{{CONTROLLER_PATTERN}}` — glob for route/controller files
   - `{{DTO_PATTERN}}` — glob for DTOs or request/response schemas
   - `{{GUARD_PATTERN}}` — glob for auth guards or middleware

   If running in quick-scaffold mode (no interview data), use the defaults
   shown in the template and add `<!-- Customize these patterns for your
   project -->` comments.

2. **Generate the markdown block** below, substituting variables and pruning
   sections that do not apply (see Tailoring Rules).

3. **Append or merge** into the project's `CLAUDE.md`:
   - If `CLAUDE.md` does not exist, create it with the block as its contents.
   - If `CLAUDE.md` exists, append the block at the end.
   - If a `## Concept Documentation (MANDATORY)` header already exists, ask
     the user before overwriting.

---

## Template

Generate the following markdown block. Lines wrapped in `{{#IF ...}}` /
`{{/IF}}` are conditional — include them only when the condition is true.
Remove the conditional markers from the output.

````markdown
## Concept Documentation (MANDATORY)

When working on domain features, **always read the concept index first**:

**Location:** `docs/concepts/_index.md`

**Key meta-files:**

- `_index.md` — Navigation hub (read first)
- `_ownership.md` — Entity ownership registry
- `_contributing.md` — Creation checklist, templates, self-audit
- `_validation.md` — Design validation rules

**Workflow:**

1. Read `docs/concepts/_index.md` to find the right file
2. Read the specific sub-file for your task (<400 lines each)
3. Follow "Related Files" links if you need additional context

## Auto-Audit Trigger

**After editing ANY file in `docs/concepts/`**, automatically run the
7-category self-audit from `_contributing.md`:

| # | Category | What to Check |
|---|----------|---------------|
| 6.1 | Structure | Required files exist, each <400 lines, headers present |
| 6.2 | Content | No duplicated content, all links resolve |
| 6.3 | Index | `_index.md` tables updated for new/changed concept |
| 6.4 | Cross-Refs | Outbound links work, inbound links exist |
| 6.5 | Consistency | Entity names match across files, no contradictions |
| 6.6 | Global Index | All folders listed in `_index.md`, all entities present |
| 6.7 | Ownership | README "Owned Entities" matches `_ownership.md` |

**This is automatic** — do not ask the user, just run the audit after every
concept file edit. Fix issues immediately and report what was fixed.

## Auto-Detect Concept Updates

After implementing a feature, check if your changes match any detection
trigger below. If they do, ask: "I made changes to `<files>` that may affect
`<concept>`. Should I update the concept documentation?"

| Change Type | File Patterns | Likely Concept Impact |
|-------------|---------------|-----------------------|
| New entity | `{{ENTITY_PATTERN}}`, migrations | `<concept>/data-models.md` |
| API routes | `{{CONTROLLER_PATTERN}}` | `<concept>/api.md` or behavior files |
| DTOs / schemas | `{{DTO_PATTERN}}` | `<concept>/data-models.md` |
{{#IF HAS_AUTH}}
| Guards / middleware | `{{GUARD_PATTERN}}` | `security-layers/` |
| Permission logic | `*permission*`, `*access*` | `permissions/` |
{{/IF}}
{{#IF HAS_MULTI_TENANCY}}
| Tenant isolation | `*tenant*`, `*rls*`, `*visibility*` | `groups/` or `security-layers/` |
{{/IF}}

**Important:** This is a prompt — do not auto-edit concept docs. Always ask
the user first.

## Design Validation

{{#IF HAS_AUTH}}
### Strict Mode (during implementation)

When implementing features that involve permissions, security, or access
control, validate the design against `docs/concepts/_validation.md`:

1. For each relevant area in the checklist, read the linked concept file
2. Answer the validation questions
3. If your design violates a documented rule:
   - **STOP** — do not implement the violating change
   - **DOCUMENT** — explain which rule is violated and why
   - **ASK** — request explicit permission to proceed
   - **Only proceed** if the user explicitly confirms the override
{{/IF}}
{{#IF NOT HAS_AUTH}}
### Strict Mode (during implementation)

When implementing features, validate the design against
`docs/concepts/_validation.md`. If your design violates a documented rule:
STOP, DOCUMENT the violation, and ASK before proceeding.
{{/IF}}

### Advisory Mode (during brainstorming)

When brainstorming or exploring ideas (not implementing), use advisory mode:

- Keep concept rules in mind as you explore ideas
- Flag potential conflicts as warnings — do not block the conversation
- At the end, summarize which ideas align and which need adjustment

## Concept Workflow Quick Reference

| Scenario | Trigger | Behavior |
|----------|---------|----------|
| Create concept | "new concept" / "add concept" | 8-step creation workflow |
| Audit concepts | "audit concepts" / "check compliance" | 7-category self-audit |
| Validate design | "validate design" / "validate against concepts" | Strict: STOP/ASK on violations |
| Brainstorm | "brainstorm with concept awareness" | Advisory: flag, don't block |
| Full codebase audit | "full codebase audit" / "full platform audit" | Search code, report violations |
| Quick lookup | "what does <concept> say about <topic>?" | Read and summarize relevant file |
````

---

## Tailoring Rules

Apply these rules when generating the template above:

| Condition | How to Tailor |
|-----------|---------------|
| `HAS_AUTH` is false | Remove the Guards/middleware and Permission logic rows from the detection table. Use the shorter "Strict Mode" block without security-specific language. |
| `HAS_MULTI_TENANCY` is false | Remove the Tenant isolation row from the detection table. |
| Quick-scaffold mode (no interview data) | Use generic defaults: `ENTITY_PATTERN` = `*model*{{FILE_EXT}}`, `CONTROLLER_PATTERN` = `*controller*{{FILE_EXT}}`, `DTO_PATTERN` = `*dto*{{FILE_EXT}}`, `GUARD_PATTERN` = `*guard*{{FILE_EXT}}`. Add `<!-- Customize these patterns for your project -->` as a comment after the detection trigger table. |
| Guided mode with interview data | Use the actual file patterns discovered during the interview (Phase 1 project type and Phase 4 architectural patterns). |
| `FILE_EXT` unknown | Default to `*.ts`. Add `<!-- Customize file extension for your project -->`. |
