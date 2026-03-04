# Design Validation Framework

Validate proposed designs and implementations against the project's documented
concept rules. Two operating modes (strict and advisory), the validation
workflow, and the process for running a full codebase audit.

The project's actual rules live in `docs/concepts/_validation.md`. This file
teaches you HOW to use those rules.

---

## 1. When to Validate

| Situation | Mode | Trigger |
|-----------|------|---------|
| Implementing a feature or fix | **Strict** | Default when writing code that touches documented concepts |
| Brainstorming or exploring ideas | **Advisory** | User says "brainstorm with concept awareness" or similar |
| Designing a new feature | **Strict** | New feature touches entities, permissions, security, or access patterns in `docs/concepts/` |

**Mode selection:** If the user is asking you to implement, build, or change
code, use strict. If exploring, comparing options, or thinking out loud, use
advisory.

---

## 2. The Validation Workflow

Before implementing any change that touches a documented concept area, walk
through the checklist in `docs/concepts/_validation.md`.

```
Read docs/concepts/_validation.md
  |
  For each rule whose "Area" is relevant to the current change:
  |
  Answer the rule's validation questions against the proposed design
  |
  +-- All pass --> COMPLIANT. Continue implementing.
  |
  +-- Any fail --> VIOLATION DETECTED
        |
        [Strict]   1. STOP   2. DOCUMENT   3. ASK   4. WAIT for confirmation
        [Advisory]  1. FLAG   2. CONTINUE   3. SUMMARIZE at session end
```

**Which rules are "relevant"?** A rule is relevant when the change involves the
same entity, module, or cross-cutting concern in the rule's "Area" column. When
in doubt, check the rule -- a false positive costs seconds; a missed violation
costs hours.

---

## 3. Strict vs Advisory Mode

### 3a. Strict Mode (implementation)

Use when writing, modifying, or reviewing code.

**On violation:**

1. **STOP** immediately. Do not write the violating code.
2. **DOCUMENT** which rule is violated and why the design breaks it.
3. **ASK** the user for explicit override permission.
4. **Only proceed** if the user explicitly confirms the override in chat.

**Warning format:**

> **Concept Violation**: This design [description] violates [rule name/number]
> in `docs/concepts/[file]`. [Why this matters.] Do you want me to proceed
> with this violation?

**Example:**

> **Concept Violation**: This endpoint returns Feature data without checking
> whether the tenant has the feature enabled. This violates rule #3
> ("Two-gate access") in `docs/concepts/features/two-gate-access.md`. Gate 1
> (TenantFeature.isEnabled) must pass before user-level access is evaluated.
> Do you want me to proceed with this violation?

**After override:** If approved, proceed and add a code comment noting the
deliberate deviation:

```
// CONCEPT OVERRIDE: Skipping Gate 1 check per user approval (see [context])
```

### 3b. Advisory Mode (brainstorming)

Use when exploring ideas, comparing approaches, or designing before
implementation.

**On violation:**

1. **FLAG** the conflict inline as a warning.
2. **CONTINUE** exploring -- do not block the conversation.
3. **SUMMARIZE** all conflicts at the end of the brainstorming session.

**Warning format:**

> Heads up: This approach would [description], which conflicts with [rule] in
> `[file]`. We can continue exploring, but keep this in mind.

**Example:**

> Heads up: This approach would have child groups inherit parent visibility,
> which conflicts with the downward-only rule in
> `docs/concepts/groups/rls-visibility.md`. We can continue exploring, but
> keep this in mind.

**End-of-session summary:** List ideas as "Aligned with concepts" (idea +
rule/file) or "Needs adjustment" (idea + conflict + what to change).

---

## 4. How to Add Validation Rules

Rules live in the project's `docs/concepts/_validation.md`. Each rule maps a
domain area to concrete yes/no questions checkable against a proposed design.

### Rule Format

Add rows to the "Design Validation Checklist" table in `_validation.md`:

```markdown
| # | Area | Key File | Validation Questions |
|---|------|----------|----------------------|
| 1 | [Domain area] | `docs/concepts/[path]` | 1) [Yes/no question] 2) [Yes/no question] |
```

### Writing Good Rules

- **Concrete** -- answerable with yes or no, not subjective judgment.
- **Traceable** -- references the concept file that defines correct behavior.
- **Actionable** -- if the answer is "no," the developer knows what to fix.
- **Scoped** -- covers one concern, not an entire subsystem.

### Adding a New Rule

1. Identify the invariant or constraint from a concept file.
2. Phrase it as one or more yes/no questions.
3. Add a row to the checklist table with area name, key file path, and questions.
4. If the rule came from a security invariant, prefix the area with `[security]`.

### When Rules Get Populated

- **Guided conversation (deep bootstrap):** Phase 5 collects invariants and past
  mistakes, converted into rules during file generation.
- **Quick scaffold:** Checklist starts empty. Rules are added as concept folders
  are created via `maintain/create-concept.md`.
- **Ongoing:** Any time a new invariant surfaces during implementation, add it.

---

## 5. Full Codebase Audit

Checks every implementation against every validation rule. Heavyweight
operation -- use for periodic reviews, not every commit.

### Trigger

User says: "Conduct a full codebase audit" or "Run a full validation audit."

### Process

1. **Read `docs/concepts/_validation.md`** to load all rules.
2. **For each rule**, search the codebase for files matching the rule's area
   (entity files, controllers, services, guards, etc.).
3. **For each matching file**, answer the rule's validation questions.
4. **Record violations** in a results table.
5. **Propose fixes** for each violation.

### Results Format

```markdown
## Codebase Audit Results

**Rules checked:** [N] | **Files scanned:** [N] | **Violations found:** [N]

### Violations

| # | Area | File:Line | Issue | Rule Violated |
|---|------|-----------|-------|---------------|
| 1 | [area] | `src/path/file.ts:42` | [what is wrong] | Rule #N |
| 2 | [area] | `src/path/other.ts:18` | [what is wrong] | Rule #M |

### Proposed Fixes

**#1:** [What to change and where.]
**#2:** [What to change and where.]
```

### After the Audit

- Ask the user which violations to fix. Do not auto-fix without permission.
- If a violation is intentional (approved override), the user can dismiss it.
  Add a code comment to prevent future audit flags.

---

## 6. Example Validation Rules

Generic starter rules for most projects. Use as a starting point when
populating `_validation.md`.

| # | Area | Key File | Validation Questions |
|---|------|----------|----------------------|
| 1 | Entity ownership | `_ownership.md` | 1) Does this new entity have a canonical owner? 2) Does the owning concept's README list it? |
| 2 | API access control | `<concept>/api.md` | 1) Does this endpoint enforce authentication? 2) Is authorization documented in the concept's API file? |
| 3 | Cross-cutting changes | `_index.md` | 1) Does this change update all referencing concept files? 2) Are cross-references still valid? |
| 4 | Schema consistency | `<concept>/data-models.md` | 1) Do field names match the canonical schema? 2) Are new fields added to the data model doc? |
| 5 | Concept doc hygiene | `_contributing.md` | 1) Was the 7-category self-audit run after editing? 2) Are all files under 400 lines? |

Replace `<concept>` with actual folder names and add project-specific rules as
concepts are documented.
