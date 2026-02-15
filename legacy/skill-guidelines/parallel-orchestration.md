# Parallel Orchestration - Complete Skill Suite

Consolidated setup guide for parallel task execution using Git worktrees: orchestration, task routing, and dependency management.

---

## Overview

The **Parallel Orchestration Suite** enables parallel development by breaking large PRDs into independent tasks that run in separate Git worktrees. Each task can have its own AI coding session running simultaneously, with dependency tracking to ensure correct execution order.

### Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `parallel-orchestrate` | Main orchestrator - plan, setup, status, review, merge | Orchestrator |
| `parallel-task-workflow` | Task-type aware router with dependency checking | Router |
| `parallel-help` | Quick reference for worktree commands | Reference |

### When to Use Each Skill

**Use `parallel-orchestrate` when:**
- Breaking large PRDs into parallelizable tasks
- Managing task dependencies and execution order
- Coordinating integration of parallel work
- Need visibility into task progress and blockers

**Use `parallel-task-workflow` when:**
- Running inside a worktree session to implement a task
- Need dependency checking before starting work
- Want automatic routing to the correct implementation skill (feature-impl, bugfix-impl, refactor-impl)

**Use `parallel-help` when:**
- Need quick reference for worktree commands
- Troubleshooting worktree issues
- Learning the parallel workflow

### When NOT to Use Parallel Orchestration

**Do NOT parallelize when:**
- Tasks have heavy dependencies on each other (would create too many execution waves)
- Tasks are small and can be completed quickly in sequence
- Changes affect the same files (merge conflicts likely)
- Integration complexity outweighs parallelization benefits
- Team size doesn't support multiple concurrent sessions

**Sequential is better when:**
- Single developer, single session
- Tasks build on each other naturally
- Rapid iteration needed
- Codebase area is tightly coupled

### How Skills Work Together

```
parallel-orchestrate (orchestrator)
    │
    ├─► Command: plan <prd-file>
    │       └─► Breaks PRD into tasks
    │       └─► Identifies dependencies
    │       └─► Creates dependency graph
    │       └─► Saves to ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md
    │
    ├─► Command: setup
    │       └─► Reads plan
    │       └─► Creates worktrees for ready tasks (dependencies satisfied)
    │       └─► Copies and commits task specs to worktrees
    │       └─► Outputs instructions for starting sessions
    │
    ├─► [User starts sessions in separate terminals]
    │       └─► Each session runs: parallel-task-workflow
    │           ├─► Phase 0: Check dependencies (STOP if not merged)
    │           ├─► Phase 1: Detect task type (feature/bugfix/refactor)
    │           └─► Phase 2: Route to implementation skill
    │               ├─► feature-impl (for feature tasks)
    │               ├─► bugfix-impl (for bugfix tasks)
    │               └─► refactor-impl (for refactor tasks)
    │
    ├─► Command: status
    │       └─► Shows active, completed, ready, and blocked tasks
    │       └─► Displays dependency status
    │
    ├─► Command: review
    │       └─► Code review of completed branches
    │       └─► Validates against project conventions
    │       └─► Checks acceptance criteria
    │
    └─► Command: merge
            └─► Merges in dependency order
            └─► Tests after each merge
            └─► Identifies newly unblocked tasks
            └─► Cleans up merged worktrees
```

### Key Concepts

**Git Worktrees:**
- Multiple working directories from the same repository
- Each on a different branch
- Enables parallel AI sessions without conflicts
- See: setup-guidelines/worktree-guide.md

**Dependency Management:**
- Tasks can depend on other tasks
- Setup only creates worktrees for tasks with satisfied dependencies
- Worker sessions check dependencies before starting
- Merge happens in correct order to unblock waiting tasks

**Execution Waves:**
- Wave 1: Tasks with no dependencies (parallel)
- Wave 2: Tasks that depend on Wave 1 (parallel within wave)
- Wave 3: Tasks that depend on Wave 2 (parallel within wave)
- And so on...

**Coordination Strategy:**
- Orchestrator manages the overall workflow
- Worker sessions implement individual tasks
- Handover happens via committed work in branches
- Orchestrator merges branches into its current branch

---

## Phase 0: Repository Detection

Follow the standard repository detection process to understand your project structure.

### Reference: setup-guidelines/repo-detection.md

The repository detection process helps customize skills to your project. Key steps:

1. **Detect Spec Directory** - Where PRDs, plans, and task specs are stored
2. **Detect Tool Configuration** - AI tool's config directory
3. **Read Project Conventions** - Naming, approval processes, testing
4. **Check Existing Artifacts** - Review existing plans for format
5. **Analyze Codebase Structure** - Understand code organization

### Parallel Orchestration Specific Checks

```bash
# Find spec directory
for dir in specs dev-docs docs/planning .docs planning docs/prd; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# Check directory structure
ls ../Worktrees 2>/dev/null || echo "WARNING: ../Worktrees does not exist"

# Verify Git version (worktrees require Git 2.5+)
git --version

# Check for shell functions
type git-worktree-add 2>/dev/null || echo "Shell functions not installed"
type git-worktree-remove 2>/dev/null || echo "Shell functions not installed"

# Find tool config
for dir in .claude .cursor .codex .windsurf .ai; do
  if [ -d "$dir" ] || [ -d "$dir/skills" ]; then
    echo "Found tool config: $dir"
    TOOL_CONFIG="$dir"
    break
  fi
done

# Review project conventions
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null
```

### Detection Summary Template

```markdown
## Parallel Orchestration Detection Summary

- **Spec directory**: ${SPEC_DIR}
- **Tool config**: ${TOOL_CONFIG}
- **Worktrees directory**: ../Worktrees [✓ exists / ✗ needs creation]
- **Git version**: [version] [✓ 2.5+ / ✗ upgrade needed]
- **Shell functions**: [✓ installed / ✗ needs installation]
- **Document naming pattern**: [e.g., YYYY-MM-DD-feature-name-prd.md]
- **Project type**: [language/framework]
- **Test location**: [detected test directory]
- **Test framework**: [e.g., pytest, jest, cargo test]
- **Existing parallel plans**: [count]
- **Key conventions**: [summary]
```

**Gate:** Do not proceed until:
- Spec directory exists or location confirmed
- Tool config directory identified
- ../Worktrees directory exists (or user confirms creation)
- Git version is 2.5+
- Shell functions are installed

---

## Phase 1: Prerequisites

Follow the standard prerequisites validation to ensure all required directories and access exist.

### Reference: setup-guidelines/prerequisites.md

Standard validation steps:

1. **Verify Spec Directory Exists** - Create if needed
2. **Verify Tool Skills Directory Exists** - Create base structure
3. **Test File Access** - Ensure read/write permissions
4. **Verify Project Documentation Access** - Can read conventions
5. **Identify Skill Consumers** - Who/what uses these skills

### Reference: setup-guidelines/worktree-guide.md

Git worktree-specific setup:

1. **Organize Repository** - Use `-base` parent folder structure
2. **Add Shell Functions** - Install git-worktree-add and git-worktree-remove
3. **Create .worktreesync File** - Define files to copy to worktrees
4. **Test Worktree Creation** - Verify setup works

### Parallel Orchestration Specific Prerequisites

```bash
# Create spec directory if needed
mkdir -p ${SPEC_DIR}

# Create skill directories
mkdir -p ${TOOL_CONFIG}/skills/parallel-orchestrate
mkdir -p ${TOOL_CONFIG}/skills/parallel-task-workflow
mkdir -p ${TOOL_CONFIG}/skills/parallel-help

# Create Worktrees directory if needed
mkdir -p ../Worktrees

# Create .worktreesync file
cat > .worktreesync << 'EOF'
# Environment files
.env
.env.local

# Tool configuration (adapt to your tool)
${TOOL_CONFIG}

# IDE settings (optional)
.vscode/settings.json

# Any other untracked local config needed for development
EOF

# Verify access
touch ${SPEC_DIR}/.test-access && rm ${SPEC_DIR}/.test-access
touch ${TOOL_CONFIG}/skills/.test-access && rm ${TOOL_CONFIG}/skills/.test-access

# Test worktree creation
git-worktree-add test-worktree test-branch
ls ../Worktrees/test-worktree
ls ../Worktrees/test-worktree/.env 2>/dev/null || echo ".env not copied"
cd <main-repo>
git-worktree-remove test-worktree
git branch -D test-branch
```

### Prerequisites Checklist

```markdown
## Parallel Orchestration Prerequisites

- [ ] Spec directory exists and is writable: ${SPEC_DIR}
- [ ] All skill directories created under ${TOOL_CONFIG}/skills/
- [ ] Worktrees directory exists: ../Worktrees
- [ ] Git version 2.5+ installed
- [ ] Shell functions installed (git-worktree-add, git-worktree-remove)
- [ ] .worktreesync file created
- [ ] Worktree creation tested successfully
- [ ] File access verified
- [ ] Project documentation accessible (CLAUDE.md or README.md)
- [ ] Test framework identified
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Skill Definitions

Complete SKILL.md templates for all skills in the parallel orchestration suite.

---

## Skill 1: parallel-orchestrate

**Purpose:** Main orchestrator for parallel task workflows - handles planning, setup, status tracking, code review, and merging with dependency management.

**File:** `${TOOL_CONFIG}/skills/parallel-orchestrate/SKILL.md`

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

- `parallel-orchestrate plan <prd-file>` - Break PRD into tasks with dependencies
- `parallel-orchestrate setup` - Create worktrees for ready tasks
- `parallel-orchestrate status` - Check progress and show blocked tasks
- `parallel-orchestrate review` - Code review before merging
- `parallel-orchestrate merge` - Integrate work in dependency order

## Usage

### Command: plan

Break a PRD into parallelizable tasks with dependency tracking.

**Input:** Path to PRD file (e.g., ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md)

**Output:** Plan file at ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md

**Process:**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Read PRD and extract requirements [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Identify natural task boundaries
⏸️ Phase 3: Analyze dependencies between tasks
⏸️ Phase 4: Create dependency graph
⏸️ Phase 5: Generate task specs
⏸️ Phase 6: Save plan file

CURRENT TASK:
Phase 1: Reading PRD to understand scope
Status: Extracting requirements and features
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 1: Read PRD and Extract Requirements

1. Read the PRD file
2. Extract all requirements
3. Identify major features
4. Note any explicit task suggestions

**Gate:** PRD understood and requirements extracted.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read PRD and extract requirements
🔄 Phase 2: Identify natural task boundaries [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Analyze dependencies between tasks
⏸️ Phase 4: Create dependency graph
⏸️ Phase 5: Generate task specs
⏸️ Phase 6: Save plan file

CURRENT TASK:
Phase 2: Identifying task boundaries
Status: Determining independent units of work
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 2: Identify Natural Task Boundaries

Look for:
- Features that can be developed independently
- Shared infrastructure vs feature-specific work
- Database models vs API endpoints vs UI components
- Test suites that can be written in parallel

**Guidelines:**
- Aim for 2-6 tasks (too many creates overhead)
- Each task should be completable in one session
- Prefer broader tasks over tiny tasks
- Consider file ownership (minimize overlap)

**Gate:** Task boundaries identified.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read PRD and extract requirements
✅ Phase 2: Identify natural task boundaries
🔄 Phase 3: Analyze dependencies between tasks [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Create dependency graph
⏸️ Phase 5: Generate task specs
⏸️ Phase 6: Save plan file

CURRENT TASK:
Phase 3: Analyzing task dependencies
Status: Determining execution order
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 3: Analyze Dependencies Between Tasks

For each task, determine:
- **Depends on:** Which tasks must complete before this one can start?
- **Blocks:** Which tasks are waiting for this one?

Common dependency patterns:
- Data models → API endpoints → UI components
- Shared utilities → Features that use them
- Infrastructure → Applications
- Core logic → Integration points

**Critical:** Be conservative about dependencies. If unsure, mark as dependent.

**Gate:** All dependencies identified.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read PRD and extract requirements
✅ Phase 2: Identify natural task boundaries
✅ Phase 3: Analyze dependencies between tasks
🔄 Phase 4: Create dependency graph [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Generate task specs
⏸️ Phase 6: Save plan file

CURRENT TASK:
Phase 4: Creating dependency graph
Status: Organizing tasks into execution waves
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 4: Create Dependency Graph

Organize tasks into execution waves:

```
Wave 1 (no dependencies - can start immediately):
- task1
- task2

Wave 2 (depends on Wave 1):
- task3 (needs task1)
- task4 (needs task2)

Wave 3 (depends on Wave 2):
- task5 (needs task3 AND task4)
```

**Sanity check:**
- Are there tasks with no dependencies? (At least one for Wave 1)
- Are there circular dependencies? (Fix if found)
- Is the graph too deep? (Consider simplifying)

**Gate:** Dependency graph created and validated.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read PRD and extract requirements
✅ Phase 2: Identify natural task boundaries
✅ Phase 3: Analyze dependencies between tasks
✅ Phase 4: Create dependency graph
🔄 Phase 5: Generate task specs [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Save plan file

CURRENT TASK:
Phase 5: Generating task specs
Status: Creating specification files for each task
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 5: Generate Task Specs

For each task, create a spec following this template:

```markdown
# Task: {task-name}

## Goal
[One sentence - what this task accomplishes]

## Description
[2-3 sentences elaborating on the goal]

## Task Type
**Task Type:** feature | bugfix | refactor

(This tells parallel-task-workflow which implementation skill to route to)

## Dependencies
**This task depends on:** [list of task IDs or "None"]
**Blocks these tasks:** [list of task IDs or "None"]
**Status:** Ready to start / Waiting for dependencies

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]
- [ ] All tests pass
- [ ] Code follows project conventions

## Implementation Notes
[Any guidance for the worker session]
- Key files to modify
- Patterns to follow
- Edge cases to consider
- Integration points

## Files to Modify
- path/to/file1.py
- path/to/file2.py
- path/to/test_file.py
```

**Gate:** All task specs generated.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-ORCHESTRATE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read PRD and extract requirements
✅ Phase 2: Identify natural task boundaries
✅ Phase 3: Analyze dependencies between tasks
✅ Phase 4: Create dependency graph
✅ Phase 5: Generate task specs
🔄 Phase 6: Save plan file [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 6: Saving plan file
Status: Writing plan to ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 6: Save Plan File

Create ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md with:

```markdown
# Parallel Execution Plan: {Feature Name}

Generated: YYYY-MM-DD
PRD: {link to PRD file}

## Overview

[Brief description of what's being built]

## Task Breakdown

Total tasks: {N}
Execution waves: {M}

## Dependency Graph

```
Wave 1 (parallel):
- task1
- task2

Wave 2 (after Wave 1):
- task3 (needs task1)
- task4 (needs task2)

Wave 3 (after Wave 2):
- task5 (needs task3, task4)
```

## Task Specifications

### Task 1: {task1-name}
[Include full spec here]

### Task 2: {task2-name}
[Include full spec here]

[... continue for all tasks ...]

## Next Steps

1. Run `parallel-orchestrate setup` to create worktrees for Wave 1 tasks
2. Start worker sessions in separate terminals
3. Run `parallel-orchestrate status` to check progress
4. Run `parallel-orchestrate review` before merging
5. Run `parallel-orchestrate merge` to integrate and unblock next wave
```

Also create individual task spec files:
- ${SPEC_DIR}/YYYY-MM-DD-{task1}-spec.md
- ${SPEC_DIR}/YYYY-MM-DD-{task2}-spec.md
- etc.

**Output:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PLAN COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Created parallel execution plan:
- Plan file: ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md
- Task specs: {N} files created

Dependency graph:
- Wave 1: {N1} tasks (ready to start)
- Wave 2: {N2} tasks (waiting for Wave 1)
- Wave 3: {N3} tasks (waiting for Wave 2)

Next step: Run `parallel-orchestrate setup` to create worktrees
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Command: setup

Create Git worktrees for tasks that are ready (dependencies satisfied).

**Input:** Reads most recent parallel plan from ${SPEC_DIR}/

**Output:** Worktrees created in ../Worktrees/, specs committed

**Process:**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 SETUP PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Read parallel plan [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Identify ready tasks
⏸️ Phase 3: Create worktrees
⏸️ Phase 4: Copy and commit specs

CURRENT TASK:
Phase 1: Reading parallel plan
Status: Finding latest plan file
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 1: Read Parallel Plan

1. Find most recent parallel plan in ${SPEC_DIR}/
2. Read task definitions
3. Read dependency graph

**Gate:** Plan file found and parsed.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 SETUP PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read parallel plan
🔄 Phase 2: Identify ready tasks [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Create worktrees
⏸️ Phase 4: Copy and commit specs

CURRENT TASK:
Phase 2: Identifying ready tasks
Status: Checking which tasks have dependencies satisfied
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 2: Identify Ready Tasks

For each task in plan:
1. Check if dependencies are satisfied:
   ```bash
   # For each dependency, check if merged
   git branch --merged | grep <dependency-branch>
   ```
2. If all dependencies merged OR no dependencies → READY
3. If any dependency not merged → BLOCKED

**Output lists:**
- Ready tasks (will create worktrees)
- Blocked tasks (will skip for now)

**Gate:** Task status determined for all tasks.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 SETUP PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read parallel plan
✅ Phase 2: Identify ready tasks
🔄 Phase 3: Create worktrees [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Copy and commit specs

CURRENT TASK:
Phase 3: Creating worktrees
Status: Running git-worktree-add for ready tasks
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 3: Create Worktrees

For each ready task:

1. Determine branch name: feat/{task-name} or fix/{task-name} or refactor/{task-name}
2. Create worktree:
   ```bash
   git-worktree-add {task-name} {branch-name}
   ```
3. Verify creation:
   ```bash
   ls ../Worktrees/{task-name}
   ```

**Gate:** All ready tasks have worktrees created.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 SETUP PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read parallel plan
✅ Phase 2: Identify ready tasks
✅ Phase 3: Create worktrees
🔄 Phase 4: Copy and commit specs [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 4: Copying and committing specs
Status: Force-adding specs to worktree branches
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 4: Copy and Commit Specs

**CRITICAL:** Spec files are typically gitignored, so must force-add.

For each worktree:

1. Copy task spec to worktree:
   ```bash
   cp ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md \
      ../Worktrees/{task}/${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md
   ```

2. Commit spec (force-add because gitignored):
   ```bash
   cd ../Worktrees/{task}
   git add -f ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md
   git commit -m "spec: add {task} specification"
   cd - # Return to main repo
   ```

3. Verify commit:
   ```bash
   git log ../Worktrees/{task} --oneline -1
   ```

**Gate:** All specs committed to worktree branches.

**Output:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ SETUP COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WORKTREES CREATED:

Ready to start (no dependencies):
- task1: ../Worktrees/task1 (branch: feat/task1)
- task2: ../Worktrees/task2 (branch: feat/task2)

Waiting for dependencies:
- task3: waiting for [task1]
- task4: waiting for [task2]
- task5: waiting for [task3, task4]

NEXT STEPS:

Start worker sessions in separate terminals:

Terminal 1:
cd ../Worktrees/task1 && claude

Terminal 2:
cd ../Worktrees/task2 && claude

In each session, run:
/parallel-task-workflow

The workflow will check dependencies, detect task type, and route to the appropriate implementation skill.

Monitor progress:
parallel-orchestrate status

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Command: status

Check progress of all tasks and show blocked/ready tasks.

**Input:** Reads parallel plan from ${SPEC_DIR}/

**Output:** Status report with dependency information

**Process:**

1. Read parallel plan
2. List all worktrees: `git worktree list`
3. For each task in plan:
   - Check if worktree exists
   - Check branch status: `git log <branch> --oneline -5`
   - Check if merged: `git branch --merged | grep <branch>`
   - Determine if blocked by dependencies
4. Generate status report

**Output:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 TASK STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

COMPLETED (merged to current branch):
✅ task1 - 5 commits, merged 10 minutes ago
   Branch: feat/task1
   Unblocked: task3

IN PROGRESS (worktree active, not merged):
🔄 task2 - 3 commits, last commit 2 minutes ago
   Branch: feat/task2
   Worktree: ../Worktrees/task2

READY TO START (dependencies satisfied, no worktree):
🟢 task3 - dependencies satisfied (task1 merged)
   Run `parallel-orchestrate setup` to create worktree

BLOCKED (waiting for dependencies):
⏸️ task4 - waiting for task2
⏸️ task5 - waiting for task3, task4

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DEPENDENCY GRAPH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

task1 (✅) → task3 (🟢)
task2 (🔄) → task4 (⏸️)
task3 (🟢) → task5 (⏸️)
task4 (⏸️) → task5 (⏸️)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Wait for task2 to complete
2. Review and merge task2
3. Run `parallel-orchestrate setup` to create worktree for task4
4. Continue monitoring with `parallel-orchestrate status`

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Command: review

Code review completed branches before merging.

**Input:** Reads parallel plan and checks for completed branches

**Output:** Review report with approval/needs-changes status

**Process:**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REVIEW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Identify completed tasks [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Review each task
⏸️ Phase 3: Generate review report

CURRENT TASK:
Phase 1: Identifying completed tasks
Status: Finding branches ready for review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 1: Identify Completed Tasks

Find tasks that are:
- Have commits on their branch
- Not yet merged
- Have active worktrees or completed commits

```bash
# For each task branch
git log --oneline <branch> ^<base-branch>
```

**Gate:** Completed tasks identified.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REVIEW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Identify completed tasks
🔄 Phase 2: Review each task [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Generate review report

CURRENT TASK:
Phase 2: Reviewing task1
Status: Checking code quality and acceptance criteria
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 2: Review Each Task

For each completed task:

1. **Read the changes:**
   ```bash
   git diff <base-branch>...<task-branch>
   ```

2. **Review against project conventions:**
   - Read CLAUDE.md or README.md
   - Check naming conventions
   - Verify coding standards
   - Check import ordering
   - Validate error handling

3. **Check test coverage:**
   - Are there tests?
   - Do tests cover edge cases?
   - Are error cases tested?

4. **Validate acceptance criteria:**
   - Read task spec
   - Check each criterion
   - Verify all met

5. **Generate review checklist:**

```markdown
## Code Review: {task-name}

### Standards
- [ ] Follows naming conventions
- [ ] Proper import ordering
- [ ] Type hints present (if applicable)
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
- [ ] Error cases tested

### Acceptance Criteria
- [ ] Criterion 1 met
- [ ] Criterion 2 met
- [ ] Criterion 3 met
- [ ] All criteria from spec met

### Decision
- [ ] ✅ APPROVED - Ready to merge
- [ ] ⚠️ NEEDS CHANGES - Issues found

### Issues Found (if any)
1. [Issue description]
2. [Issue description]
```

**Repeat for each task.**

**Gate:** All tasks reviewed.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REVIEW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Identify completed tasks
✅ Phase 2: Review each task
🔄 Phase 3: Generate review report [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 3: Generating review report
Status: Summarizing review results
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 3: Generate Review Report

**Output:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 REVIEW REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

APPROVED (ready to merge):
✅ task1
   - All standards met
   - Tests comprehensive
   - All acceptance criteria met

✅ task2
   - All standards met
   - Tests comprehensive
   - All acceptance criteria met

NEEDS CHANGES:
(none)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT STEPS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Run `parallel-orchestrate merge` to integrate approved tasks

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Command: merge

Merge approved branches in dependency order.

**Input:** Reads parallel plan for dependency order

**Output:** Merged branches, unblocked tasks identified

**Process:**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MERGE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Read dependency order [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Create merge sequence
⏸️ Phase 3: Merge tasks
⏸️ Phase 4: Identify unblocked tasks
⏸️ Phase 5: Clean up worktrees

CURRENT TASK:
Phase 1: Reading dependency order
Status: Loading parallel plan
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 1: Read Dependency Order

1. Read parallel plan
2. Extract dependency graph
3. Identify execution waves

**Gate:** Dependency order understood.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MERGE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read dependency order
🔄 Phase 2: Create merge sequence [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Merge tasks
⏸️ Phase 4: Identify unblocked tasks
⏸️ Phase 5: Clean up worktrees

CURRENT TASK:
Phase 2: Creating merge sequence
Status: Ordering tasks by dependencies
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 2: Create Merge Sequence

**CRITICAL:** Merge in dependency order!

Create sequence respecting dependencies:
1. Tasks with no dependencies first (Wave 1)
2. Then tasks whose dependencies are all merged (Wave 2)
3. Continue for all waves

**Example sequence:**
```
Merge order:
1. task1 (Wave 1 - no dependencies)
2. task2 (Wave 1 - no dependencies)
3. task3 (Wave 2 - needs task1)
4. task4 (Wave 2 - needs task2)
5. task5 (Wave 3 - needs task3, task4)
```

**Gate:** Merge sequence created and validated.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MERGE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read dependency order
✅ Phase 2: Create merge sequence
🔄 Phase 3: Merge tasks [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Identify unblocked tasks
⏸️ Phase 5: Clean up worktrees

CURRENT TASK:
Phase 3: Merging task1
Status: Integrating task1 branch
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 3: Merge Tasks

For each task in sequence:

1. **Merge branch:**
   ```bash
   git merge {task-branch} --no-ff -m "Merge {task-branch}: {task description}"
   ```

2. **Handle conflicts (if any):**
   - Review conflicting files
   - Resolve conflicts
   - Mark as resolved
   - Complete merge

3. **Run tests:**
   ```bash
   # Run project test suite
   pytest  # or npm test, cargo test, etc.
   ```

4. **If tests fail:**
   - STOP immediately
   - Report which tests failed
   - Do NOT continue merging
   - Fix tests or revert merge
   - User must address before continuing

5. **If tests pass:**
   - Continue to next task
   - Note which tasks were unblocked

**Repeat for each task in sequence.**

**Gate:** All tasks merged successfully with tests passing.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MERGE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read dependency order
✅ Phase 2: Create merge sequence
✅ Phase 3: Merge tasks
🔄 Phase 4: Identify unblocked tasks [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Clean up worktrees

CURRENT TASK:
Phase 4: Identifying unblocked tasks
Status: Checking which tasks can now start
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 4: Identify Unblocked Tasks

After merging, check which blocked tasks are now ready:

For each blocked task:
1. Check if all dependencies are now merged
2. If yes → Task is UNBLOCKED and ready for setup
3. If no → Task remains blocked

**Gate:** Unblocked tasks identified.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MERGE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 1: Read dependency order
✅ Phase 2: Create merge sequence
✅ Phase 3: Merge tasks
✅ Phase 4: Identify unblocked tasks
🔄 Phase 5: Clean up worktrees [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 5: Cleaning up worktrees
Status: Removing merged worktrees
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

#### Phase 5: Clean Up Worktrees

For each merged task:

1. **Remove worktree:**
   ```bash
   cd <main-repo>  # Ensure not in worktree directory
   git-worktree-remove {task-name}
   ```

2. **(Optional) Delete branch:**
   ```bash
   git branch -d {task-branch}  # Only if user confirms
   ```

**Output:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ MERGE COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

MERGED TASKS:

Wave 1 (no dependencies):
✅ task1 - Merged successfully
✅ task2 - Merged successfully
   → Tests passing
   → Unblocked: task3, task4

NEWLY UNBLOCKED TASKS:

🟢 task3 - dependencies satisfied (task1 merged)
🟢 task4 - dependencies satisfied (task2 merged)

STILL BLOCKED:

⏸️ task5 - waiting for task3, task4

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT STEPS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Run `parallel-orchestrate setup` to create worktrees for:
- task3
- task4

Then start worker sessions for the next wave.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Notes

**Critical Implementation Details:**

1. **Orchestrator merges into its current branch**
   - Worker sessions work on separate branches
   - Orchestrator integrates by merging branches
   - All work must be committed before merge

2. **Spec files are typically gitignored**
   - Must use `git add -f` to force-add specs
   - This ensures specs are available in worktree sessions

3. **Test after each merge**
   - Catch integration issues early
   - Stop if tests fail
   - Do not proceed to next merge

4. **Dependency enforcement at two levels**
   - Setup: Only create worktrees for ready tasks
   - Worker: Check dependencies before starting work

5. **Merge in correct dependency order**
   - Critical for correctness
   - Use dependency graph from plan
   - Wave-by-wave execution

## Best Practices

**Task Decomposition:**
- Aim for 2-6 tasks (sweet spot for parallelization)
- Each task should be completable in one session
- Minimize file overlap to reduce merge conflicts
- Be conservative with dependencies

**When to use parallel orchestration:**
- Large features with natural boundaries
- Multiple independent components
- Team has capacity for concurrent sessions
- Integration complexity is manageable

**When NOT to use:**
- Small features (overhead not worth it)
- Heavily interdependent tasks (too many waves)
- Single developer, single session preference
- Tight coupling in codebase area
```

---

## Skill 2: parallel-task-workflow

**Purpose:** Task-type aware router that runs inside worktree sessions. Checks dependencies, detects task type (feature/bugfix/refactor), and routes to the appropriate implementation skill.

**File:** `${TOOL_CONFIG}/skills/parallel-task-workflow/SKILL.md`

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

This skill runs inside each worktree to implement tasks. It:
1. Checks dependencies first (STOP if not merged)
2. Detects the task type (feature/bugfix/refactor)
3. Routes to the appropriate implementation skill

## Input

Expects a task specification at ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md with:
- Goal and description
- Task Type (feature/bugfix/refactor)
- Dependencies section
- Acceptance criteria
- Implementation notes

## Process

### Phase 0: Dependency Check

**CRITICAL:** Verify all dependencies are merged before starting.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-TASK-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 0: Dependency check [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 1: Detect task type
⏸️ Phase 2: Route to implementation skill
⏸️ Phase 3: Verify completion

CURRENT TASK:
Phase 0: Checking dependencies
Status: Verifying all dependencies are merged
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Steps:**

1. **Read task spec and extract dependencies:**
   ```markdown
   ## Dependencies
   **This task depends on:** task1, task2
   ```

2. **For each dependency, check if merged:**
   ```bash
   git branch --merged | grep feat/task1
   git branch --merged | grep feat/task2
   ```

3. **If ANY dependency is NOT merged:**
   - **STOP IMMEDIATELY**
   - Output:
     ```
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
     ⚠️ BLOCKED - DEPENDENCIES NOT MET
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

     This task depends on:
     - task1 (feat/task1) - ✅ MERGED
     - task2 (feat/task2) - ❌ NOT MERGED

     You cannot start this task until ALL dependencies are merged.

     NEXT STEPS:
     1. Exit this session
     2. Complete task2 in its worktree session
     3. Run `parallel-orchestrate review` in main session
     4. Run `parallel-orchestrate merge` to merge task2
     5. Return to this session and run /parallel-task-workflow again

     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
     ```
   - **EXIT** - Do not proceed

4. **If ALL dependencies are merged:**
   - Output:
     ```
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
     ✅ DEPENDENCIES SATISFIED
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

     All dependencies are merged:
     - task1 (feat/task1) - ✅ MERGED
     - task2 (feat/task2) - ✅ MERGED

     Proceeding to Phase 1...

     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
     ```
   - Proceed to Phase 1

**Gate:** All dependencies satisfied OR stop if blocked.

---

### Phase 1: Detect Task Type

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-TASK-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Dependency check
🔄 Phase 1: Detect task type [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Route to implementation skill
⏸️ Phase 3: Verify completion

CURRENT TASK:
Phase 1: Detecting task type
Status: Determining which implementation skill to use
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Read task spec to determine type:**

1. **Look for Task Type field:**
   ```markdown
   ## Task Type
   **Task Type:** feature | bugfix | refactor
   ```

2. **If found:** Use the specified type

3. **If NOT found, infer from content:**
   - Contains "implement", "add feature", "new functionality", "create" → **Feature**
   - Contains "fix bug", "debug", "error", "issue", "broken" → **Bugfix**
   - Contains "refactor", "restructure", "improve modularity", "clean up" → **Refactor**

4. **If still unclear:**
   - Ask user: "Is this a feature, bugfix, or refactor task?"
   - Wait for user response

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 TASK TYPE DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Task Type: FEATURE

Will route to: /feature-impl

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Gate:** Task type determined.

---

### Phase 2: Route to Appropriate Implementation Skill

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-TASK-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Dependency check
✅ Phase 1: Detect task type
🔄 Phase 2: Route to implementation skill [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Verify completion

CURRENT TASK:
Phase 2: Routing to implementation skill
Status: Invoking {skill-name}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Based on task type, invoke the corresponding implementation skill:

#### For Feature Tasks:
```
/feature-impl ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md
```

**feature-impl follows TDD implementation process:**
- Phase 0: Read documentation
- Phase 1: Research existing code
- Phase 2: Design structure
- Phase 3: Write tests first
- Phase 4: Implement feature
- Phase 5: Run tests
- Phase 6: Review modularity
- Phase 7: Update documentation
- Phase 8: Commit all work

#### For Bugfix Tasks:
```
/bugfix-impl ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md
```

**bugfix-impl follows hypothesis-based debugging:**
- Phase 0: Read documentation
- Phase 1: Reproduce bug
- Phase 2: Generate hypotheses (3+)
- Phase 3: Test each hypothesis systematically
- Phase 4: Identify root cause
- Phase 5: Implement minimal fix
- Phase 6: Verify fix with tests
- Phase 7: Update documentation
- Phase 8: Commit all work

#### For Refactor Tasks:
```
/refactor-impl ${SPEC_DIR}/YYYY-MM-DD-{task}-spec.md
```

**refactor-impl follows safe refactoring:**
- Phase 0: Read documentation
- Phase 1: Preserve existing tests
- Phase 2: Refactor incrementally
- Phase 3: Verify tests pass after each change
- Phase 4: Review modularity improvement
- Phase 5: Update documentation
- Phase 6: Commit all work

**Gate:** Implementation skill completes successfully.

---

### Phase 3: Verify Completion

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 PARALLEL-TASK-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Dependency check
✅ Phase 1: Detect task type
✅ Phase 2: Route to implementation skill
🔄 Phase 3: Verify completion [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 3: Verifying completion
Status: Checking that work is committed and ready for merge
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

After implementation skill completes, verify:

1. **All changes committed:**
   ```bash
   git status
   # Should show: "nothing to commit, working tree clean"
   ```

2. **Tests passing:**
   ```bash
   # Run project test suite
   pytest  # or npm test, cargo test, etc.
   ```

3. **Commits on branch:**
   ```bash
   git log --oneline -5
   # Should show implementation commits
   ```

**If verification fails:**
- Report what's missing
- Guide user to complete
- Do not claim completion

**If verification succeeds:**

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TASK COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Task: {task-name}
Type: {feature/bugfix/refactor}
Status: ✅ Complete and committed

VERIFICATION:
✅ All changes committed
✅ Tests passing
✅ Ready for review and merge

BRANCH: {branch-name}
COMMITS: {N} commits on this branch

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT STEPS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This task is ready for the orchestrator to review and merge.

In the MAIN session (not this worktree), run:
1. parallel-orchestrate status    (check all tasks)
2. parallel-orchestrate review    (review this task)
3. parallel-orchestrate merge     (integrate and unblock next tasks)

You can now exit this session or start another task.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Integration

This skill is invoked by users in worktree sessions, not by the orchestrator directly.

**Workflow:**
1. Orchestrator creates worktree and commits spec
2. User starts AI session in worktree
3. User runs `/parallel-task-workflow`
4. Skill checks dependencies, routes to implementation
5. Implementation skill completes and commits
6. User returns to orchestrator session for review/merge

**Critical:** This skill does NOT implement tasks directly - it routes to the correct implementation skill based on task type.

---

## Notes

**Dependency Enforcement:**
- Checks dependencies using `git branch --merged`
- STOPS immediately if any dependency not merged
- Prevents wasted work on blocked tasks

**Task Type Detection:**
- Reads Task Type field from spec if present
- Infers from content if not specified
- Asks user if unclear

**Implementation Skill Routing:**
- Feature tasks → `/feature-impl`
- Bugfix tasks → `/bugfix-impl`
- Refactor tasks → `/refactor-impl`

**Handover to Orchestrator:**
- Worker sessions MUST commit all work
- Orchestrator merges branches
- Uncommitted work will not be merged

## Best Practices

**Always commit work:**
- Implementation skills handle this in final phase
- Verify with `git status` before finishing
- Uncommitted work is lost during merge

**Check dependencies early:**
- Phase 0 prevents wasted effort
- Exit immediately if blocked
- Return after dependencies merge

**Trust the router:**
- Don't manually invoke implementation skills
- Let parallel-task-workflow route correctly
- Ensures dependency checking happens
```

---

## Skill 3: parallel-help

**Purpose:** Quick reference for Git worktree commands and troubleshooting.

**File:** `${TOOL_CONFIG}/skills/parallel-help/SKILL.md`

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
│   └── ${SPEC_DIR}/      # Specifications
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
| Shell functions not found | Install functions from setup-guidelines/worktree-guide.md |
| Git version too old | Upgrade to Git 2.5+ for worktree support |

## Workflow Example

```bash
# 1. Orchestrator creates plan and worktrees
parallel-orchestrate plan ${SPEC_DIR}/my-prd.md
parallel-orchestrate setup

# 2. Start worker sessions (separate terminals)
cd ../Worktrees/task1 && claude  # or your AI tool
cd ../Worktrees/task2 && claude

# 3. In each worker session
/parallel-task-workflow

# 4. Back in main session - check progress
parallel-orchestrate status

# 5. Review and merge when done
parallel-orchestrate review
parallel-orchestrate merge

# 6. Setup next wave (if there are unblocked tasks)
parallel-orchestrate setup
```

## Parallel Workflow Commands

| Command | Where to Run | Purpose |
|---------|-------------|---------|
| `parallel-orchestrate plan <prd>` | Main session | Break PRD into tasks with dependencies |
| `parallel-orchestrate setup` | Main session | Create worktrees for ready tasks |
| `parallel-orchestrate status` | Main session | Check progress, show blocked/ready tasks |
| `parallel-orchestrate review` | Main session | Code review completed branches |
| `parallel-orchestrate merge` | Main session | Integrate in dependency order |
| `parallel-task-workflow` | Worktree session | Check dependencies, route to implementation |
| `parallel-help` | Any session | Show this help |

## Dependency Concepts

**Execution Waves:**
- Wave 1: Tasks with no dependencies (can start immediately)
- Wave 2: Tasks that depend on Wave 1 (start after Wave 1 merges)
- Wave 3: Tasks that depend on Wave 2 (start after Wave 2 merges)
- And so on...

**Task States:**
- **Ready**: Dependencies satisfied, can create worktree
- **In Progress**: Worktree active, commits being made
- **Completed**: Commits done, ready for review
- **Blocked**: Waiting for dependencies to merge

**Dependency Checking:**
- `parallel-orchestrate setup`: Only creates worktrees for ready tasks
- `parallel-task-workflow`: Stops if dependencies not merged
- `parallel-orchestrate merge`: Merges in correct order, unblocks next wave

## Full Documentation

See: setup-guidelines/worktree-guide.md

## Troubleshooting Parallel Workflows

**Problem: "Task is blocked"**
- Check which dependencies are not merged: `parallel-orchestrate status`
- Complete and merge dependencies first
- Then run `parallel-orchestrate setup` again

**Problem: "Spec file not in worktree"**
- Check if spec was force-added: `git log` in worktree
- Spec directory may be gitignored - must use `git add -f`
- Re-run `parallel-orchestrate setup` if spec missing

**Problem: "Merge conflicts"**
- Resolve conflicts manually in main session
- Ensure tests pass after resolution
- Complete merge before continuing

**Problem: "Tests fail after merge"**
- Stop merging immediately
- Review conflicting changes
- Fix tests or revert merge
- Do not proceed until tests pass

**Problem: "Worker session can't find implementation skill"**
- Ensure implementation skills are installed (feature-impl, bugfix-impl, refactor-impl)
- Check ${TOOL_CONFIG}/skills/ directory
- Verify .worktreesync includes ${TOOL_CONFIG}
```

---

## Phase 3: Integration

How the skills work together in the parallel orchestration workflow.

### Workflow Overview

```
Main Session (Orchestrator)                    Worktree Sessions (Workers)
─────────────────────────────                  ────────────────────────────

1. parallel-orchestrate plan <prd>
   └─> Creates parallel plan
       └─> Identifies dependencies
           └─> Creates dependency graph
               └─> Generates task specs

2. parallel-orchestrate setup
   └─> Reads plan
       └─> Creates worktrees for ready tasks
           └─> Commits specs to worktrees
               └─> Outputs session instructions
                                                3. User starts sessions
                                                   cd ../Worktrees/task1 && claude
                                                   cd ../Worktrees/task2 && claude

                                                4. /parallel-task-workflow
                                                   └─> Phase 0: Check dependencies
                                                       └─> Phase 1: Detect task type
                                                           └─> Phase 2: Route to impl
                                                               ├─> /feature-impl
                                                               ├─> /bugfix-impl
                                                               └─> /refactor-impl
                                                                   └─> Implementation
                                                                       └─> Tests
                                                                           └─> Commit

5. parallel-orchestrate status
   └─> Shows task progress
       └─> Shows blocked/ready tasks
           └─> Shows dependency graph

6. parallel-orchestrate review
   └─> Reviews completed branches
       └─> Checks conventions
           └─> Validates acceptance criteria
               └─> Approves or requests changes

7. parallel-orchestrate merge
   └─> Merges in dependency order
       └─> Tests after each merge
           └─> Identifies unblocked tasks
               └─> Cleans up worktrees

8. parallel-orchestrate setup (next wave)
   └─> Creates worktrees for newly unblocked tasks
       └─> Repeat cycle for next execution wave
```

### Dependency Flow Example

```
Plan creates:
  Wave 1 (parallel): task1, task2 (no dependencies)
  Wave 2 (after Wave 1): task3 (needs task1), task4 (needs task2)
  Wave 3 (after Wave 2): task5 (needs task3 AND task4)

Execution:
1. setup creates worktrees for task1, task2 (Wave 1 - ready)
2. Workers implement task1, task2 in parallel
3. status shows: task1, task2 in progress; task3, task4, task5 blocked
4. review validates task1, task2
5. merge integrates task1, task2 → unblocks task3, task4
6. status shows: task3, task4 ready; task5 still blocked
7. setup creates worktrees for task3, task4 (Wave 2 - now ready)
8. Workers implement task3, task4 in parallel
9. review validates task3, task4
10. merge integrates task3, task4 → unblocks task5
11. status shows: task5 ready
12. setup creates worktree for task5 (Wave 3 - now ready)
13. Worker implements task5
14. review validates task5
15. merge integrates task5 → complete
```

### File Locations

```
project/
├── ${SPEC_DIR}/
│   ├── YYYY-MM-DD-parallel-plan.md    # Created by: plan
│   ├── YYYY-MM-DD-task1-spec.md       # Created by: plan, copied by: setup
│   ├── YYYY-MM-DD-task2-spec.md       # Created by: plan, copied by: setup
│   └── ...
├── .worktreesync                      # Lists files to sync
└── .gitignore                         # Typically ignores ${SPEC_DIR}/*

../Worktrees/
├── task1/
│   └── ${SPEC_DIR}/
│       └── YYYY-MM-DD-task1-spec.md   # Committed by: setup (force-add)
└── task2/
    └── ${SPEC_DIR}/
        └── YYYY-MM-DD-task2-spec.md   # Committed by: setup (force-add)
```

### Communication Between Sessions

**Orchestrator → Workers:**
- Creates worktrees with specs committed
- Spec contains: dependencies, task type, acceptance criteria

**Workers → Orchestrator:**
- Commits implementation to branch
- Orchestrator reads commits via `git diff` and `git log`

**Critical Handover:**
- Workers MUST commit all work
- Orchestrator merges branches
- Uncommitted work is lost

### Coordination Strategy

**Orchestrator responsibilities:**
- Planning and task decomposition
- Dependency tracking
- Worktree lifecycle (create, clean up)
- Code review
- Integration (merging)
- Unblocking tasks

**Worker responsibilities:**
- Dependency checking (Phase 0)
- Task implementation
- Testing
- Committing work
- Signaling completion

**User responsibilities:**
- Starting worker sessions
- Monitoring progress
- Running orchestrator commands
- Resolving merge conflicts (if any)

---

## Phase 4: Testing & Validation

Test the complete orchestration workflow.

### Test 1: Simple Plan (No Dependencies)

Create a simple PRD with 2 independent tasks to verify basic workflow:

**Create test PRD:**
```markdown
# Test PRD: Simple Independent Tasks

## Goal
Test parallel orchestration with two independent tasks.

## Requirements
1. Create a string formatter utility
2. Create a date formatter utility

These are independent and can be developed in parallel.
```

**Save as:** `${SPEC_DIR}/test-simple-prd.md`

**Test steps:**
```bash
# 1. Plan
parallel-orchestrate plan ${SPEC_DIR}/test-simple-prd.md

# Expected output:
# - Plan file created
# - 2 tasks identified
# - No dependencies
# - 1 execution wave

# 2. Setup (should create 2 worktrees)
parallel-orchestrate setup

# Expected output:
# - 2 worktrees created: ../Worktrees/task1, ../Worktrees/task2
# - Specs committed to both worktrees
# - Instructions for starting sessions

# 3. Verify worktrees created
ls ../Worktrees/
git worktree list

# Expected:
# - task1 and task2 directories exist
# - Both listed in git worktree list

# 4. Check status
parallel-orchestrate status

# Expected output:
# - 2 tasks "ready to start" or "in progress"
# - 0 blocked tasks
# - No dependency arrows

# 5. Simulate work (create dummy commits in each worktree)
cd ../Worktrees/task1
echo "# Task 1 implementation" > implementation.txt
git add implementation.txt
git commit -m "feat: implement task1"
cd <main-repo>

cd ../Worktrees/task2
echo "# Task 2 implementation" > implementation.txt
git add implementation.txt
git commit -m "feat: implement task2"
cd <main-repo>

# 6. Test review
parallel-orchestrate review

# Expected output:
# - Reviews both tasks
# - Shows review checklist for each
# - Approval status

# 7. Test merge
parallel-orchestrate merge

# Expected output:
# - Merges task1
# - Tests pass
# - Merges task2
# - Tests pass
# - Worktrees cleaned up
# - No tasks unblocked (all were ready from start)
```

**Success criteria:**
- Plan creates 2 tasks with no dependencies
- Setup creates 2 worktrees immediately
- Status shows both as ready/in-progress
- Merge integrates both successfully
- Tests pass after merge

### Test 2: Plan with Dependencies

Create a PRD with dependent tasks to verify dependency management:

**Create test PRD:**
```markdown
# Test PRD: Tasks with Dependencies

## Goal
Test parallel orchestration with task dependencies.

## Requirements
1. Build data model (no dependencies)
2. Build API using data model (depends on 1)
3. Build UI using API (depends on 2)

These must be completed in order due to dependencies.
```

**Save as:** `${SPEC_DIR}/test-deps-prd.md`

**Test steps:**
```bash
# 1. Plan (should identify dependencies)
parallel-orchestrate plan ${SPEC_DIR}/test-deps-prd.md

# Expected output:
# - 3 tasks identified
# - Dependencies: task2 needs task1, task3 needs task2
# - 3 execution waves

# 2. Setup (should only create worktree for task1)
parallel-orchestrate setup

# Expected output:
# - 1 worktree created: ../Worktrees/task1
# - task2, task3 NOT created (blocked)

# Verify only task1 worktree created
ls ../Worktrees/
# Expected: only task1 directory

# 3. Status
parallel-orchestrate status

# Expected output:
# - task1: ready to start
# - task2: blocked by task1
# - task3: blocked by task2

# 4. Complete task1 (simulate)
cd ../Worktrees/task1
echo "# Task 1 implementation" > implementation.txt
git add implementation.txt
git commit -m "feat: implement data model"
cd <main-repo>

# 5. Merge task1
parallel-orchestrate merge

# Expected output:
# - Merges task1
# - Tests pass
# - Unblocks task2
# - task3 still blocked

# 6. Setup again (should create worktree for task2)
parallel-orchestrate setup

# Expected output:
# - 1 worktree created: ../Worktrees/task2
# - task3 NOT created (still blocked)

# Verify task2 worktree now created
ls ../Worktrees/
# Expected: task2 directory (task1 cleaned up after merge)

# 7. Complete task2
cd ../Worktrees/task2
echo "# Task 2 implementation" > implementation.txt
git add implementation.txt
git commit -m "feat: implement API"
cd <main-repo>

# 8. Merge task2
parallel-orchestrate merge

# Expected output:
# - Merges task2
# - Tests pass
# - Unblocks task3

# 9. Setup again (should create worktree for task3)
parallel-orchestrate setup

# Expected output:
# - 1 worktree created: ../Worktrees/task3

# 10. Complete task3
cd ../Worktrees/task3
echo "# Task 3 implementation" > implementation.txt
git add implementation.txt
git commit -m "feat: implement UI"
cd <main-repo>

# 11. Merge task3
parallel-orchestrate merge

# Expected output:
# - Merges task3
# - Tests pass
# - All tasks complete
```

**Success criteria:**
- Plan identifies 3 execution waves
- Setup only creates worktrees for ready tasks
- Merge unblocks next wave correctly
- Final status shows all tasks completed
- Dependencies respected throughout

### Test 3: Dependency Checking

Test that parallel-task-workflow blocks when dependencies not met:

**Test steps:**
```bash
# Using test from Test 2, after task2 worktree created but BEFORE task1 merged:

# Manually create worktree for task3 (should be blocked)
git-worktree-add task3 feat/task3
cp ${SPEC_DIR}/YYYY-MM-DD-task3-spec.md ../Worktrees/task3/${SPEC_DIR}/
cd ../Worktrees/task3
git add -f ${SPEC_DIR}/YYYY-MM-DD-task3-spec.md
git commit -m "spec: add task3 specification"

# Start AI session and run parallel-task-workflow
# (In actual test, you'd start claude/AI tool and run the skill)

# Simulate skill's dependency check:
cd ../Worktrees/task3

# Check dependencies (task2 should NOT be merged yet)
git branch --merged | grep feat/task2
# Expected: no output (not merged)

# The skill should output:
# ⚠️ BLOCKED: This task depends on task2 which is not merged yet
# And EXIT without proceeding to implementation
```

**Success criteria:**
- parallel-task-workflow detects unmerged dependency
- Stops immediately with clear error message
- Does not proceed to implementation
- Instructs user on what to do next

### Test 4: Full Workflow

Test the complete orchestration lifecycle with realistic PRD:

**Create realistic PRD:**
```markdown
# Test PRD: User Authentication Feature

## Goal
Implement complete user authentication system.

## Requirements
1. Database schema for users (no dependencies)
2. Password hashing utilities (no dependencies)
3. User registration API (depends on 1, 2)
4. User login API (depends on 1, 2)
5. Authentication middleware (depends on 3, 4)
```

**Test steps:**
```bash
# 1. Create plan
parallel-orchestrate plan ${SPEC_DIR}/test-auth-prd.md

# Expected:
# - 5 tasks
# - Wave 1: task1 (schema), task2 (password utils)
# - Wave 2: task3 (registration), task4 (login)
# - Wave 3: task5 (middleware)

# 2. Setup Wave 1
parallel-orchestrate setup

# Expected: 2 worktrees (task1, task2)

# 3. Start worker sessions (separate terminals)
# Terminal 1: cd ../Worktrees/task1 && claude
# Terminal 2: cd ../Worktrees/task2 && claude

# 4. In each worker session, run:
# /parallel-task-workflow

# Expected:
# - Dependency check passes (no dependencies)
# - Task type detected (both "feature")
# - Routes to /feature-impl
# - Implementation completes
# - Work committed

# 5. Check status
parallel-orchestrate status

# Expected:
# - task1, task2: in progress or completed
# - task3, task4: blocked
# - task5: blocked

# 6. Review Wave 1
parallel-orchestrate review

# Expected:
# - Reviews task1, task2
# - Shows approval status

# 7. Merge Wave 1
parallel-orchestrate merge

# Expected:
# - Merges task1, task2
# - Tests pass
# - Unblocks task3, task4

# 8. Setup Wave 2
parallel-orchestrate setup

# Expected: 2 worktrees (task3, task4)

# 9. Implement Wave 2 (same as steps 3-4)

# 10. Review and merge Wave 2
parallel-orchestrate review
parallel-orchestrate merge

# Expected:
# - Merges task3, task4
# - Tests pass
# - Unblocks task5

# 11. Setup Wave 3
parallel-orchestrate setup

# Expected: 1 worktree (task5)

# 12. Implement Wave 3

# 13. Final review and merge
parallel-orchestrate review
parallel-orchestrate merge

# Expected:
# - Merges task5
# - Tests pass
# - All tasks complete
# - All worktrees cleaned up

# 14. Verify final state
git log --oneline -20
git worktree list

# Expected:
# - All feature commits in history
# - No worktrees remaining (all cleaned up)
# - Tests passing
```

**Success criteria:**
- All tasks completed in correct dependency order
- No task started before dependencies merged
- All tests passing after each merge
- No lost work
- Clean git history
- All worktrees cleaned up

---

## Phase 5: Best Practices

### Task Decomposition

**Good task boundaries:**
- Each task completable in one session (1-3 hours)
- Minimal file overlap between tasks
- Clear interfaces between tasks
- 2-6 tasks total (sweet spot for parallelization)

**Bad task boundaries:**
- Tasks too small (overhead exceeds benefit)
- Tasks too large (can't complete in one session)
- Heavy file overlap (merge conflicts likely)
- Too many tasks (coordination overhead)

**Dependency guidelines:**
- Be conservative: if unsure, mark as dependent
- Prefer fewer waves with more parallel tasks
- Avoid circular dependencies
- Consider data flow (models → APIs → UI)

### When to Use Parallel Orchestration

**Use when:**
- Large features with natural boundaries
- Multiple independent components
- Team capacity for concurrent sessions
- Integration complexity is manageable
- 2-6 tasks identified

**Don't use when:**
- Small features (< 2 tasks)
- Heavily interdependent tasks (> 5 waves)
- Single developer preference for sequential
- Tight coupling in codebase area
- Rapid iteration needed

### Git Worktree Best Practices

**Directory organization:**
- Use `-base` parent folder structure
- Keep ../Worktrees consistent across projects
- Document structure in project README

**Environment sync:**
- Maintain .worktreesync file
- Include .env files
- Include tool config directories
- Test sync by creating test worktree

**Cleanup:**
- Remove worktrees after merge
- Prune worktree metadata regularly
- Delete merged branches (optional)

**Reference:** See setup-guidelines/worktree-guide.md for complete guide

### Dependency Management

**Planning phase:**
- Identify all dependencies upfront
- Create dependency graph
- Validate no circular dependencies
- Estimate execution waves

**Setup phase:**
- Only create worktrees for ready tasks
- Check dependencies before creating
- Document why tasks are blocked

**Worker phase:**
- Always check dependencies (Phase 0)
- Stop immediately if blocked
- Don't waste effort on blocked tasks

**Merge phase:**
- Merge in dependency order (critical!)
- Test after each merge
- Identify unblocked tasks
- Communicate what's ready next

### Coordination Strategies

**Communication:**
- Use parallel-orchestrate status regularly
- Monitor progress from main session
- Workers signal completion by committing
- Orchestrator signals ready by creating worktrees

**Conflict resolution:**
- Minimize by good task boundaries
- Resolve quickly when they occur
- Test after resolution
- Don't proceed until tests pass

**Session management:**
- Use terminal titles to identify sessions
- Keep orchestrator in main repo
- Workers in worktrees
- Don't mix responsibilities

### Common Pitfalls

**Skipping dependency checks:**
- Always run Phase 0 in parallel-task-workflow
- Don't manually invoke implementation skills
- Trust the router

**Merging out of order:**
- Always follow dependency graph
- Don't merge blocked tasks early
- Test after each merge

**Forgetting to commit:**
- Implementation skills should commit in final phase
- Verify with git status
- Uncommitted work is lost

**Creating too many tasks:**
- Aim for 2-6 tasks
- Broader tasks better than tiny tasks
- Consider coordination overhead

**Ignoring test failures:**
- Stop merging if tests fail
- Fix or revert
- Never proceed with failing tests

---

## Phase 6: Troubleshooting

### Issue: Worktrees Not Created

**Symptoms:**
- `parallel-orchestrate setup` doesn't create worktrees
- "No ready tasks" message

**Diagnosis:**
```bash
# Check if ../Worktrees exists
ls ../Worktrees

# Check shell functions
type git-worktree-add

# Check plan file
cat ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md

# Check dependency status
parallel-orchestrate status
```

**Solutions:**
- Create ../Worktrees directory: `mkdir ../Worktrees`
- Install shell functions (see Prerequisites)
- Verify dependencies are satisfied in plan
- Check that tasks aren't already merged

### Issue: Specs Not in Worktree

**Symptoms:**
- Worktree created but spec file missing
- parallel-task-workflow can't find spec

**Diagnosis:**
```bash
# In worktree
cd ../Worktrees/task1
ls ${SPEC_DIR}/

# Check git log
git log --oneline -1
```

**Solutions:**
- Spec directory likely gitignored
- Re-run setup (it force-adds specs)
- Manually force-add:
  ```bash
  cd ../Worktrees/task1
  git add -f ${SPEC_DIR}/*.md
  git commit -m "spec: add task specification"
  ```

### Issue: Dependency Check Fails

**Symptoms:**
- parallel-task-workflow says "BLOCKED"
- Dependencies appear merged but skill disagrees

**Diagnosis:**
```bash
# Check what's merged
git branch --merged

# Check specific dependency
git branch --merged | grep feat/dependency-task
```

**Solutions:**
- Ensure dependency actually merged to current branch
- Check you're on correct branch in main repo
- Merge dependency first
- Re-run parallel-task-workflow after merge

### Issue: Merge Conflicts

**Symptoms:**
- `parallel-orchestrate merge` reports conflicts
- Files marked as conflicted

**Diagnosis:**
```bash
# Check conflict status
git status

# See conflicting files
git diff --name-only --diff-filter=U
```

**Solutions:**
- Review conflicting files
- Resolve conflicts manually
- Stage resolved files: `git add <files>`
- Complete merge: `git commit`
- Verify tests pass
- Continue with remaining merges

### Issue: Tests Fail After Merge

**Symptoms:**
- Merge succeeds but tests fail
- Integration issues

**Diagnosis:**
```bash
# Run tests
pytest  # or your test command

# Check what was merged
git log --oneline -5

# See changes
git diff HEAD~1
```

**Solutions:**
- Review test failures
- Check integration between merged tasks
- Fix failing tests
- OR revert merge: `git reset --hard HEAD~1`
- Do not proceed until tests pass

### Issue: Worker Session Can't Find Implementation Skill

**Symptoms:**
- parallel-task-workflow routes but skill not found
- "Skill not found" error

**Diagnosis:**
```bash
# Check if skills installed
ls ${TOOL_CONFIG}/skills/feature-impl
ls ${TOOL_CONFIG}/skills/bugfix-impl
ls ${TOOL_CONFIG}/skills/refactor-impl

# Check .worktreesync
cat .worktreesync
```

**Solutions:**
- Install missing implementation skills
- Add ${TOOL_CONFIG} to .worktreesync
- Recreate worktree (new ones will sync)
- Manually copy ${TOOL_CONFIG} to worktree

### Issue: Circular Dependencies

**Symptoms:**
- No tasks marked as "ready" in plan
- All tasks blocked

**Diagnosis:**
```bash
# Review plan file
cat ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md

# Look for circular dependencies:
# task1 depends on task2
# task2 depends on task1
```

**Solutions:**
- Redesign task boundaries
- Break circular dependency
- Make one task independent
- Re-run plan command

---

## Summary

**This suite provides:**
- Parallel task execution with Git worktrees
- Dependency tracking and enforcement
- Task state management (ready/blocked/in-progress/completed)
- Code review before integration
- Safe merging in correct order
- Automatic routing to implementation skills

**Key skills:**
- `parallel-orchestrate` - The orchestrator (plan, setup, status, review, merge)
- `parallel-task-workflow` - Worker router (dependency check, task type detection, routing)
- `parallel-help` - Quick reference

**Prerequisites met:**
- Git 2.5+ with worktree support
- Shell functions installed (git-worktree-add, git-worktree-remove)
- Directory structure configured (../Worktrees)
- .worktreesync file created
- Implementation skills installed (feature-impl, bugfix-impl, refactor-impl)

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths (${SPEC_DIR}, ${TOOL_CONFIG})
3. Test with simple PRD (Test 1)
4. Test with dependencies (Test 2)
5. Use for parallel development!

---

**Last Updated:** 2026-02-14
**Related Documents:**
- setup-guidelines/repo-detection.md
- setup-guidelines/prerequisites.md
- setup-guidelines/worktree-guide.md
- setup-guidelines/core-conventions.md
- skill-guidelines/feature-development.md
- skill-guidelines/bugfixing.md
- skill-guidelines/refactoring.md
