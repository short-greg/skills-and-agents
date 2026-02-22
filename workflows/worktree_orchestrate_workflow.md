---
name: worktree-orchestrate-workflow
description: >
  Use when breaking large features into parallelizable tasks across git worktrees.
  Triggers on: "parallelize this work", "break into tasks", "coordinate parallel development".
argument-hint: "[PRD or plan to decompose into parallel tasks]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Worktree Orchestrate Workflow

**Goal:** Break a large plan or PRD into independent parallelizable tasks, manage dependencies, and coordinate safe integration.

**Intent:** Enable faster development of large features by identifying natural task boundaries, tracking dependencies between tasks, setting up isolated worktrees for parallel execution, and merging completed work in correct dependency order. Prevent merge conflicts, duplicated effort, and incorrect sequencing.

**Scope:** Task decomposition and coordination. Includes: analyzing plan/PRD for task boundaries, creating dependency graphs, generating task specs, setting up worktrees for ready tasks, tracking task status, and merging completed work. Does NOT include executing the tasks themselves (that's worktree_task_workflow).

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (imperative setup, declarative dependency management)

**Uncertainty Handling:**
- If task boundaries are unclear, invoke `brainstorm` to explore decomposition options
- If dependencies are complex, create explicit dependency graph before proceeding
- If task granularity is wrong (too large or too small), re-decompose with user input
- If merge conflicts arise, stop and assess before continuing

**Assumptions:**
- PRD or plan exists and is readable
- Git worktrees are supported (Git 2.5+)
- Tasks can be meaningfully parallelized (not all tightly coupled)

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand the PRD/plan, project structure, and identify natural parallelization opportunities

**Inputs:** PRD file or plan from user

**Outputs:**
- PRD/plan understood (requirements, scope, deliverables)
- Project structure identified (what areas will be affected)
- Existing code patterns identified (how features are typically structured)
- Potential task boundaries identified (components, layers, features)

**Self-satisfiable:** Reads PRD, project documentation, and relevant code

### Step 2: Brainstorm Task Decomposition

**Primitive:** brainstorm

**Purpose:** Identify natural task boundaries and explore decomposition options

**Inputs:**
- PRD understanding from Step 1
- Project structure from Step 1

**Outputs:**
- Candidate task boundaries (multiple options)
- Trade-offs between decomposition approaches
- Dependency considerations for each approach
- Recommended decomposition (2-6 tasks typically optimal)

**Decomposition strategies:**
- By component (frontend, backend, database)
- By feature (user stories, use cases)
- By layer (API, service, data access)
- By area of code (modules, packages)

**Key principle:** Tasks should be independent enough that merge conflicts are unlikely.

### Step 3: Define Task Specs

**Primitive:** define

**Purpose:** Create detailed task specifications with clear boundaries and dependencies

**Inputs:**
- Recommended decomposition from Step 2
- PRD from Step 1

**Outputs:**
- Task spec files (one per task) saved to `${SPEC_DIR}/parallel/`
- Dependency graph (which tasks depend on which)
- Execution waves (tasks grouped by dependency level)

**Task spec format:**
```markdown
# Task: [Task Name]

## Goal
[What this task should accomplish - one sentence]

## Objective
[Broader objective this task contributes to]

## Key Results
1. [Measurable outcome 1]
2. [Measurable outcome 2]

## Background
[Context, constraints, related information]

## Dependencies
- [List of task names this depends on, or "None"]

## Files Likely Affected
- [List of files/directories this task will modify]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Additional Context (optional)
- User stories
- Technical notes
- Edge cases to handle
```

**Dependency graph format:**
```
Wave 1 (no dependencies): Task A, Task B
Wave 2 (depends on Wave 1): Task C (depends on A), Task D (depends on B)
Wave 3 (depends on Wave 2): Task E (depends on C, D)
```

### Step 4: Validate Task Specs

**Primitive:** validate

**Purpose:** Verify task specs are complete, dependencies are accurate, and tasks are truly independent

**Inputs:**
- Task specs from Step 3
- Dependency graph from Step 3
- PRD from Step 1

**Outputs:**
- Pass/fail verdict
- Evidence (which specs pass/fail validation)
- Issues identified (overlapping scope, missing dependencies, unclear boundaries)

**Validation criteria:**
1. Each task has clear Goal and Key Results
2. Dependencies are correctly identified (no circular dependencies)
3. Tasks within same wave don't modify same files
4. All PRD requirements are covered by at least one task
5. Task granularity is appropriate (2-6 tasks)

**On failure:**
1. Identify which tasks need refinement
2. Loop back to Step 3 with specific fixes
3. Update checklist with remediation items

**Gate:** User confirms task decomposition before worktree setup

### Step 5: Setup Worktrees

**Primitive:** implement (setup action)

**Purpose:** Create git worktrees for tasks with satisfied dependencies

**Inputs:**
- Validated task specs from Steps 3-4
- Dependency graph from Step 3

**Outputs:**
- Worktrees created for Wave 1 tasks (those with no dependencies)
- Task specs copied to worktrees
- Branch naming: `parallel/{task-name}`
- Worktree location: `../{repo-name}-worktrees/{task-name}/`

**Setup steps:**
1. Identify ready tasks (dependencies satisfied or no dependencies)
2. For each ready task:
   - Create worktree: `git worktree add ../{repo-name}-worktrees/{task-name} -b parallel/{task-name}`
   - Copy task spec to worktree root
   - Record worktree location in parallel plan
3. Output session instructions (how to start work in each worktree)

**Worktree commands reference:**
```bash
# Create worktree
git worktree add <path> -b <branch-name>

# List worktrees
git worktree list

# Remove worktree (after merge)
git worktree remove <path>
```

### Step 6: Track Status

**Primitive:** validate (status check)

**Purpose:** Monitor progress across all tasks and identify blockers

**Inputs:**
- Parallel plan from Steps 3-5
- Worktree states

**Outputs:**
- Status report per task (not started, in progress, blocked, ready for merge, merged)
- Blockers identified
- Newly unblocked tasks (dependencies now merged)

**Status checking:**
1. Read parallel plan
2. For each task, check:
   - Worktree exists? → In progress or ready
   - All dependencies merged? → Ready to start
   - Branch has commits? → Work in progress
   - Task marked complete? → Ready for merge
3. Report status and next actions

### Step 7: Merge Completed Tasks

**Primitive:** implement (merge action)

**Purpose:** Integrate completed work in correct dependency order

**Inputs:**
- Completed tasks (ready for merge)
- Dependency graph from Step 3

**Outputs:**
- Merged branches
- Test results after each merge
- Cleaned up worktrees
- Updated parallel plan

**Merge process:**
1. Create merge sequence respecting dependencies
2. For each task in sequence:
   - Merge branch to main: `git merge parallel/{task-name}`
   - Run tests: verify no regressions
   - If tests fail: STOP, report issue, do not continue
   - If tests pass: continue to next task
3. Remove merged worktrees: `git worktree remove <path>`
4. Delete merged branches: `git branch -d parallel/{task-name}`
5. Identify newly unblocked tasks
6. Update parallel plan with merge status

**On merge conflict:**
1. Stop immediately
2. Report which tasks conflict
3. User must resolve manually or adjust decomposition
4. Do not auto-resolve

**On test failure:**
1. Stop immediately
2. Report which tests fail
3. User must fix in worktree before re-attempting merge
4. Do not continue merging other tasks

### Step 8: Setup Next Wave

**Primitive:** implement (setup action)

**Purpose:** Create worktrees for newly unblocked tasks after merges

**Inputs:**
- Updated dependency graph (post-merge)
- Task specs for unblocked tasks

**Outputs:**
- New worktrees created
- Session instructions for new tasks

**Process:**
1. Identify tasks whose dependencies are now all merged
2. Create worktrees for these tasks (same as Step 5)
3. Update parallel plan
4. Output instructions

**Repeat:** Steps 6-8 until all tasks are merged

---

## Preconditions

**Must be provided:**
- PRD or plan: what to decompose into parallel tasks

**Self-satisfiable:**
- project structure: read project to understand architecture
- git setup: verify worktrees are supported

**Prerequisites:**
- Git 2.5+ (worktree support)
- Clean working directory (no uncommitted changes)
- Main branch is stable (tests pass)

---

## Postconditions

**Success:**
- All tasks defined and validated
- Worktrees created for ready tasks
- Parallel plan documented at `${SPEC_DIR}/parallel/{plan-name}.md`
- All completed tasks merged in correct order
- All tests pass after final merge

**Failure (blocked):**
- PRD cannot be decomposed into independent tasks (blocked at Step 2)
- Dependencies are circular (blocked at Step 4)
- Merge conflicts cannot be resolved (blocked at Step 7)
- Tests fail after merge (blocked at Step 7)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing parallel plan at `${SPEC_DIR}/parallel/`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify current wave and task status
   - Check which worktrees exist
   - Resume from appropriate point
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1-4 (Planning):** If partial, read existing task specs and continue
- **Step 5 (Setup):** Check which worktrees exist, create missing ones
- **Step 6 (Status):** Safe to re-run; read current state
- **Step 7 (Merge):** Check which branches are merged; continue from last successful merge
- **Step 8 (Next Wave):** Check which tasks are unblocked; create missing worktrees

**Critical:** If interrupted mid-merge, verify test status before continuing.

---

## On Validation Failure

**Validation failure indicates task specs or dependencies are incorrect:**

1. **Validate reports failure** — which criteria failed
2. **Analyze root cause:**
   - Tasks overlap in scope → revise task boundaries
   - Missing dependencies → add dependencies
   - Circular dependencies → restructure tasks
3. **Loop back to Step 3** with specific fixes
4. **Re-validate** before proceeding

**Iteration bounds:** Max 2 decomposition iterations before escalating to user

---

## Quality Gates

| Step | Gate | Purpose |
|------|------|---------|
| Step 4 | Validate task specs | Verify decomposition before setup |
| Step 7 | Test after merge | Verify integration doesn't break tests |

**User approval gates:**
- After Step 4: User confirms task decomposition
- After each merge: User can review before continuing (optional)

---

## Customization Points

When adapting this workflow for your project:

### Repository Type

**Prototype:**
- Simplified task specs (Goal + Acceptance Criteria only)
- Fewer dependency checks
- Optional code review before merge

**Production:**
- Full task specs with detailed context
- Strict dependency enforcement
- Required code review before merge
- Tests must pass after each merge

**Library:**
- Task specs include API compatibility notes
- Extra validation for public API changes
- Changelog updates tracked per task

### Worktree Configuration
- Worktree directory location (default: `../{repo-name}-worktrees/`)
- Branch naming convention (default: `parallel/{task-name}`)

### Testing Requirements
- Test command to run after merge
- Required coverage or checks

### Merge Strategy
- Fast-forward vs. merge commits
- Squash merging preference

---

## Example Usage

**User:** "Parallelize the implementation of this user management PRD"

**Workflow execution:**

1. **Orient** — Read PRD, identify it covers: user registration, user profiles, admin panel, reporting
2. **Brainstorm** — Identify task boundaries:
   - Option A: By feature (registration, profiles, admin, reporting)
   - Option B: By layer (API, frontend, database)
   - Recommend: Option A (features are independent)
3. **Define task specs:**
   - Task: user-registration (Wave 1, no deps)
   - Task: user-profiles (Wave 1, no deps)
   - Task: admin-panel (Wave 2, depends on user-profiles)
   - Task: reporting (Wave 2, depends on user-profiles)
4. **Validate** — Check tasks don't overlap, dependencies correct → PASS
5. **Setup worktrees** — Create worktrees for Wave 1 (registration, profiles)
6. **Track status** — User works on tasks in separate sessions
7. **Merge** — User completes tasks:
   - Merge user-registration → tests pass
   - Merge user-profiles → tests pass
8. **Setup next wave** — Create worktrees for Wave 2 (admin, reporting)
9. **Continue** until all merged

**Outcome:** Large PRD completed faster through parallel execution.

---

## Best Practices

- **Right-size tasks** — 2-6 tasks is optimal; too many creates coordination overhead
- **Minimize overlap** — Tasks should touch different files when possible
- **Test after each merge** — Catch integration issues immediately
- **Merge in dependency order** — Never merge a task before its dependencies
- **Clean up worktrees** — Remove after merge to avoid confusion
- **Document the plan** — Parallel plan file enables recovery and visibility

---

## Common Anti-Patterns

**Anti-Pattern 1: Too many tasks**
- ❌ "I'll create 20 tiny tasks for maximum parallelism"
- ✅ 2-6 tasks balances parallelism with coordination overhead

**Anti-Pattern 2: Overlapping scope**
- ❌ "Task A and Task B both modify the User model"
- ✅ Either combine tasks or clearly delineate who modifies what

**Anti-Pattern 3: Ignoring dependencies**
- ❌ "I'll merge in whatever order tasks complete"
- ✅ Merge in dependency order; Wave 2 after Wave 1

**Anti-Pattern 4: Skipping tests**
- ❌ "I'll run tests once at the end after all merges"
- ✅ Test after each merge to isolate issues

**Anti-Pattern 5: Leaving worktrees**
- ❌ "I'll clean up worktrees later"
- ✅ Remove worktrees immediately after merge

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [project_quality.md](../protocols/project_quality.md) — Quality assessment for merges

---

## Related Primitives

This workflow composes these primitives:

1. [orient](../primitives/orient.md) — Understand PRD and project
2. [brainstorm](../primitives/brainstorm.md) — Explore decomposition options
3. [define](../primitives/define.md) — Create task specifications
4. [validate](../primitives/validate.md) — Verify task specs and merge results
5. [implement](../primitives/implement.md) — Setup worktrees and execute merges

---

## Related Workflows

- [worktree_task_workflow](worktree_task_workflow.md) — Execute individual tasks in worktrees (used by this workflow's tasks)
- [feature_workflow](feature_workflow.md) — May be invoked by worktree_task_workflow for feature-type tasks
- [bugfix_workflow](bugfix_workflow.md) — May be invoked by worktree_task_workflow for bugfix-type tasks
