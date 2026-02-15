# Skill Setup Guidelines Folder Intent

## Purpose

This folder contains **setup guides** that help users set up skills for their own repositories following framework conventions.

**These are NOT the skills themselves** - they are guides showing how to create and customize skills.

---

## What Goes Here

**Setup guides for creating skills:**
- Templates showing how to structure skills
- Interview processes for gathering requirements
- Customization guidance for different repo types
- Complete SKILL.md examples ready to copy

**NOT in this folder:**
- The actual SKILL.md files (those go in user's `${TOOL_CONFIG}/skills/` directory)
- Framework documentation (that's in SETUP.md and references/)
- Agent configurations (that's in agent-guidelines/)

---

## Current Guides

### [create-skill.md](create-skill.md)

**What:** Meta-skill that creates custom skills following framework conventions

**Use when:** User wants to create a new skill for their repository

**What it does:**
- Interviews user about the skill to create
- Determines skill type and phases needed
- Generates complete SKILL.md following framework patterns
- Includes all required elements (progress tracking, OKRs, checklists, spec maintenance)

**Output:** `${TOOL_CONFIG}/skills/[skill-name]/SKILL.md` in user's repository

---

### [feature-development.md](feature-development.md)

**What:** Setup guide for feature development skills

**Includes skills:**
- feature-define (write PRD)
- feature-plan (create technical plan)
- feature-impl (implement feature)
- feature-workflow (orchestrates all three)

**Use when:** Repository needs structured feature development process

---

###[bugfixing.md](bugfixing.md)

**What:** Setup guide for bugfixing skills

**Includes skills:**
- bugfix-investigate (diagnose root cause)
- bugfix-impl (implement fix)
- bugfix-workflow (orchestrates both)

**Use when:** Repository needs systematic bug fixing process

---

### [refactoring.md](refactoring.md)

**What:** Setup guide for refactoring skills

**Includes skills:**
- refactor-plan (plan refactoring)
- refactor-impl (execute refactoring)
- refactor-workflow (orchestrates both)

**Use when:** Repository needs code quality improvement process

---

### [parallel-orchestration.md](parallel-orchestration.md)

**What:** Setup guide for parallel work using git worktrees

**Includes:**
- Worktree setup instructions (shell functions, .worktreesync)
- parallel-orchestrate skill (manages parallel tasks)
- Worker skill patterns
- Dependency management

**Use when:** Large features can be split into parallel tasks

---

### [creating-examples.md](creating-examples.md)

**What:** Setup guide for example/tutorial creation skills

**Includes skills:**
- create-example (create code examples)
- create-tutorial (create tutorials)

**Use when:** Repository needs educational content creation

---

## How to Use These Guides

### Step 1: Choose Relevant Guide

Based on your repository needs:
- Building features? → feature-development.md
- Fixing bugs? → bugfixing.md
- Refactoring code? → refactoring.md
- Parallel work? → parallel-orchestration.md
- Creating examples? → creating-examples.md
- Custom skill? → create-skill.md

### Step 2: Follow Setup Process

Each guide includes:
1. **Phase 0: Repository Detection** - Understand your repo structure
2. **Phase 1: Prerequisites** - Validate requirements
3. **Skill Templates** - Complete SKILL.md files to copy
4. **Customization** - How to adapt for your repo type
5. **Testing** - How to verify skills work

### Step 3: Copy and Customize

1. Create skill directory: `mkdir -p ${TOOL_CONFIG}/skills/[skill-name]/`
2. Copy SKILL.md template from guide
3. Customize for your repo type (prototype/production/library)
4. Test the skill
5. Iterate as needed

---

## Folder Structure Pattern

Each guide follows this structure:

```markdown
# [Skill Category] Setup Guide

## Overview
[What skills are in this guide, when to use them]

## Phase 0: Repository Detection
[How to understand repo structure for these skills]

## Phase 1: Prerequisites
[What needs to exist before setting up]

## Skill: [skill-name-1]
[Complete SKILL.md template]

## Skill: [skill-name-2]
[Complete SKILL.md template]

## Integration
[How skills work together]

## Customization
[How to adapt for repo types]

## Testing
[How to verify skills work]
```

---

## Key Principles

### 1. Complete Templates

Every guide includes **complete, copy-paste-ready SKILL.md templates**.

Users should be able to:
- Copy the template
- Customize placeholders (`${TOOL_CONFIG}`, `${SPEC_DIR}`, repo-specific values)
- Use immediately

### 2. Framework Compliance

All skill templates follow framework conventions:
- Progress tracking (state variable, status log, visual indicator)
- Goal and OKRs (outcome-oriented)
- Checklists with conditions
- Spec/plan maintenance
- Phase 0: Read Documentation (for implementation skills)
- Final Phase: Update Documentation (for implementation skills)

### 3. Repo Type Customization

Every template shows how to customize for:
- **Prototype repos:** Simpler, fewer quality gates
- **Production repos:** Comprehensive quality checks
- **Library repos:** Extra API/compatibility focus

### 4. References Framework Docs

Templates reference shared documentation:
- `[See references/goals-and-objectives.md for OKR guidance]`
- `[See references/checklist-management.md for checklist patterns]`
- `[See references/assessment-approaches.md for assessment criteria]`
- `[See references/spec-maintenance.md for progress tracking]`

---

## Creating vs Using Skills

**Important distinction:**

**This folder (skill-setup-guidelines/):**
- Shows HOW to create skills
- Contains templates and guides
- For users setting up their repository

**User's repository (${TOOL_CONFIG}/skills/):**
- Contains actual SKILL.md files
- Used by AI to execute workflows
- Customized for that specific repository

**Example:**
```
# This repository (framework):
skill-setup-guidelines/feature-development.md  # Template/guide

# User's repository:
.claude/skills/feature-define/SKILL.md        # Actual skill
.claude/skills/feature-plan/SKILL.md          # Actual skill
.claude/skills/feature-impl/SKILL.md          # Actual skill
```

---

## Relation to Other Folders

### vs. references/

**references/:** Common patterns to prevent duplication **within this repo**
- Assessment criteria
- OKR guidance
- Checklist patterns

**skill-setup-guidelines/:** Templates and guides for **user repositories**
- Complete skill workflows
- Setup instructions
- Customization examples

### vs. SETUP.md

**SETUP.md:** Overall setup process (repo docs, folder structure, choosing skills)

**skill-setup-guidelines/:** Detailed skill templates and implementation guides

Users read SETUP.md first to understand the overall process, then come here for specific skill templates.

---

## Summary

**This folder helps users:**
- Set up skills for their repositories
- Follow framework conventions
- Customize for repo type
- Get working skills quickly

**Guidelines in this folder:**
- Are complete and copy-ready
- Follow framework patterns
- Reference shared docs
- Show customization
- Include testing guidance

**Users take templates from here → customize → use in their `${TOOL_CONFIG}/skills/` directory.**
