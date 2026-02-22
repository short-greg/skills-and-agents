---
name: setup-skill-env
description: >
  Use when setting up skill environment for a new or existing project.
  Triggers on: "setup skills", "configure skill environment", "install skills framework".
argument-hint: "[project path or 'current']"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, AskUserQuestion
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Setup Skill Environment Workflow

**Goal:** Configure a complete skill environment for a project, installing primitives, workflows, and protocols, and creating documentation for effective AI coding.

**Intent:** Projects need proper setup for AI coding to be effective. This includes understanding the repo, installing the right skills, configuring worktrees if needed, and creating documentation that guides AI behavior. Without this setup, AI coding is inconsistent and ineffective.

**Scope:** Complete environment setup: repo understanding, skill installation, worktree configuration, documentation creation, and skill customization.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (adaptive to project state with user decision gates)

---

## Key Results

1. Project structure understood and documented
2. **Repo analysis report created** (uncertainties, risks, complexity, modularity)
3. Primitives, workflows, and protocols installed
4. Worktree decision made and configured (if enabled)
5. Project documentation created/updated for AI coding (CLAUDE.md or equivalent)
6. Skill customizations applied based on user preferences
7. Setup validated and working

---

## Available Primitives

`orient`, `define`, `validate`, `investigate`, `critique`

---

## Constraints

- Detect before assuming (always check project state first)
- Ask user for decisions, don't assume preferences
- Handle both cases: existing repo and new repo
- Create documentation that makes AI coding effective
- Validate setup works before completing

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Project state** — Existing repo with code? New/empty repo? Existing AI config?
2. **Project type** — Language, framework, patterns used
3. **Team context** — Solo or team? Existing conventions?
4. **Likely needs** — What workflows will this project need?

Output reasoning before proceeding.

---

## Execution

### Phase 1: Determine Project State

First, determine which case we're in:

**Case A: Existing Repository**
- Has code, conventions, possibly existing docs
- Need to understand before configuring
- May have existing AI tool config

**Case B: New/Empty Repository**
- Little or no code
- Need to establish conventions
- Fresh configuration

**Detection:**
```bash
# Check for existing code
ls -la src/ lib/ app/ 2>/dev/null
git log --oneline -5 2>/dev/null

# Check for existing AI config
ls -la .claude/ .cursor/ .codex/ 2>/dev/null

# Check for existing project docs
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null
```

**Ask user to confirm:**
```
Is this an existing project with code, or a new/empty project?

1. Existing project - has code and conventions to understand
2. New project - fresh setup, establishing conventions
```

---

### Phase 2: Understand the Repository (Existing Repo Only)

**Primitive:** `orient`

For existing repos, understand before configuring:

1. **Project structure** — Where is code? Tests? Docs?
2. **Language/framework** — What technologies?
3. **Conventions** — Naming, file organization, patterns
4. **Existing documentation** — README, CONTRIBUTING, etc.
5. **Build/test commands** — How to run tests, build, lint

**Output understanding:**
```markdown
## Repository Understanding

**Type:** [language/framework]
**Structure:**
- Source: [path]
- Tests: [path]
- Docs: [path]

**Conventions:**
- [Convention 1]
- [Convention 2]

**Commands:**
- Test: [command]
- Build: [command]
- Lint: [command]

**Existing AI config:** [found/none]
```

**Gate:** Confirm understanding with user before proceeding.

---

### Phase 3: Repository Analysis Report (Existing Repo Only)

**Primitives:** `investigate`, `critique`

Create a comprehensive analysis report highlighting what the AI needs to know to work effectively in this codebase.

**Analysis Categories:**

#### 3.1 Uncertainties

Identify areas where requirements, behavior, or intent are unclear:

```markdown
## Uncertainties

| Area | Uncertainty | Impact | Recommendation |
|------|-------------|--------|----------------|
| [Component] | [What's unclear] | [How it affects work] | [How to resolve] |

**Examples:**
- Unclear API contracts between modules
- Undocumented business rules
- Missing specifications for edge cases
- Ambiguous naming that could mean multiple things
- Configuration that isn't explained
```

#### 3.2 Risks

Identify technical and operational risks:

```markdown
## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk description] | Low/Med/High | Low/Med/High | [How to mitigate] |

**Risk categories to assess:**
- **Technical debt** — Code that will cause problems
- **Fragility** — Code that breaks easily
- **Security** — Potential vulnerabilities
- **Performance** — Bottlenecks or scaling issues
- **Dependencies** — Outdated, unmaintained, or risky deps
- **Testing gaps** — Areas without coverage
```

#### 3.3 Complexity Analysis

Identify areas of high complexity:

```markdown
## Complexity

| Component | Complexity | Reason | Recommendation |
|-----------|------------|--------|----------------|
| [Component] | Low/Med/High | [Why complex] | [How to approach] |

**Complexity indicators:**
- Deep nesting (>3 levels)
- Long functions (>50 lines)
- High cyclomatic complexity
- Many dependencies
- Complex state management
- Intricate business logic
```

#### 3.4 Modularity Critique

Evaluate code organization and boundaries:

```markdown
## Modularity Assessment

### Coupling
| Module A | Module B | Coupling Type | Severity | Issue |
|----------|----------|---------------|----------|-------|
| [module] | [module] | [type] | Low/Med/High | [description] |

**Coupling types:**
- Content coupling (direct access to internals)
- Common coupling (shared global state)
- Control coupling (passing control flags)
- Stamp coupling (passing unused data)

### Cohesion
| Module | Cohesion Level | Issue |
|--------|----------------|-------|
| [module] | Low/Med/High | [description] |

**Low cohesion indicators:**
- Module does unrelated things
- Functions don't share data/concepts
- Hard to name the module's purpose

### Boundary Issues
- [Where boundaries are unclear]
- [Where responsibilities overlap]
- [Where dependencies go wrong direction]

### Recommendations
1. [Specific recommendation for improvement]
2. [Specific recommendation for improvement]
```

#### 3.5 Conventions Critique

Evaluate consistency of conventions:

```markdown
## Conventions Assessment

| Convention | Consistency | Issues | Examples |
|------------|-------------|--------|----------|
| Naming | [%] consistent | [issues] | [file:line] |
| File organization | [%] consistent | [issues] | [examples] |
| Error handling | [%] consistent | [issues] | [examples] |
| Testing patterns | [%] consistent | [issues] | [examples] |
```

**Write report to:** `${SPEC_DIR}/repo-analysis-report.md`

**Gate:** Review report with user, discuss key findings.

---

### Phase 4: Tool Configuration

**Determine AI tool:**

Ask user:
```
Which AI coding tool are you using?

1. Claude Code (.claude/)
2. Cursor (.cursor/)
3. Windsurf (.windsurf/)
4. Codex (.codex/)
5. Other (specify path)
```

**Create directory structure:**
```bash
mkdir -p ${TOOL_CONFIG}/skills
mkdir -p ${TOOL_CONFIG}/primitives
mkdir -p ${TOOL_CONFIG}/protocols
```

---

### Phase 5: Worktree Configuration

**Present worktree option with context:**

```markdown
## Worktree Setup

Worktrees enable parallel work on multiple features/bugs simultaneously.

**Benefits:**
- Work on feature A while tests run on feature B
- Keep main branch always clean
- Quick context switching without stashing
- Run multiple AI sessions on different tasks

**Requires:**
- Shell function installation (bash/zsh)
- Worktrees directory parallel to repo
- .worktreesync file for environment sync

**Best for:**
- Projects with long-running features
- Teams working on multiple tasks
- CI-heavy projects benefiting from isolated branches

**Skip if:**
- You work on one task at a time
- Simple project with fast iteration
- You prefer traditional branch switching
```

**Ask user:**
```
Enable worktree support?

1. Yes - Walk me through worktree setup
2. No - Skip worktree setup (can add later)
3. Not sure - Explain more
```

**If yes:** Guide through SETUP_WORKTREE_ENV.md:
1. Create Worktrees directory
2. Add shell functions to .zshrc/.bashrc
3. Create .worktreesync file
4. Test the setup

---

### Phase 6: Workflow Selection

**Present workflow categories:**

```markdown
## Workflow Selection

Select workflows based on your development needs:

### Core Development
- **feature-workflow** - End-to-end feature development (define → design → implement → validate)
- **bugfix-workflow** - Hypothesis-driven bug fixing (investigate → hypothesize → fix → validate)
- **refactor-workflow** - Safe behavior-preserving refactoring

### Code Quality
- **code-review-workflow** - Systematic code review with checklist
- **test-strategy-workflow** - Test design and coverage planning

### Parallel Work (requires worktrees)
- **worktree-orchestrate-workflow** - Coordinate parallel tasks from main repo
- **worktree-task-workflow** - Execute single task in worktree

### Meta (for extending the framework)
- **create-primitive-workflow** - Create new primitives
- **create-workflow-workflow** - Create new workflows
- **setup-skill-env-workflow** - Set up skill environment (this workflow)
```

**Ask user to select:**
```
Which workflows do you need? (Select all that apply)

1. Core Development (feature, bugfix, refactor) - Recommended for most projects
2. Code Quality (code-review, test-strategy)
3. Parallel Work (worktree workflows) - Requires worktree setup
4. Meta workflows (create-primitive, create-workflow)
5. All of the above
6. Let me select individually
```

---

### Phase 7: Skill Customization

**Ask about customization preferences:**

**Spec directory:**
```
Where should specifications and plans be stored?

1. specs/ (recommended)
2. dev-docs/
3. docs/planning/
4. .specs/ (hidden)
5. Other (specify)
```

**Modularity assessment:**
```
Include modularity assessment in workflows?
(Checks code for coupling, complexity, duplication after implementation)

1. Yes - Include modularity validation
2. No - Skip modularity checks
3. Only for production code
```

**Documentation requirements:**
```
How strict should documentation requirements be?

1. Strict - Update docs on every implementation
2. Moderate - Update docs when patterns change
3. Light - Only update when explicitly requested
```

**Validation strictness:**
```
How strict should validation gates be?

1. Strict - Must pass all gates before proceeding
2. Moderate - Warn on failures, allow override
3. Light - Suggestions only, no blocking
```

**User approval gates:**
```
Require user approval at workflow gates?

1. Always - Confirm at every major phase transition
2. Key decisions only - Confirm design and completion
3. Minimal - Only ask when blocked or uncertain
```

---

### Phase 8: Install Components

**Install selected components:**

1. **Install primitives** — Copy from `primitives/` to `${TOOL_CONFIG}/primitives/`
   - orient.md, define.md, design.md, implement.md, validate.md
   - investigate.md, brainstorm.md, critique.md
   - plan.md, bookkeep.md, upkeep.md

2. **Install protocols** — Copy from `protocols/` to `${TOOL_CONFIG}/protocols/`
   - tracking.md, recovery.md, checklist_management.md
   - reasoning_patterns.md, doc_maintenance.md
   - modularity.md, goals_and_objectives.md
   - manage_complexity_uncertainty_risk.md

3. **Install workflows** — Copy selected from `workflows/` to `${TOOL_CONFIG}/skills/`
   - Each workflow becomes a skill directory with SKILL.md

4. **Apply customizations** — Replace placeholders in installed files:
   - `${SPEC_DIR}` → user's spec directory
   - `${TOOL_CONFIG}` → user's tool config path
   - Apply customization preferences (modularity, strictness, etc.)

**Installation structure:**
```
${TOOL_CONFIG}/
├── skills/
│   ├── feature-workflow/
│   │   └── SKILL.md
│   ├── bugfix-workflow/
│   │   └── SKILL.md
│   └── ...
├── primitives/
│   ├── orient.md
│   ├── define.md
│   └── ...
└── protocols/
    ├── tracking.md
    ├── recovery.md
    └── ...
```

---

### Phase 9: Create Project Documentation

**Create or update CLAUDE.md (or equivalent):**

For AI coding to be effective, the project needs documentation that guides AI behavior.

**Required sections:**

```markdown
# Project Name

## Project Overview
[What this project does, its purpose]

## Directory Structure
[Where code, tests, docs live]

## Conventions
[Naming, patterns, style rules]

## Commands
[How to test, build, lint, deploy]

## Skills Configuration
[Path to skills, which workflows available]

## Working Agreements
[How AI should behave in this project]
```

**Ask user for input on each section** if not detectable from existing docs.

**For existing repos:** Merge with existing CLAUDE.md, don't overwrite.

---

### Phase 10: Validate Setup

**Primitive:** `validate`

Verify the setup works:

1. **Config directory exists** — `${TOOL_CONFIG}/` created?
2. **Skills installed** — Selected workflows present?
3. **Primitives installed** — All primitives copied?
4. **Protocols installed** — All protocols copied?
5. **Spec directory exists** — Can write specs?
6. **Project docs exist** — CLAUDE.md created/updated?
7. **Worktrees working** — If enabled, shell functions work?

**Test commands:**
```bash
# Verify skills
ls ${TOOL_CONFIG}/skills/*/SKILL.md

# Verify primitives
ls ${TOOL_CONFIG}/primitives/*.md

# Verify protocols
ls ${TOOL_CONFIG}/protocols/*.md

# Verify spec directory
mkdir -p ${SPEC_DIR} && touch ${SPEC_DIR}/.gitkeep
```

**On failure:** Report what's missing, offer to fix.

---

### Phase 11: Create Setup Summary

**Write summary file:**

**File:** `${TOOL_CONFIG}/SETUP_SUMMARY.md`

```markdown
# Skill Environment Setup Summary

**Configured:** [date]
**Tool:** [tool name]
**Project:** [project name]

## Configuration

| Setting | Value |
|---------|-------|
| Tool config | ${TOOL_CONFIG}/ |
| Spec directory | ${SPEC_DIR}/ |
| Worktrees | [enabled/disabled] |
| Modularity checks | [enabled/disabled] |
| Validation strictness | [strict/moderate/light] |
| User approval gates | [always/key/minimal] |

## Installed Components

### Workflows
- [x] feature-workflow
- [x] bugfix-workflow
- [ ] refactor-workflow (not installed)
...

### Primitives
[list of installed primitives]

### Protocols
[list of installed protocols]

## Quick Start

```bash
# Start a feature
/feature-workflow "implement user authentication"

# Fix a bug
/bugfix-workflow "login fails on mobile"

# Review code
/code-review-workflow PR#123
```

## Customizations Applied

- Spec directory: ${SPEC_DIR}
- Documentation: [strictness level]
- Validation: [strictness level]

## Re-running Setup

To add more workflows or change settings:
- Re-run this workflow
- Or manually copy workflows from the framework repo
```

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

Per `recovery` protocol:
- If interrupted mid-setup, detect partial configuration
- Resume from last completed phase
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
