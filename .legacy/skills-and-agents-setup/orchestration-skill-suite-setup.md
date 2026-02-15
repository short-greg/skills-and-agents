# Orchestration Skill Suite Setup

Setup instructions for skills that manage parallel task execution using Git worktrees.

---

## Overview

The **Orchestration Skill Suite** enables parallel development by breaking PRDs into independent tasks that run in separate Git worktrees. Each task can have its own AI coding session running simultaneously.

**This suite includes:**
- `parallel-orchestrate` - Main orchestrator (plan, setup, status, review, merge)
- `parallel-help` - Reference guide for worktree commands
- `parallel-task-workflow` - Task-type aware router that delegates to feature-impl, bugfix-impl, or refactor-impl

**When to use:**
- Breaking large PRDs into parallelizable tasks
- Running multiple AI coding sessions simultaneously
- Managing task dependencies and execution order
- Coordinating integration of parallel work

**Prerequisites:**
- Git repository
- Git worktrees feature (Git 2.5+)
- Shell functions for worktree management (provided below)

---

## Phase 0: Repository Detection

Before setting up the orchestration suite, detect and validate the repository structure.

### Step 1: Detect Directory Structure

The orchestration suite expects a specific directory layout:

```bash
# Expected structure:
project-base/
├── project/              # Main repo (current location)
│   ├── .worktreesync     # Config file (to be created)
│   └── specs/            # Or dev-docs/, docs/planning/, etc.
└── Worktrees/            # Created at ../Worktrees
```

**Check if structure exists:**
```bash
# From main repo
ls ../Worktrees 2>/dev/null || echo "WARNING: ../Worktrees does not exist"
```

**If ../Worktrees doesn't exist**, guide user to create it:
```bash
mkdir ../Worktrees
```

**If main repo isn't in a `-base` parent folder**, recommend reorganization:
```bash
# Create parent folder
mkdir my-project-base
mv my-project my-project-base/
cd my-project-base
mkdir Worktrees
```

### Step 2: Detect Spec Directory

Find where specifications/plans are stored:

```bash
# Common locations
for dir in specs dev-docs docs/planning .docs planning; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$SPEC_DIR" ]; then
  echo "Where should specs be stored? (e.g., specs/, dev-docs/)"
  # Get user input and create directory
fi
```

**Record the spec directory** for use in skill templates.

### Step 3: Detect Tool Configuration

Find the AI tool's configuration directory:

```bash
# Check for common tool configs
for dir in .claude .cursor .codex .windsurf; do
  if [ -d "$dir" ]; then
    echo "Found tool config: $dir"
    TOOL_CONFIG="$dir"
    break
  fi
done
```

### Step 4: Read Project Conventions

Extract project-specific conventions:

```bash
# Read project documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - Naming conventions
# - Testing commands
# - Commit message format
# - Branch naming patterns
```

**Output Phase 0 Summary:**
```markdown
## Repository Detection Summary

- **Worktrees directory**: ../Worktrees [✓ exists / ✗ needs creation]
- **Spec directory**: specs/ [or detected location]
- **Tool config**: .claude/ [or detected location]
- **Project conventions**: [Found in CLAUDE.md / README.md]
- **Naming pattern**: [Detected pattern for branches, commits, etc.]
```

**Gate:** Do not proceed until all directories exist and conventions are understood.

---

## Phase 1: Validate Prerequisites

Ensure all prerequisites are installed and configured.

### Step 1: Check Git Version

```bash
git --version
# Requires Git 2.5+ for worktrees
```

### Step 2: Check Shell Functions

The orchestration workflow requires helper shell functions. Check if installed:

```bash
type git-worktree-add 2>/dev/null || echo "Shell functions not installed"
type git-worktree-remove 2>/dev/null || echo "Shell functions not installed"
```

**If missing**, guide user through installation:

#### For zsh/bash Users

Add to `~/.zshrc` or `~/.bashrc`:

```bash
# Git Worktree Add - Creates worktree with environment sync
git-worktree-add() {
    local WT_PARENT="../Worktrees"

    if [ ! -d "$WT_PARENT" ]; then
        echo "ERROR: The folder '$WT_PARENT' does not exist."
        echo "Please create it first: mkdir $WT_PARENT"
        return 1
    fi

    if [ "$#" -lt 2 ]; then
        echo "Usage: git-worktree-add <name> <branch>"
        echo ""
        echo "Creates a new Git worktree with environment file sync."
        echo ""
        echo "Examples:"
        echo "  git-worktree-add my-feature feat/my-feature"
        echo "  git-worktree-add bugfix fix/login-issue"
        echo ""
        echo "After creation:"
        echo "  cd ../Worktrees/<name>"
        echo "  <your-ai-tool-command>"
        echo ""
        echo "Config: Add files to .worktreesync to sync untracked files"
        return 1
    fi

    local NAME=$1
    local BRANCH=$2
    local TARGET="$WT_PARENT/$NAME"
    local INCLUDE_FILE=".worktreesync"

    # Create the worktree
    git worktree add "$TARGET" -b "$BRANCH"

    # Sync environment files if config exists
    if [ -f "$INCLUDE_FILE" ]; then
        echo "Syncing environment from $INCLUDE_FILE..."
        while IFS= read -r item || [ -n "$item" ]; do
            [[ -z "$item" || "$item" =~ ^# || ! -e "$item" ]] && continue
            cp -r "$item" "$TARGET/$item"
            echo "  + $item"
        done < "$INCLUDE_FILE"
    else
        echo "No $INCLUDE_FILE found; created standard worktree."
    fi

    # Initialize submodules in the new worktree
    if [ -f "$TARGET/.gitmodules" ]; then
        echo "Initializing submodules..."
        git -C "$TARGET" submodule update --init
    fi

    echo "Successfully created worktree at: $TARGET"
}

# Git Worktree Remove - Cleans up worktree and metadata
git-worktree-remove() {
    local WT_PARENT="../Worktrees"

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-remove <name>"
        echo ""
        echo "Removes a Git worktree and cleans up metadata."
        echo ""
        echo "Examples:"
        echo "  git-worktree-remove my-feature"
        echo ""
        echo "Note: Close all terminals in the worktree first!"
        echo "List worktrees: git worktree list"
        return 1
    fi

    local NAME=$1
    local TARGET="$WT_PARENT/$NAME"

    if git worktree remove "$TARGET"; then
        git worktree prune
        echo "Successfully removed $NAME and cleaned up metadata."
    else
        echo "Error: Could not remove $TARGET."
        echo "Ensure no terminals are open in that directory."
        return 1
    fi
}
```

**Reload shell:**
```bash
source ~/.zshrc  # or source ~/.bashrc
```

#### For PowerShell Users (Windows)

Add to `$PROFILE`:

```powershell
# Git Worktree Add (PowerShell)
function git-worktree-add {
    param(
        [Parameter(Mandatory=$true)][string]$Name,
        [Parameter(Mandatory=$true)][string]$Branch
    )

    $WtParent = "..\Worktrees"

    if (-not (Test-Path $WtParent)) {
        Write-Error "ERROR: The folder '$WtParent' does not exist."
        Write-Host "Please create it first: mkdir $WtParent"
        return
    }

    $Target = Join-Path $WtParent $Name
    $IncludeFile = ".worktreesync"

    git worktree add $Target -b $Branch

    if (Test-Path $IncludeFile) {
        Write-Host "Syncing environment from $IncludeFile..."
        Get-Content $IncludeFile | ForEach-Object {
            $item = $_.Trim()
            if ($item -and -not $item.StartsWith("#") -and (Test-Path $item)) {
                Copy-Item -Path $item -Destination (Join-Path $Target $item) -Recurse -Force
                Write-Host "  + $item"
            }
        }
    } else {
        Write-Host "No $IncludeFile found; created standard worktree."
    }

    # Initialize submodules
    $GitModules = Join-Path $Target ".gitmodules"
    if (Test-Path $GitModules) {
        Write-Host "Initializing submodules..."
        git -C $Target submodule update --init
    }

    Write-Host "Successfully created worktree at: $Target"
}

# Git Worktree Remove (PowerShell)
function git-worktree-remove {
    param(
        [Parameter(Mandatory=$true)][string]$Name
    )

    $Target = Join-Path "..\Worktrees" $Name

    $result = git worktree remove $Target 2>&1
    if ($LASTEXITCODE -eq 0) {
        git worktree prune
        Write-Host "Successfully removed $Name and cleaned up metadata."
    } else {
        Write-Error "Could not remove $Target. Ensure no terminals are open in that directory."
    }
}
```

**Reload PowerShell:**
```powershell
. $PROFILE
```

### Step 3: Create .worktreesync File

Create a `.worktreesync` file that defines which untracked files to copy to new worktrees:

```bash
# Create .worktreesync
cat > .worktreesync << 'EOF'
# Environment files
.env
.env.local

# Tool configuration (adapt to your tool)
.claude
# .cursor
# .codex

# IDE settings (optional)
.vscode/settings.json

# Any other untracked local config needed for development
EOF
```

**Important:** Git only tracks indexed files. Without `.worktreesync`, essential files like `.env` won't be in worktrees.

**Add to .gitignore if it contains sensitive paths:**
```bash
echo ".worktreesync" >> .gitignore
```

### Step 4: Test Worktree Creation

Verify the setup works:

```bash
# Test creating a worktree
git-worktree-add test-worktree test-branch

# Verify it was created
ls ../Worktrees/test-worktree

# Verify .worktreesync files were copied
ls ../Worktrees/test-worktree/.env 2>/dev/null || echo ".env not copied"

# Clean up test
cd <main-repo>
git-worktree-remove test-worktree
git branch -D test-branch
```

**Output Phase 1 Summary:**
```markdown
## Prerequisites Validation

- [✓] Git version 2.5+
- [✓] Shell functions installed
- [✓] .worktreesync created
- [✓] Worktree creation tested successfully
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Define Skills in Suite

This suite includes three skills following the naming conventions.

### Skill 1: `parallel-orchestrate`

**Name:** `parallel-orchestrate` (follows `{category}-{action}` pattern)
**Alternative names:** `parallel-orchestrate`, `task-orchestrate`

**Purpose:** Main orchestrator for parallel task workflows - handles planning, setup, status tracking, code review, and merging.

**Commands:**
- `parallel-orchestrate plan <prd-file>` - Break PRD into tasks with dependencies
- `parallel-orchestrate setup` - Create worktrees for ready tasks
- `parallel-orchestrate status` - Show progress and blocked tasks
- `parallel-orchestrate review` - Code review before merging
- `parallel-orchestrate merge` - Merge in dependency order

**Phases:**
1. **Plan** - Task decomposition and dependency analysis
2. **Setup** - Worktree creation (only for tasks with satisfied dependencies)
3. **Status** - Progress tracking and dependency monitoring
4. **Review** - Code quality review before merge
5. **Merge** - Integration in correct order, unblock waiting tasks

**Output:** Coordinated parallel development with dependency management

### Skill 2: `parallel-help`

**Name:** `parallel-help` (follows `{category}-{action}` pattern)

**Purpose:** Reference guide for worktree commands and troubleshooting.

**Phases:**
1. Show quick command reference
2. Display directory structure diagram
3. Provide common issue solutions
4. Link to full guidelines

**Output:** User guidance on worktree usage

### Skill 3: `parallel-task-workflow`

**Name:** `parallel-task-workflow` (follows `{category}-{action}` pattern)

**Purpose:** Task-type aware router that runs inside worktree sessions. Checks dependencies, detects task type (feature/bugfix/refactor), and routes to the appropriate implementation skill.

**Phases:**
- Phase 0: Dependency check (verify dependencies merged)
- Phase 1: Detect task type (feature, bugfix, or refactor)
- Phase 2: Route to appropriate implementation skill:
  - Feature tasks → `/feature-impl`
  - Bugfix tasks → `/bugfix-impl` (hypothesis-based)
  - Refactor tasks → `/refactor-impl`

**Important:** This skill does NOT implement tasks directly - it routes to the correct implementation skill based on task type.

**Output:** Completed task with passing tests, committed and ready for merge

---

## Phase 3: Provide SKILL.md Templates

Complete templates for each skill in the suite.

### Template 1: parallel-orchestrate

**File:** `{tool-config}/skills/parallel-orchestrate/SKILL.md`

```markdown
---
name: parallel-orchestrate
description: Orchestrate parallel task workflows - plan, setup, status, review, merge
disable-model-invocation: true
argument-hint: "[plan|setup|status|review|merge] [prd-file]"
---

# Orchestrate Parallel Tasks

Main orchestrator for managing parallel development with Git worktrees and task dependencies.

## Commands

- `parallel-orchestrate plan <prd-file>` - Break PRD into tasks
- `parallel-orchestrate setup` - Create worktrees
- `parallel-orchestrate status` - Check progress
- `parallel-orchestrate review` - Code review
- `parallel-orchestrate merge` - Integrate work

## Usage

### Command: plan

Break a PRD into parallelizable tasks with dependency tracking.

**Input:** Path to PRD file
**Output:** Plan file at `{spec-dir}/YYYY-MM-DD-parallel-plan.md`

**Process:**
1. Read PRD and extract requirements
2. Identify natural task boundaries
3. Analyze dependencies between tasks
4. Create dependency graph showing execution waves
5. Generate task specs for each task
6. Save plan file

**Task Spec Format:**
```markdown
# Task: {task-name}

## Goal
[One sentence - what this task accomplishes]

## Description
[2-3 sentences elaborating]

## Dependencies
**This task depends on:** [list of task IDs or "None"]
**Blocks these tasks:** [list of task IDs or "None"]
**Status:** Ready to start / Waiting for dependencies

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Implementation Notes
[Any guidance for the worker]
```

### Command: setup

Create Git worktrees for tasks that are ready (dependencies satisfied).

**Process:**
1. Read parallel plan file
2. For each task in plan:
   - Check if dependencies are satisfied (merged to current branch)
   - If ready, create worktree using `git-worktree-add`
   - Copy task spec to worktree's spec directory
   - Force-add and commit spec (since spec dir may be gitignored)
3. Output instructions for starting worker sessions

**Critical:** When committing specs to worktrees:
```bash
cd ../Worktrees/{task-name}
git add -f {spec-dir}/YYYY-MM-DD-{task}-spec.md
git commit -m "spec: add {task} specification"
```

**Output:**
```markdown
## Worktrees Created

Ready to start (no dependencies):
- {task1}: ../Worktrees/{task1}
- {task2}: ../Worktrees/{task2}

Waiting for dependencies:
- {task3}: waiting for [{task1}]
- {task4}: waiting for [{task2}]

## Next Steps

Start worker sessions in separate terminals:
```bash
cd ../Worktrees/{task1} && {tool-command}
cd ../Worktrees/{task2} && {tool-command}
```
```

### Command: status

Check progress of all tasks and show blocked/ready tasks.

**Process:**
1. List all worktrees: `git worktree list`
2. For each task in plan:
   - Check branch status
   - Check if commits exist
   - Determine if blocked by dependencies
3. Show progress summary

**Output:**
```markdown
## Task Status

Completed:
- [✓] {task1} - merged to main

In Progress:
- [ ] {task2} - 3 commits, not merged

Ready to Start:
- [ ] {task3} - dependencies satisfied, worktree created

Blocked:
- [ ] {task4} - waiting for {task1}, {task2}

## Dependencies

{task1} → {task3}, {task4}
{task2} → {task4}
```

### Command: review

Code review completed branches before merging.

**Process:**
1. For each completed task branch:
   - Read the changes: `git diff main...{branch}`
   - Review against project conventions (CLAUDE.md)
   - Check test coverage
   - Validate acceptance criteria met
   - Provide feedback

**Review Checklist:**
```markdown
## Code Review: {task-name}

### Standards
- [ ] Follows naming conventions
- [ ] Proper import ordering
- [ ] Type hints present
- [ ] Docstrings for public APIs
- [ ] No hardcoded values
- [ ] Error handling implemented

### Design
- [ ] Single Responsibility principle
- [ ] DRY (no duplication)
- [ ] Loosely coupled
- [ ] Reusable components

### Testing
- [ ] All tests pass
- [ ] Coverage adequate
- [ ] Edge cases tested

### Acceptance Criteria
- [ ] All criteria from spec met
```

**Output:** Approved/Needs Changes for each task

### Command: merge

Merge approved branches in dependency order.

**Process:**
1. Read plan to get dependency order
2. Create merge sequence respecting dependencies:
   - Tasks with no dependencies first
   - Then tasks whose dependencies are all merged
3. For each task in sequence:
   - Merge branch to current branch: `git merge {branch}`
   - Run tests to verify no regressions
   - If tests fail, stop and report
   - If tests pass, continue
   - After merge, check which blocked tasks are now unblocked
4. Clean up merged worktrees
5. Delete merged branches (optional)

**Critical:** Merge in correct order to satisfy dependencies!

**Output:**
```markdown
## Merge Results

Wave 1 (no dependencies):
- [✓] Merged {task1}
- [✓] Merged {task2}
  → Tests passing
  → Unblocked: {task3}, {task4}

Wave 2 (after Wave 1):
- [✓] Merged {task3}
- [✓] Merged {task4}
  → Tests passing
  → Unblocked: {task5}

## Next Steps

Unblocked tasks ready for setup:
- {task5} - run `parallel-orchestrate setup` to create worktree
```

## Notes

- The orchestrator merges worktree branches **into its current branch**
- Worker sessions must commit all work before orchestrator can merge
- Specs in `{spec-dir}/` are typically gitignored - must force-add when committing
- Test after each merge to catch integration issues early
```

### Template 2: parallel-help

**File:** `{tool-config}/skills/parallel-help/SKILL.md`

```markdown
---
name: parallel-help
description: Quick reference for Git worktree commands and troubleshooting
---

# Worktree Help

Quick reference guide for Git worktree usage.

## Quick Commands

### Create Worktree
```bash
git-worktree-add <name> <branch>

# Example:
git-worktree-add my-feature feat/my-feature
```

### Remove Worktree
```bash
# Exit the worktree directory first!
cd <main-repo>
git-worktree-remove <name>

# Example:
git-worktree-remove my-feature
```

### List Worktrees
```bash
git worktree list
```

## Directory Structure

```
project-base/
├── project/              # Main repo
│   ├── .worktreesync     # Files to sync to worktrees
│   └── {spec-dir}/       # Specifications
└── Worktrees/            # All worktrees live here
    ├── task1/
    ├── task2/
    └── task3/
```

## Common Issues

| Issue | Solution |
|-------|----------|
| "Worktrees folder does not exist" | Run `mkdir ../Worktrees` |
| "Could not remove worktree" | Close all terminals in that directory first |
| Missing `.env` in worktree | Add `.env` to `.worktreesync` and recreate |
| Branch already exists | Use existing branch: `git worktree add ../Worktrees/name existing-branch` |

## Workflow Example

```bash
# 1. Orchestrator creates plan and worktrees
parallel-orchestrate plan specs/my-prd.md
parallel-orchestrate setup

# 2. Start worker sessions (separate terminals)
cd ../Worktrees/task1 && {tool-command}
cd ../Worktrees/task2 && {tool-command}

# 3. Check progress
parallel-orchestrate status

# 4. Review and merge when done
parallel-orchestrate review
parallel-orchestrate merge
```

## Full Documentation

See: `skills-and-agents-setup/orchestration-skill-suite-setup.md`
```

### Template 3: parallel-task-workflow

**File:** `{tool-config}/skills/parallel-task-workflow/SKILL.md`

```markdown
---
name: parallel-task-workflow
description: Task-type aware router with dependency checking - routes to feature-impl, bugfix-impl, or refactor-impl
disable-model-invocation: true
argument-hint: "[task-spec-file]"
---

# Parallel Task Workflow

Task-type aware router that runs inside worktree sessions to execute tasks with dependency checking.

## Purpose

This skill runs inside each worktree to implement tasks. It checks dependencies first, detects the task type, and routes to the appropriate implementation skill.

## Input

Expects a task specification at `{spec-dir}/YYYY-MM-DD-{task}-spec.md` with:
- Goal and description
- Task Type (feature/bugfix/refactor)
- Dependencies section
- Acceptance criteria
- Implementation notes

## Process

### Phase 0: Dependency Check

**CRITICAL:** Verify all dependencies are merged before starting.

1. Read task spec and extract dependencies
2. For each dependency, check if merged:
   ```bash
   git branch --merged | grep <dependency-branch>
   ```
3. If any dependency not merged:
   - **STOP IMMEDIATELY**
   - Output: "⚠ BLOCKED: This task depends on {dep} which is not merged yet"
   - Instruct user to complete dependencies first
   - Exit

4. If all dependencies satisfied, proceed to Phase 1

### Phase 1: Detect Task Type

Read task spec to determine type:

**Look for Task Type field:**
```markdown
**Task Type:** feature | bugfix | refactor
```

**Or infer from content:**
- Contains "implement", "add feature", "new functionality" → Feature
- Contains "fix bug", "debug", "error", "issue" → Bugfix
- Contains "refactor", "restructure", "improve modularity" → Refactor

**If unclear:** Ask user to clarify task type.

### Phase 2: Route to Appropriate Implementation Skill

Based on task type, invoke the corresponding implementation skill:

#### For Feature Tasks:
```
/feature-impl <task-spec-file>
```

Follows TDD implementation process:
- Design structure
- Write tests first
- Implement feature
- Review modularity
- Commit when complete

#### For Bugfix Tasks:
```
/bugfix-impl <task-spec-file>
```

Follows hypothesis-based debugging:
- Generate 3+ hypotheses
- Test each systematically
- Identify root cause
- Implement minimal fix
- Commit when complete

#### For Refactor Tasks:
```
/refactor-impl <task-spec-file>
```

Follows safe refactoring:
- Preserve tests
- Refactor incrementally
- Verify tests pass
- Review modularity improvement
- Commit when complete

### Phase 3: Verify Completion

After implementation skill completes, verify:
- All changes committed
- Tests passing
- Ready for orchestrator merge

Output: "Task complete and committed. Ready for review and merge by the orchestrator."

## Integration

This skill is invoked by users in worktree sessions, not by the orchestrator directly. The orchestrator creates the worktree and commits the spec; the user then starts this skill within the worktree.

## Notes

- Uses full TDD cycle from implementation suite
- Adds Phase 0 for dependency enforcement
- Must commit before handover to orchestrator
- Orchestrator will merge the worktree branch, so uncommitted work is lost
```

---

## Phase 4: Integration Instructions

How the skills work together.

### Workflow Overview

```
Main Session (Orchestrator)
  └─> parallel-orchestrate plan
      └─> Creates parallel plan with dependencies
          └─> parallel-orchestrate setup
              └─> Creates worktrees for ready tasks
                  └─> User starts worktree sessions
                      └─> Each session runs: parallel-task-workflow
                          └─> Tasks complete and commit
                              └─> parallel-orchestrate status (check progress)
                                  └─> parallel-orchestrate review
                                      └─> parallel-orchestrate merge
                                          └─> Unblock waiting tasks
                                              └─> Repeat setup→implement→merge for next wave
```

### Dependency Flow Example

```
Plan creates:
  Wave 1 (parallel): task1, task2 (no dependencies)
  Wave 2: task3 (needs task1), task4 (needs task2)
  Wave 3: task5 (needs task3 AND task4)

Execution:
1. setup creates worktrees for task1, task2
2. Workers implement task1, task2
3. merge integrates task1, task2 → unblocks task3, task4
4. setup creates worktrees for task3, task4
5. Workers implement task3, task4
6. merge integrates task3, task4 → unblocks task5
7. setup creates worktree for task5
8. Worker implements task5
9. merge integrates task5 → complete
```

### File Locations

```
project/
├── {spec-dir}/
│   ├── YYYY-MM-DD-parallel-plan.md    # Created by: plan
│   ├── YYYY-MM-DD-task1-spec.md       # Created by: plan, copied by: setup
│   ├── YYYY-MM-DD-task2-spec.md       # Created by: plan, copied by: setup
│   └── ...
└── .worktreesync                      # Lists files to sync

../Worktrees/
├── task1/
│   └── {spec-dir}/
│       └── YYYY-MM-DD-task1-spec.md   # Committed by: setup
└── task2/
    └── {spec-dir}/
        └── YYYY-MM-DD-task2-spec.md   # Committed by: setup
```

---

## Phase 5: Testing & Validation

Test the complete orchestration workflow.

### Test 1: Simple Plan (No Dependencies)

Create a simple PRD with 2 independent tasks:

```markdown
# Test PRD

Build two independent utilities:
1. String formatter utility
2. Date formatter utility
```

**Test steps:**
```bash
# 1. Plan
parallel-orchestrate plan test-prd.md

# 2. Setup (should create 2 worktrees)
parallel-orchestrate setup

# 3. Verify worktrees created
ls ../Worktrees/

# 4. Check status
parallel-orchestrate status

# 5. Simulate work (create dummy commits in each worktree)
# Then test review and merge
parallel-orchestrate review
parallel-orchestrate merge
```

### Test 2: Plan with Dependencies

Create a PRD with dependent tasks:

```markdown
# Test PRD with Dependencies

1. Build data model (no dependencies)
2. Build API using data model (depends on 1)
3. Build UI using API (depends on 2)
```

**Test steps:**
```bash
# 1. Plan (should identify dependencies)
parallel-orchestrate plan test-prd-deps.md

# 2. Setup (should only create worktree for task 1)
parallel-orchestrate setup

# Verify only task1 worktree created
ls ../Worktrees/

# 3. Complete task1, merge
# ... work in task1 worktree ...
parallel-orchestrate merge

# 4. Setup again (should create worktree for task 2)
parallel-orchestrate setup

# Verify task2 worktree now created
ls ../Worktrees/

# 5. Repeat until all tasks done
```

### Test 3: Dependency Checking

Test that parallel-task-workflow blocks when dependencies not met:

```bash
# In a worktree for a task with dependencies
# Dependencies not yet merged

parallel-task-workflow

# Should output:
# ⚠ BLOCKED: This task depends on {dep} which is not merged yet
```

### Test 4: Full Workflow

Test the complete orchestration lifecycle:

1. Create realistic PRD
2. Run plan command
3. Run setup command
4. Start worker sessions in worktrees
5. Implement tasks with parallel-task-workflow
6. Check status periodically
7. Review completed work
8. Merge in dependency order
9. Verify unblocked tasks
10. Clean up worktrees

**Success criteria:**
- All tasks completed
- All dependencies respected
- All tests passing after merge
- No lost work
- Clean git history

---

## Troubleshooting

### Issue: Worktrees not created

**Check:**
- Does `../Worktrees` exist?
- Do shell functions work? `type git-worktree-add`
- Are dependencies satisfied? Check plan file

### Issue: Specs not in worktree

**Check:**
- Was spec force-added? `git log` in worktree
- Is spec directory correct in setup command?
- Is spec committed? `git show HEAD`

### Issue: Merge conflicts

**Resolution:**
- Stop merge process
- Review conflicting files
- Resolve manually
- Ensure tests pass
- Complete merge

### Issue: Worker session blocked

**Check:**
- Read spec's Dependencies section
- Verify dependencies merged: `git branch --merged`
- If not merged, complete dependencies first
- Re-run parallel-task-workflow after dependencies merge

---

## Customization

### Adapting to Your Project

1. **Spec directory**: Replace `{spec-dir}` with your actual directory (specs/, dev-docs/, etc.)
2. **Tool command**: Replace `{tool-command}` with your AI tool's CLI command (claude, cursor, etc.)
3. **Tool config**: Replace `{tool-config}` with your tool's directory (.claude/, .cursor/, etc.)
4. **Commit format**: Adjust commit message format to match project conventions
5. **Branch naming**: Adjust branch naming pattern if needed

### Adding to .worktreesync

Common additions:
```
# Python virtual environment (if shared)
../.venv

# Node modules (if using symlinks)
# node_modules

# Database files for local dev
*.db
*.sqlite

# API keys / secrets
.secrets
```

---

## Summary

**This suite provides:**
- ✅ Parallel task execution with Git worktrees
- ✅ Dependency tracking and enforcement
- ✅ Task state management
- ✅ Code review before integration
- ✅ Safe merging in correct order

**Key skills:**
- `parallel-orchestrate` - The orchestrator (plan, setup, status, review, merge)
- `parallel-help` - Quick reference
- `parallel-task-workflow` - Worker implementation

**Prerequisites met:**
- Git 2.5+ with worktree support
- Shell functions installed
- Directory structure configured
- .worktreesync file created

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths
3. Test with a simple PRD
4. Use for parallel development!

---

**Last Updated:** 2026-02-12
**Related:** [Skills and Agents Guidelines](skills-and-agents-setup-guidelines-and-conventions.md)
