---
name: setup-skill-env
description: >
  Use when setting up skill environment for a new or existing project.
  Triggers on: "setup skills", "configure skill environment", "install skills framework".
argument-hint: "[project path or 'current']"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, AskUserQuestion, TodoWrite
---

# Setup Skill Environment Workflow

**Goal:** Configure a complete skill environment for a project, installing primitives, workflows, and protocols, and creating documentation for effective AI coding.

**Intent:** Projects need proper setup for AI coding to be effective. This includes understanding the repo, installing the right skills, configuring worktrees if needed, and creating documentation that guides AI behavior. Without this setup, AI coding is inconsistent and ineffective.

**Scope:** Complete environment setup: repo understanding, skill installation, worktree configuration, documentation creation, and skill customization.

---

## Key Results

1. User requirements understood through interview before setup begins
2. User requirements understood — project state, preferences, desired workflows clear
3. Framework components installed — selected primitives, workflows, protocols in place
4. Project documentation guides AI behavior — CLAUDE.md (or equivalent) enables effective AI coding
5. Setup is functional — installed components work correctly
6. Repository understood — structure, conventions, AI-friendliness assessed (if existing repo)
7. Existing skills resolved — adapted, kept, removed, or left as-is (if existing skills present)

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which setup tasks are complete vs remain
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist AFTER the interview, based on what the interview reveals. Do NOT use a fixed checklist.
- `reasoning.md` — Reason about project state and approach before starting.
- `goals_and_objectives.md` — Ensure setup achieves user's stated goals.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these for setup. If you do not understand a primitive, read it before using it.

- `orient` — Understand repository structure and conventions.
- `define` — Establish user requirements and preferences.
- `validate` — Verify setup works correctly.
- `investigate` — Analyze existing code for risks, complexity, uncertainties.
- `critique` — Evaluate modularity and conventions of existing code.

---

## Constraints

- Detect before assuming (always check project state first)
- Ask user for decisions, don't assume preferences
- Handle both cases: existing repo and new repo
- Create documentation that makes AI coding effective
- Validate setup works before completing

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Project (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Project state** — Existing repo with code? New/empty repo? Existing AI config?
2. **Project type** — Language, framework, patterns used
3. **Team context** — Solo or team? Existing conventions?
4. **Likely needs** — What workflows will this project need?

Output your reasoning.

### Interview the User (→ KR1, KR2)

Before doing anything, interview the user to understand their situation:

**Questions to ask:**

1. **Project state:**
   - Is this an existing project with code, or a new/empty project?
   - Does it have existing AI tool config (`.claude/`, `.cursor/`, etc.)?

2. **Existing skills:**
   - Are there existing skills installed?
   - If yes: Do you want to adapt them to the framework, keep them alongside, remove them, or leave them as-is?

3. **What they want:**
   - Which workflows do they need? (feature, bugfix, refactor, code review, etc.)
   - Do they want worktree support for parallel work?
   - How strict should validation be?

4. **Preferences:**
   - Where should specs and plans for tasks be stored?
   - What documentation format do they prefer?

**Detect what you can automatically** (existing code, AI config, project structure) and confirm with user.

### Determine Best Approach (→ KR1, KR2)

Based on the interview, determine the order of remaining tasks.

**Tasks to complete (order based on project state):**

- **Understand repository** — For existing repos, understand structure and conventions first
- **Handle existing skills** — Adapt, keep, remove, or leave as-is based on user choice
- **Configure tool directories** — Set up `.claude/skills/`, etc.
- **Install framework components** — Primitives, protocols, selected workflows
- **Create/update documentation** — CLAUDE.md or equivalent
- **Configure worktrees** — If user wants parallel work support
- **Validate setup** — Confirm everything works
- **Create summary** — Document what was done

### Understand the Repository (→ KR6)

Use `orient` primitive. For existing repos only.

For existing repos, understand before configuring:

1. **Project structure** — Where is code? Tests? Docs?
2. **Language/framework** — What technologies?
3. **Conventions** — Naming, file organization, patterns
4. **Existing documentation** — README, CONTRIBUTING, etc.
5. **Build/test commands** — How to run tests, build, lint

**Gate:** Confirm understanding with user before proceeding.

### Repository Analysis Report (→ KR6)

Use `investigate` and `critique` primitives. For existing repos only.

Create a comprehensive analysis report highlighting what the AI needs to know to work effectively in this codebase.

**Analysis Categories:**
- Uncertainties — areas where requirements, behavior, or intent are unclear
- Risks — technical and operational risks
- Complexity Analysis — areas of high complexity
- Modularity Critique — code organization and boundaries
- Conventions Critique — consistency of conventions

**Gate:** Review report with user, discuss key findings.

### Handle Existing Skills (→ KR7)

Based on interview, adapt, keep, remove, or leave existing skills as-is.

### Tool Configuration (→ KR3)

Determine AI tool and create directory structure:

- Ask user which AI coding tool (Claude Code, Cursor, Windsurf, etc.)
- Create `${TOOL_CONFIG}/skills`, `${TOOL_CONFIG}/primitives`, `${TOOL_CONFIG}/protocols`

### Worktree Configuration (→ KR3)

If user wants parallel work support:

1. Create Worktrees directory
2. Add shell functions to .zshrc/.bashrc
3. Create .worktreesync file
4. Test the setup

### Workflow Selection (→ KR2, KR3)

Present workflow categories and let user select:

- **Core Development:** feature-workflow, bugfix-workflow, refactor-workflow
- **Code Quality:** code-review-workflow, test-strategy-workflow
- **Parallel Work:** worktree workflows (requires worktrees)
- **Meta:** create-primitive-workflow, create-workflow-workflow

### Skill Customization (→ KR2, KR3)

Ask about customization preferences:

- Spec directory location
- Modularity assessment inclusion
- Documentation requirements strictness
- Validation strictness
- User approval gates frequency

### Install Components (→ KR3)

Install selected components:

1. **Install primitives** — Copy from `primitives/` to `${TOOL_CONFIG}/primitives/`
2. **Install protocols** — Copy from `protocols/` to `${TOOL_CONFIG}/protocols/`
3. **Install workflows** — Copy selected from `workflows/` to `${TOOL_CONFIG}/skills/`
4. **Apply customizations** — Replace placeholders in installed files

### Create Project Documentation (→ KR4)

Create or update CLAUDE.md (or equivalent) with:

- Project Overview
- Directory Structure
- Conventions
- Commands
- Skills Configuration
- Working Agreements

**For existing repos:** Merge with existing CLAUDE.md, don't overwrite.

### Validate Setup (→ KR5)

Use `validate` primitive.

Verify the setup works:

1. Config directory exists
2. Skills installed
3. Primitives installed
4. Protocols installed
5. Spec directory exists
6. Project docs exist
7. Worktrees working (if enabled)

**On failure:** Report what's missing, offer to fix.

### Create Setup Summary (→ KR5)

Write summary file to `${TOOL_CONFIG}/SETUP_SUMMARY.md` documenting:

- Configuration settings
- Installed components
- Quick start examples
- Customizations applied
- How to re-run setup

---

## Progress Tracking

Per `checklists.md` — build checklist using format: `<Skill> - KR<num> - <task>`

---

## Preconditions

**Must be provided:** Project path or confirmation of current directory

**Self-satisfiable:**
- Project structure (detected)
- Existing configuration (detected)
- Framework components (from framework repo)

---

## Postconditions

**Success:**
- Skill environment fully configured
- Selected workflows installed and customized
- Project documentation created/updated
- Setup validated and working

**Failure:**
- User aborts
- Framework repo not accessible
- Installation fails

---

## Recovery

Per `recovery.md`:
- If interrupted mid-setup, detect partial configuration
- Resume from last completed task
- Don't duplicate already-installed components

---

## Iteration

If validation fails:
1. Identify missing components
2. Offer to install/fix
3. Re-validate

---

## Notes for Users

1. **This workflow is run in YOUR project**, not in the framework repo
2. **You need access to the framework repo** to copy components
3. **Re-run anytime** to add workflows or change settings
4. **Worktrees are optional** but recommended for parallel work
5. **Customizations are applied** to installed files, not the framework
