---
name: worktree-task-workflow
description: >
  Use when executing a task in a git worktree based on a task spec.
  Triggers on: "execute this task", "work on this worktree task", "implement task spec".
argument-hint: "[task spec file or task description]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Worktree Task Workflow

**Goal:** Execute a single task in a git worktree based on a task specification, routing to the appropriate workflow.

**Intent:** Enable reliable task execution in parallel worktree environments by reading the task spec, verifying dependencies are satisfied, determining the appropriate workflow to use (feature, bugfix, refactor, etc.), and executing the task to completion. Signal readiness for merge when complete.

**Scope:** Single task execution within a worktree. Includes: reading task spec, checking dependencies, determining task type, invoking appropriate workflow, and signaling completion. Does NOT include: task decomposition, worktree setup, or merge coordination (those are worktree_orchestrate_workflow).

---

## Workflow Type

**Type:** Adaptive

**Style:** Declarative (routes to other workflows based on task type)

**Uncertainty Handling:**
- If task type is unclear, analyze task spec to determine best workflow match
- If no workflow matches, ask user for guidance
- If dependencies aren't merged, block immediately with clear error

**Routing logic:**
- Task involves new functionality → feature_workflow
- Task involves fixing a bug → bugfix_workflow
- Task involves code improvement without behavior change → refactor_workflow
- Task involves documentation → documentation workflow (if exists)
- Task involves testing → testing workflow (if exists)

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Read task spec and understand what needs to be accomplished

**Inputs:** Task spec file (typically at worktree root or `${SPEC_DIR}/parallel/`)

**Outputs:**
- Task goal understood
- Key results identified
- Dependencies identified
- Acceptance criteria understood
- Files likely to be affected identified

**Self-satisfiable:** Reads task spec file from worktree

**Task spec expected format:**
```markdown
# Task: [Task Name]

## Goal
[What this task should accomplish]

## Objective
[Broader objective]

## Key Results
1. [Measurable outcome 1]
2. [Measurable outcome 2]

## Background
[Context]

## Dependencies
- [List of task names this depends on]

## Files Likely Affected
- [List]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
```

### Step 2: Validate Dependencies

**Primitive:** validate

**Purpose:** Verify all dependencies are merged before starting work

**Inputs:**
- Dependencies list from Step 1
- Main branch state

**Outputs:**
- Pass/fail verdict
- List of merged dependencies
- List of unmerged dependencies (if any)

**Dependency check process:**
1. Read dependencies from task spec
2. For each dependency:
   - Check if `parallel/{dependency-name}` branch is merged to main
   - OR check if dependency task is marked complete in parallel plan
3. If ANY dependency is not merged: FAIL

**On failure:**
```
BLOCKED: Dependencies not satisfied

Unmerged dependencies:
- task-name-1 (not merged)
- task-name-2 (not merged)

Action: Wait for these tasks to complete and merge before starting.
Do NOT proceed with implementation.
```

**Critical:** This step is a hard gate. Do NOT proceed if dependencies are unmerged. Exit workflow immediately.

### Step 3: Determine Task Type

**Primitive:** orient (classification)

**Purpose:** Analyze task spec to determine which workflow to invoke

**Inputs:**
- Task spec from Step 1
- Available workflows

**Outputs:**
- Task type classification (feature, bugfix, refactor, documentation, testing, other)
- Recommended workflow to invoke
- Confidence level

**Classification heuristics:**

| Task Indicators | Classification | Workflow |
|-----------------|----------------|----------|
| "implement", "add", "create", "build" | Feature | feature_workflow |
| "fix", "bug", "error", "issue", "broken" | Bugfix | bugfix_workflow |
| "refactor", "improve", "clean up", "reorganize" | Refactor | refactor_workflow |
| "document", "README", "docs" | Documentation | (inline or dedicated) |
| "test", "coverage", "spec" | Testing | (inline or dedicated) |

**If unclear:**
1. Check if task spec mentions workflow explicitly
2. Analyze key results for clues
3. If still unclear, ask user: "This task could be handled as [X] or [Y]. Which workflow should I use?"

### Step 4: Invoke Workflow

**Primitive:** implement (delegation)

**Purpose:** Execute the appropriate workflow with task context

**Inputs:**
- Task type from Step 3
- Task spec from Step 1
- Workflow to invoke

**Outputs:**
- Workflow execution (follows that workflow's steps)
- Task implementation complete
- Tests passing

**Invocation process:**

1. **Prepare context** — Extract from task spec:
   - Feature description (Goal + Objective)
   - Success criteria (Key Results + Acceptance Criteria)
   - Scope (Files Likely Affected + Background constraints)

2. **Invoke workflow** — Start the selected workflow:
   - For feature: "Implement [Goal] with success criteria [Key Results]"
   - For bugfix: "Fix [Goal] - expected behavior: [Key Results]"
   - For refactor: "Refactor [Goal] - improvement targets: [Key Results]"

3. **Execute workflow** — Follow all steps of invoked workflow

4. **Track progress** — Maintain trace in worktree's `${TASK_DIR}/`

**Note:** The invoked workflow (feature, bugfix, etc.) runs completely within the worktree context.

### Step 5: Validate Completion

**Primitive:** validate

**Purpose:** Verify task is complete and ready for merge

**Inputs:**
- Completed implementation from Step 4
- Acceptance criteria from Step 1
- Key results from Step 1

**Outputs:**
- Pass/fail verdict
- Evidence for each acceptance criterion
- Test results
- Readiness for merge

**Validation checklist:**
1. All acceptance criteria satisfied
2. All key results achieved
3. Tests pass in worktree
4. All changes committed
5. No uncommitted files
6. Branch is clean and mergeable

**On failure:**
1. Identify which criteria not met
2. Loop back to Step 4 (continue workflow execution)
3. Re-validate

**Iteration bounds:** Max 3 iterations before escalating to user

### Step 6: Signal Completion

**Primitive:** implement (signal action)

**Purpose:** Mark task as ready for merge in parallel plan

**Inputs:**
- Validated completion from Step 5
- Task name

**Outputs:**
- Updated parallel plan (task marked "ready for merge")
- Summary of completed work
- Merge instructions

**Completion signal:**
```markdown
## Task Complete: [Task Name]

**Status:** Ready for merge

**Branch:** parallel/[task-name]

**Summary:**
- [Brief description of what was implemented]

**Acceptance Criteria:**
- [x] Criterion 1 - [evidence]
- [x] Criterion 2 - [evidence]

**Tests:** All passing

**Merge command:**
```bash
cd [main-repo-path]
git merge parallel/[task-name]
```

**Next steps:**
1. Run orchestrate workflow to merge this task
2. After merge, Wave 2 tasks may become unblocked
```

---

## Preconditions

**Must be provided:**
- task spec: task specification file or content

**Self-satisfiable:**
- dependency status: check main branch for merged dependencies
- available workflows: scan for feature, bugfix, refactor workflows
- worktree context: read worktree state

**Prerequisites:**
- Running in a git worktree (not main working directory)
- Task spec exists and is readable
- All dependencies merged (verified in Step 2)

---

## Postconditions

**Success:**
- Task implemented per specification
- All acceptance criteria met
- Tests pass
- Changes committed
- Task marked ready for merge

**Failure (blocked):**
- Dependencies not merged (blocked at Step 2)
- Task type cannot be determined (blocked at Step 3)
- Implementation cannot satisfy criteria (blocked at Step 4/5)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at worktree's `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Check if dependencies are still satisfied
   - Identify last completed step
   - Resume from appropriate point
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1 (Orient):** Safe to re-run; re-read task spec
- **Step 2 (Validate Dependencies):** Re-check; dependencies may have been merged
- **Step 3 (Determine Type):** Use cached classification if available
- **Step 4 (Invoke Workflow):** Invoked workflow has its own recovery; resume there
- **Step 5 (Validate):** Safe to re-run; check current state
- **Step 6 (Signal):** Check if already signaled; update if needed

**Critical:** If resuming, re-verify dependencies first (Step 2).

---

## On Validation Failure

**Validation failure at Step 5 indicates task not complete:**

1. **Validate reports failure** — which criteria not met
2. **Analyze gap:**
   - Missing implementation → continue Step 4 workflow
   - Test failures → fix and re-run tests
   - Uncommitted changes → commit
3. **Loop back** to Step 4 or appropriate sub-step
4. **Re-validate**

**Iteration bounds:** Max 3 iterations before escalating to user

---

## Quality Gates

| Step | Gate | Purpose |
|------|------|---------|
| Step 2 | Dependency check | Block if dependencies not merged |
| Step 5 | Completion validation | Verify ready for merge |

**Hard gate:** Step 2 is non-negotiable. Do NOT proceed with unmerged dependencies.

---

## Customization Points

When adapting this workflow for your project:

### Repository Type

**Prototype:**
- Simplified validation (tests pass, changes committed)
- Task type determination can be more lenient

**Production:**
- Full validation against all acceptance criteria
- Code review may be required before signaling complete
- Strict workflow matching

**Library:**
- API compatibility checks
- Documentation requirements
- Changelog entry verification

### Workflow Discovery
- Location of workflow definitions
- Custom workflows to consider (testing, documentation, etc.)

### Dependency Checking
- How to verify dependencies are merged
- Whether to check parallel plan file or git history

### Completion Signaling
- Where to update task status
- Notification mechanism (file, message, etc.)

---

## Example Usage

**User:** "Execute the user-profiles task in this worktree"

**Workflow execution:**

1. **Orient** — Read task spec:
   - Goal: Implement user profile viewing and editing
   - Dependencies: None (Wave 1 task)
   - Acceptance: Users can view profiles, users can edit their profile
2. **Validate dependencies** — No dependencies → PASS
3. **Determine task type** — "Implement" + "viewing and editing" → Feature
4. **Invoke workflow** — Start feature_workflow:
   - Orient (understand profile patterns)
   - Define (profile view, profile edit)
   - Design (components, API endpoints)
   - Implement (code + tests)
   - Validate (tests pass)
5. **Validate completion:**
   - [x] Users can view profiles
   - [x] Users can edit their profile
   - [x] Tests pass
   - → PASS
6. **Signal completion** — Update parallel plan, output merge instructions

**Outcome:** Task complete, ready for merge by orchestrate workflow.

---

## Example: Blocked by Dependencies

**User:** "Execute the admin-panel task"

**Workflow execution:**

1. **Orient** — Read task spec:
   - Goal: Build admin panel for user management
   - Dependencies: user-profiles (Wave 1)
2. **Validate dependencies:**
   - Check: Is parallel/user-profiles merged? NO
   - → FAIL

**Output:**
```
BLOCKED: Dependencies not satisfied

Unmerged dependencies:
- user-profiles (not merged)

Action: Wait for user-profiles to complete and merge before starting.

To check status:
  cd [main-repo]
  git log --oneline main | grep user-profiles
```

**Outcome:** Workflow exits immediately without implementation.

---

## Best Practices

- **Read the spec carefully** — Task spec is the source of truth
- **Check dependencies first** — Don't start work if blocked
- **Match workflow accurately** — Wrong workflow leads to wrong process
- **Validate thoroughly** — All acceptance criteria must pass
- **Signal clearly** — Other tasks may be waiting on this one
- **Stay in worktree** — Don't modify main branch directly

---

## Common Anti-Patterns

**Anti-Pattern 1: Ignoring dependencies**
- ❌ "Dependencies aren't merged, but I'll start anyway"
- ✅ Stop immediately if dependencies aren't satisfied

**Anti-Pattern 2: Wrong workflow**
- ❌ "This bugfix looks like a feature, so I'll use feature workflow"
- ✅ Match workflow to actual task type; ask if unclear

**Anti-Pattern 3: Incomplete validation**
- ❌ "Tests pass, so I'm done"
- ✅ Verify ALL acceptance criteria, not just tests

**Anti-Pattern 4: Forgetting to signal**
- ❌ "I finished, but didn't update the parallel plan"
- ✅ Always signal completion so orchestrator knows

**Anti-Pattern 5: Working on main**
- ❌ "I'll just make changes in main repo instead of worktree"
- ✅ All changes happen in the worktree; that's the point

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [project_quality.md](../protocols/project_quality.md) — Quality assessment

---

## Related Primitives

This workflow composes these primitives:

1. [orient](../primitives/orient.md) — Read and understand task spec
2. [validate](../primitives/validate.md) — Check dependencies and completion

**Note:** Step 4 delegates to other workflows which compose additional primitives.

---

## Related Workflows

- [worktree_orchestrate_workflow](worktree_orchestrate_workflow.md) — Creates task specs and coordinates merges (parent workflow)
- [feature_workflow](feature_workflow.md) — Invoked for feature-type tasks
- [bugfix_workflow](bugfix_workflow.md) — Invoked for bugfix-type tasks
- [refactor_workflow](refactor_workflow.md) — Invoked for refactor-type tasks
