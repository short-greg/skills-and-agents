---
name: setup-skill-env
description: >
  Install the skills-and-agents framework to make a repository AI-friendly.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup skills", "install framework", "make repo AI-friendly".
argument-hint: "[project path or 'current']"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, AskUserQuestion, TodoWrite
---

# Setup Skill Environment Workflow

**Goal:** Install the skills-and-agents framework into a project so AI tools can use it.

**Intent:** Projects need the framework installed correctly for AI coding to work. Skills are lightweight pointers (SKILL.md files) that reference workflows. The workflows, modes, and protocols must be accessible to those skills.

**Scope:** Framework installation: copy framework files, create skill directories with SKILL.md pointers, create basic CLAUDE.md.

---

## Key Results - KR

1. Framework files installed — modes, protocols, workflows copied to accessible location
2. Skills created — SKILL.md pointers in `.claude/skills/[skill-name]/`
3. Project has AI guidance — CLAUDE.md exists with project conventions
4. Setup validated — skills are invocable

## Requirements and Constraints - REQ

1. Progress tracked per `protocols/tracking_and_recovery.md`
2. Ask user for AI tool type and installation approach
3. Do not overwrite existing files without user confirmation

---

## Preconditions

**Required:** Project path or current directory, framework source location

**Elicit if not provided:**
- AI tool type (Claude Code → `.claude/`, Cursor → `.cursor/`, etc.)
- Installation approach (submodule vs copy)

**Optional:** Custom paths

## Postconditions

**Success:** Framework installed, skills created, CLAUDE.md exists, skills invocable

**Failure:** Framework source not accessible, cannot write to directories, user aborts

---

## Steps

MUST read and follow steps in `dynamic_workflow_base.md`

---

## Deliverables

| Deliverable | Format | Location | Validation |
|-------------|--------|----------|------------|
| Framework files | Directory | `.skills-framework/` or copied location | modes/, protocols/, workflows/ present |
| Skill directories | SKILL.md files | `.claude/skills/[name]/SKILL.md` | Files exist, reference workflows correctly |
| CLAUDE.md | Markdown | Project root | File exists, references framework |

---

## Possible Tasks

### Determine Installation Approach (→ KR1)

Execute approach selection using `interviewing` mode when starting setup. Ask user:
1. **AI tool type:** Claude Code, Cursor, Windsurf, other?
2. **Installation method:**
   - Submodule (recommended for updates): `git submodule add ... .skills-framework`
   - Copy files (simpler, no updates): copy to project directory

**Fallback:** Default to submodule for git repos, copy for non-git.

### Install Framework Files (→ KR1)

Execute framework installation when approach determined.

**If submodule:**
```bash
git submodule add https://github.com/short-greg/skills-and-agents.git .skills-framework
```

**If copy:**
Copy entire framework to `.skills-framework/` or user-specified location.

Verify these directories exist:
- `.skills-framework/modes/` (10 files)
- `.skills-framework/protocols/` (13 files)
- `.skills-framework/workflows/` (workflow files)

**Fallback:** If submodule fails, offer copy approach.

### Create Skill Directories (→ KR2)

Execute skill creation when framework files installed. Create skill directories with SKILL.md pointers.

**Structure:**
```
.claude/skills/
├── dev-planning/
│   └── SKILL.md  → references .skills-framework/workflows/dev_planning_workflow.md
├── setup-skill-env/
│   └── SKILL.md  → references .skills-framework/workflows/setup_skill_env_workflow.md
```

**SKILL.md format:**
```markdown
---
name: dev-planning
description: Create a development plan before implementation
argument-hint: "[project or feature to plan]"
---

# Dev Planning

Create a development plan by following [dev_planning_workflow.md](../../../.skills-framework/workflows/dev_planning_workflow.md).
```

**Fallback:** If directory creation fails, report error.

### Create CLAUDE.md (→ KR3)

Execute documentation creation using `defining` mode when CLAUDE.md does not exist. Create CLAUDE.md with:

- Project name and description
- Reference to framework: "This project uses skills-and-agents framework in `.skills-framework/`"
- Key commands (build, test, lint)
- Available skills list

**Fallback:** If CLAUDE.md exists, ask user if they want to update or skip.

### Validate Setup (→ KR4)

Execute validation using `evaluating` mode when installation complete. Check:

1. Framework directory exists with subdirectories
2. Skill directories exist with SKILL.md files
3. SKILL.md files reference valid workflow paths
4. CLAUDE.md exists and references framework

Report any issues.

**Fallback:** If validation fails, offer to re-run failed tasks.

---

## Skill Structure

Each skill is a directory with a SKILL.md file:

```
.claude/skills/
├── dev-planning/
│   └── SKILL.md
├── setup-skill-env/
│   └── SKILL.md
└── [other-skill]/
    └── SKILL.md
```

**SKILL.md** is a lightweight pointer with:
- YAML frontmatter (name, description, argument-hint)
- Reference to the full workflow file
- Brief usage instructions

The actual logic is in the workflow file, which references modes and protocols.

---

## Framework Structure

```
.skills-framework/           # or wherever installed
├── modes/              # Atomic cognitive actions
│   ├── base.md
│   ├── orienting.md
│   ├── defining.md
│   └── ...
├── protocols/               # Reusable patterns
│   ├── tracking_and_recovery.md
│   ├── thinking.md
│   └── ...
└── workflows/               # Multi-step processes
    ├── dynamic_workflow_base.md
    ├── dev_planning_workflow.md
    └── ...
```

---

## Additional Notes

**Why submodule?** Allows pulling framework updates. Use `git submodule update --remote` to get latest.

**Why SKILL.md pointers?** Keeps skills lightweight. The full workflow logic stays in one place (the framework), not duplicated in each skill.
