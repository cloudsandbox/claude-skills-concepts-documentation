# concept-docs

A reusable skill for bootstrapping and maintaining structured concept documentation systems.

## What This Is

This skill creates a `docs/concepts/` documentation system designed for AI-agent-friendly
codebases. It gives your project structured templates, an entity ownership registry,
cross-references between domain concepts, automated audits, and design validation rules --
all enforced by a generated CLAUDE.md section. Every file stays under 400 lines so agents
can read them in a single pass.

## Quick Start

1. Copy `skill/` to your skills directory
2. Say **"bootstrap concept docs"** in a new conversation
3. Answer the guided questions (or say **"quick"** for empty templates)

## Installation

### Method 1: Personal skill (per developer)

Each developer runs:

**macOS / Linux:**

```bash
git clone https://github.com/cloudsandbox/claude-skills-concepts-documentation.git /tmp/concept-docs
cp -r /tmp/concept-docs/skill ~/.claude/skills/concept-docs
rm -rf /tmp/concept-docs
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/cloudsandbox/claude-skills-concepts-documentation.git $env:TEMP\concept-docs
Copy-Item -Recurse $env:TEMP\concept-docs\skill $env:USERPROFILE\.claude\skills\concept-docs
Remove-Item -Recurse -Force $env:TEMP\concept-docs
```

**Windows (Command Prompt):**

```cmd
git clone https://github.com/cloudsandbox/claude-skills-concepts-documentation.git %TEMP%\concept-docs
xcopy /E /I %TEMP%\concept-docs\skill %USERPROFILE%\.claude\skills\concept-docs
rmdir /S /Q %TEMP%\concept-docs
```

Available across all their projects. Invoke by saying "bootstrap concept docs" in any
conversation.

### Method 2: Project-level skill (shared via git)

One person adds it to the project repo:

**macOS / Linux:**

```bash
git clone https://github.com/cloudsandbox/claude-skills-concepts-documentation.git /tmp/concept-docs
cp -r /tmp/concept-docs/skill .claude/skills/concept-docs
rm -rf /tmp/concept-docs
git add .claude/skills/concept-docs
git commit -m "chore: add concept-docs skill"
git push
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/cloudsandbox/claude-skills-concepts-documentation.git $env:TEMP\concept-docs
Copy-Item -Recurse $env:TEMP\concept-docs\skill .claude\skills\concept-docs
Remove-Item -Recurse -Force $env:TEMP\concept-docs
git add .claude/skills/concept-docs
git commit -m "chore: add concept-docs skill"
git push
```

Everyone who pulls the repo gets the skill automatically. No individual setup needed.
**This is the recommended approach for teams.**

### Method 3: Git submodule (stays updated)

**macOS / Linux:**

```bash
git submodule add https://github.com/cloudsandbox/claude-skills-concepts-documentation.git .claude/skills/concept-docs-repo
ln -s concept-docs-repo/skill .claude/skills/concept-docs
```

**Windows (PowerShell):**

```powershell
git submodule add https://github.com/cloudsandbox/claude-skills-concepts-documentation.git .claude/skills/concept-docs-repo
New-Item -ItemType Junction -Path .claude\skills\concept-docs -Target .claude\skills\concept-docs-repo\skill
```

The team pulls updates with:

```bash
git submodule update --remote
```

This keeps the skill in sync with upstream changes without manually copying files.

### Method 4: Manual reference

Read the files directly. The templates and workflows are human-readable markdown -- no AI
agent required. Start with `skill/SKILL.md` and follow the links.

## Example Prompts

Here are prompts you can use at each stage of working with the skill.

### Bootstrapping (first time setup)

```
Bootstrap concept docs for this project
```

```
Bootstrap concept docs in quick mode
```

```
Set up concept documentation -- this is a multi-tenant SaaS platform with RBAC
```

```
I want to create structured docs for my API. Bootstrap concept docs.
```

### Adding new concepts

```
Add a new concept for "payments" -- it owns Invoice, Transaction, and Refund entities
```

```
Create a concept for the notification system
```

```
New concept: authentication. It handles login, sessions, and OAuth flows.
```

### Auditing and compliance

```
Audit concepts
```

```
Check concept compliance -- I want to see if all cross-references are valid
```

```
I just edited docs/concepts/users/data-models.md -- run the self-audit
```

### Design validation

```
Validate this design against concepts -- I'm adding a new endpoint that bypasses
the permission check for public resources
```

```
Brainstorm with concept awareness -- I want to explore adding a caching layer
```

```
Conduct a full platform concepts audit
```

### Day-to-day usage

```
What does the permissions concept say about field-level security?
```

```
Summarize the groups concept for a new developer
```

```
Is there a concept for webhooks?
```

```
Does my auth guard implementation follow the security-layers concept rules?
```

## Modes

### Deep mode (default)

A 5-phase guided conversation discovers your domain entities, security model, and
validation rules, then generates fully populated documentation. Phases:

1. **Domain discovery** -- identify bounded contexts and core entities
2. **Relationship mapping** -- ownership, references, cross-concept links
3. **Security & permissions** -- access rules, guard chains, bypass flags
4. **Validation rules** -- design constraints your team must follow
5. **Generation** -- output all files, CLAUDE.md section, and audit pass

### Quick mode

Generates the empty meta-structure immediately. No questions asked. Add concepts later
with the maintenance workflows.

> Say **"quick"** when prompted to skip the interview.

## What Gets Generated

```
docs/concepts/
├── _index.md           # Navigation hub -- read this first
├── _contributing.md    # 8-step creation checklist + 7-category audit
├── _ownership.md       # Entity ownership registry
├── _validation.md      # Design validation checklist
├── <concept>/          # One folder per domain concept (deep mode only)
│   ├── README.md       # Overview, owned entities, routing
│   └── data-models.md  # Entity schemas in ASCII-box format
└── ...
```

The skill also appends a section to your project's `CLAUDE.md` (or creates one) with
always-on behaviors:

- **Auto-audit** -- runs 7-category structural audit after every concept file edit
- **Design validation** -- strict mode blocks violations; advisory mode flags them
- **Detection triggers** -- prompts doc updates when entity files, guards, or DTOs change
- **Concept lookup** -- quick-reference commands for onboarding and review

## After Bootstrap -- Maintenance

Once the docs exist, you can manage them with natural language:

| Command | What It Does |
|---------|--------------|
| "new concept" or "add concept" | Follows the 8-step creation workflow |
| "audit concepts" or "check compliance" | Runs the 7-category structural audit |
| "validate design" | Checks your design against documented rules (strict mode) |
| "brainstorm with concept awareness" | Advisory mode -- flags conflicts without blocking |
| "full platform concepts audit" | Searches the entire codebase for concept violations |

### Adding a concept

The 8-step workflow in `_contributing.md` handles everything:

1. Create folder with `README.md` + `data-models.md`
2. Declare owned entities and "Referenced By" links
3. Update `_index.md` navigation tables
4. Update `_ownership.md` with new entities
5. Add cross-references to related concepts
6. Run the 7-category self-audit
7. Fix any issues found
8. Confirm all checks pass

### Auditing

The audit covers seven categories:

1. **Structure** -- file sizes, required files, headers
2. **Content** -- no duplicates, links resolve
3. **Index** -- navigation tables up to date
4. **Cross-references** -- outbound and inbound links valid
5. **Consistency** -- entity naming, no contradictions
6. **Global index** -- all folders listed, all entities present
7. **Ownership** -- README declarations match `_ownership.md`

## Customization

- Add project-specific validation rules to `docs/concepts/_validation.md`
- Modify detection triggers in the generated CLAUDE.md section
- Add behavior files (e.g., `api.md`, `resolution.md`) to concept folders as your
  domain grows
- Adjust the audit categories in `skill/maintain/audit.md` if your project has
  different structural requirements

## File Overview

| File | Purpose |
|------|---------|
| `skill/SKILL.md` | Entry point -- detects mode, routes to sub-files |
| `skill/bootstrap/guided-conversation.md` | 5-phase domain discovery interview |
| `skill/bootstrap/quick-scaffold.md` | Fast path -- empty templates, no questions |
| `skill/bootstrap/templates.md` | Parameterized templates for all generated files |
| `skill/bootstrap/claude-md-generator.md` | Generates CLAUDE.md section for always-on behaviors |
| `skill/maintain/create-concept.md` | 8-step workflow for adding new concepts |
| `skill/maintain/audit.md` | 7-category self-audit + compliance check |
| `skill/maintain/validation.md` | Design validation with strict/advisory modes |

## Design Principles

- **Agent-first** -- files are sized and structured for LLM context windows (under 400 lines)
- **Human-readable** -- plain markdown throughout; no proprietary formats
- **Self-auditing** -- the system checks its own consistency after every edit
- **Composable** -- use deep mode for full setup, quick mode for scaffolding, or
  individual maintenance commands as needed
- **Team-friendly** -- commit the skill and generated docs to git; everyone benefits

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (for automated workflows)
- Any git-based project (language-agnostic)
- No runtime dependencies -- the skill is pure markdown

## License

MIT
