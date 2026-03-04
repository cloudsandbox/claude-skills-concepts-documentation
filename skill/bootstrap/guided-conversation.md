# Guided Conversation — Domain Discovery Interview

You are conducting a structured interview to discover the user's domain model and
generate a complete `docs/concepts/` documentation system. This file tells you
HOW to run the interview. Follow it step by step.

## Ground Rules

1. **ONE question at a time.** Never batch questions. Ask, wait, then ask next.
2. **Prefer multiple choice.** Present numbered options when answers are finite.
3. **Summarize before moving on.** End each phase with a compact summary and ask
   "Does this look right?" before proceeding to the next phase.
4. **Allow course corrections.** On "go back," update notes and re-summarize.
5. **Skip intelligently.** If a phase does not apply, confirm and move on.
6. **Keep a running domain model.** Internal scratchpad (not shown to user) of
   entities, owners, subsystems, and relationships. Use it for follow-ups.
7. **Stay conversational.** Plain language. No jargon unless the user starts it.

## Before You Begin

Introduce the process:

> I am going to walk you through a short domain-discovery interview so I can
> generate structured concept documentation for your project. There are 5 phases
> and it usually takes 5-15 minutes. You can say "skip" to advance or "back" to
> revisit. Ready?

Wait for confirmation before starting Phase 1.

## Phase 1 — Project Identity

**Goal:** Understand the project type, audience, and scope. Ask the following
questions one at a time.

### Q1.1 — Project Type

> What type of project is this?
>
> 1. SaaS / web application
> 2. REST or GraphQL API
> 3. CLI tool
> 4. Library / SDK
> 5. Mobile app
> 6. Monorepo with multiple apps
> 7. Something else (please describe)

Record the answer. If "monorepo," ask which apps are included.

### Q1.2 — Primary Users

> Who are the primary users?
>
> 1. Developers (consuming an API or library)
> 2. End users (non-technical, using a UI)
> 3. Admins / operators (managing the system)
> 4. Mixed — multiple user types

If "mixed," ask the user to list the distinct user types.

### Q1.3 — Multi-Tenancy and Roles

> Does your project have any of these? (pick all that apply)
>
> 1. Multi-tenancy (data isolation per customer/org)
> 2. User roles or permissions
> 3. Both multi-tenancy and roles
> 4. Neither — single-user or no access control

Record the answer. This determines whether Phase 3 is needed.

### Q1.4 — Existing Documentation

> Are there any existing docs I should know about?
>
> 1. Yes — README, ADRs, or design docs in the repo
> 2. Yes — external docs (wiki, Notion, Confluence, etc.)
> 3. No existing documentation
> 4. Not sure

If "yes," ask for file paths or URLs. Note them for reference during generation
but do NOT read external URLs automatically.

### Phase 1 Summary

Restate: Type, Users, Multi-tenancy/roles, Existing docs. Ask "Does this look
right?" Wait for confirmation.

## Phase 2 — Entity Discovery

**Goal:** Build a list of core domain entities, their ownership, and
relationships. This is the longest phase -- stay iterative.

### Q2.1 — Core Entity List

> What are the core domain entities in your project? List as many as come to
> mind. These are the "nouns" — things like User, Organization, Invoice, Product,
> Ticket, etc.

Record the list. If the user gives fewer than 3, prompt:

> Those are a good start. Are there any others? Think about what gets created,
> stored, or processed by the system.

### Q2.2 — Entity Deep-Dive (repeat for each entity)

For each entity the user listed, ask ONE round of questions. Do not ask all
sub-questions at once — pick the most relevant based on context.

**Ownership:**
> Who or what "owns" a [Entity]? For example, does it belong to a User, a
> Tenant, or is it system-wide?

**Key fields:**
> What are the most important fields on [Entity]? Just the top 3-5 that define
> it — I do not need every column.

**Relationships:**
> How does [Entity] relate to the other entities you mentioned? For example:
> - A Ticket belongs to a Customer
> - An Order contains many LineItems
> - A User can have many Roles

Adapt the depth based on the number of entities. For projects with more than
8 entities, ask ownership and relationships only — skip key fields to keep the
interview moving. You can discover fields later from code.

### Q2.3 — Cross-Cutting Entities

> Looking at the relationships, some entities get referenced by almost
> everything — like User, Tenant, or Organization. Which of your entities would
> you call "cross-cutting"?

If the user is unsure, suggest candidates based on the relationships collected.

### Q2.4 — Subsystem Grouping

> Let me suggest some logical groupings based on what you have described.
> Each group will become a concept folder in the docs.

Present a proposed grouping as a numbered list. For example:

> 1. **identity** — User, Role, Permission
> 2. **billing** — Invoice, Subscription, Payment
> 3. **support** — Ticket, Comment, SLA
>
> Does this grouping make sense? Would you rename, merge, or split any of these?

Iterate until the user approves the groupings.

### Phase 2 Summary

Present a table of subsystems, their entities, and which are cross-cutting.
List key relationships below the table. Ask "Does this look right?"
Wait for confirmation.

## Phase 3 — Security & Access Model

**Goal:** Understand auth patterns. Feeds `security-layers/` and `_validation.md`.

### Skip Check

If Phase 1 Q1.3 answer was "Neither," confirm:

> You mentioned there is no access control. Should I skip the security section,
> or is there anything lightweight (API keys, basic auth) worth documenting?

If the user confirms skip, move directly to Phase 4.

### Q3.1 — Auth Type

> What kind of authentication does your project use?
>
> 1. Session-based (cookies)
> 2. Token-based (JWT, OAuth2)
> 3. API keys
> 4. SSO / SAML / OIDC
> 5. Multiple methods
> 6. Not sure — I can check the code later

### Q3.2 — Authorization Model

> How does your project decide what a user can do?
>
> 1. Role-based (RBAC) — users have roles, roles have permissions
> 2. Attribute-based (ABAC) — rules based on user/resource attributes
> 3. Row-level security (RLS) — users see only their own data
> 4. Field-level security (FLS) — some fields hidden per role
> 5. Combination of the above
> 6. Simple — just "logged in" vs "not logged in"

If "combination," ask which specific combination.

### Q3.3 — Access Control Invariants

> What access control rules must NEVER be violated? These are the rules that, if
> broken, would be a security incident. For example:
> - "A user must never see another tenant's data"
> - "Only admins can delete records"
> - "API keys must not grant write access"
>
> List as many as you can think of.

Record these carefully — they become validation rules in `_validation.md`.

### Phase 3 Summary

Restate: Auth type, Authorization model, Invariants (numbered list). Ask "Does
this look right?" Wait for confirmation.

## Phase 4 — Architecture Boundaries

**Goal:** Finalize subsystem groupings (concept folders) and map dependencies.

### Q4.1 — Confirm Subsystems

> In Phase 2 we grouped your entities into these subsystems:
>
> [list from Phase 2]
>
> Now that we have discussed security, should security/auth be its own
> subsystem, or does it fit inside an existing one?

If Phase 3 was skipped, omit the security question and just confirm the
Phase 2 groupings.

### Q4.2 — Dependencies

> Which subsystems depend on which? For example:
> - "billing depends on identity (needs to know the user)"
> - "support depends on identity and billing"
>
> I will use these to set up cross-references between concept folders.

Present a dependency diagram if there are more than 3 subsystems:

> ```
> identity <-- billing
> identity <-- support
> billing  <-- support
> ```
>
> Does this capture the dependencies?

### Q4.3 — Architectural Patterns

> Are there any architectural patterns the team follows that should be
> documented? For example:
>
> 1. Event-driven / pub-sub
> 2. CQRS (separate read/write models)
> 3. Repository pattern for data access
> 4. Service layer between controllers and repositories
> 5. Specific naming conventions
> 6. None that come to mind

Record patterns. These go into the relevant concept READMEs.

### Phase 4 Summary

Present a table: Subsystem, Depends On, Key Patterns. Ask "Does this look
right?" Wait for confirmation.

## Phase 5 — Validation Rules

**Goal:** Capture design mistakes and invariants that the AI should enforce
during implementation. These become entries in `_validation.md`.

### Q5.1 — Past Mistakes

> What design mistakes have bitten the team before? Things like:
> - "We forgot to check tenant isolation in a new endpoint"
> - "Someone added a direct DB query instead of using the repository"
> - "A migration dropped a column that was still referenced"
>
> These do not have to be security-related — any recurring mistake counts.

### Q5.2 — Enforcement Rules

> Based on everything we have discussed, what rules should I enforce when
> helping with implementation? For example:
> - "Always check Gate 1 + Gate 2 before granting feature access"
> - "Never use raw SQL — always go through the ORM"
> - "Every new endpoint must have an integration test"
>
> I will add these to the validation checklist so they get flagged
> automatically.

### Q5.3 — Fragile Invariants

> Are there any invariants that are easy to accidentally violate? Things that
> look safe but are actually dangerous?

If the user is unsure, suggest examples based on earlier phases:

> Based on what you told me, some candidates might be:
> - [invariant derived from Phase 3]
> - [pattern from Phase 4 that could be bypassed]
>
> Do any of those resonate, or can you think of others?

### Phase 5 Summary

Present a numbered table: Rule, Source (which phase it came from). Ask "Does
this look right?" Wait for confirmation.

## Final Summary and Generation

After all 5 phases, present the **complete domain model** for approval:

- **Project:** type, users, multi-tenancy, auth, authz
- **Subsystems and Entities:** table from Phase 4, enriched with Phase 2 entities
- **Cross-cutting entities:** list from Phase 2
- **Key relationships:** list from Phase 2
- **Validation rules:** numbered list from Phases 3 and 5
- **Subsystem dependencies:** diagram from Phase 4

End with: "Shall I generate the documentation now?" Wait for explicit approval.

### On Approval — Generate Files

1. **Read `../bootstrap/templates.md`** to load the file templates.
2. **Generate `docs/concepts/` files** using templates populated with interview
   data. Required files:
   - `_index.md` — navigation hub listing all subsystems
   - `_ownership.md` — entity ownership registry
   - `_contributing.md` — creation checklist and self-audit
   - `_validation.md` — design validation rules from Phases 3 and 5
   - One folder per subsystem containing `README.md` and `data-models.md`
3. **Read `../bootstrap/claude-md-generator.md`** and follow its instructions to
   generate or append a CLAUDE.md section with concept-docs integration rules.
4. **Run the 7-category self-audit** (from the generated `_contributing.md`) on
   all files. Fix issues before showing results.
5. **Show the file tree** and summary. Ask if the user wants to review any file.

## Edge Cases

| Situation | How to Handle |
| --- | --- |
| User skips a phase | Record "skipped." Generate defaults flagged with `<!-- REVIEW: defaults, not interview data -->`. |
| Very short answers | Probe once. If still minimal, accept and move on. |
| Import from existing docs | Ask for file path, read it, extract entities/relationships, confirm interpretation. |
| 15+ entities | Skip field discovery in Q2.2. Focus on ownership and relationships only. |
| User unsure | Say "I will mark this as TBD." Use `<!-- TBD -->` in generated files. |
