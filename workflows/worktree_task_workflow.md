---
name: worktree-task-workflow
description: >
  Use when executing a task in a git worktree based on a task spec.
  Triggers on: "execute this task", "work on this worktree task", "implement task spec".
argument-hint: "[task spec file or task description]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
protocols:
  - tracking
  - recovery
  - checklist_management
  - reasoning_patterns
  - goals_and_objectives
  - doc_maintenance
---

# Worktree Task Workflow

**Goal:** Execute a single task in a git worktree based on a task specification, routing to the appropriate workflow.

**Intent:** Enable reliable task execution in parallel worktree environments by reading task spec, verifying dependencies, determining appropriate workflow, and executing to completion.

**Scope:** Single task execution: read spec, check dependencies, route to workflow (feature/bugfix/refactor), validate completion, signal readiness for merge.

---

## Workflow Type

**Type:** Adaptive

**Style:** Declarative (routes to other workflows based on task type)

---

## Key Results

Per `goals_and_objectives` protocol — outcome-oriented results:

**Required:**
1. **Task is understood** — goal, dependencies, acceptance criteria are clear
2. **Dependencies are satisfied** — all required tasks merged before starting
3. **Acceptance criteria are met** — all specified criteria pass
4. **Task is merge-ready** — tests pass, changes committed, completion signaled

**Conditional:**
5. **Documentation is updated** — if task changes behavior or reveals gaps (per `doc_maintenance`)

---

## Available Primitives

`orient`, `validate`, `implement`

---

## Constraints

- Do NOT proceed if dependencies aren't merged (hard gate)
- Route to correct workflow based on task type
- Validate against ALL acceptance criteria, not just tests
- Signal completion so orchestrator knows status
- Stay in worktree (don't modify main branch directly)

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Task type** — Feature? Bugfix? Refactor? Documentation?
2. **Dependency status** — All dependencies merged?
3. **Workflow selection** — Which workflow matches this task?
4. **Scope clarity** — Are acceptance criteria clear?

Output reasoning before proceeding.

---

## Progress Tracking (Required)

Per `checklist_management` protocol — create and maintain a checklist throughout execution.

**On workflow start, create this checklist:**

```markdown
## Task Progress

- [ ] 1. Understand task (orient)
- [ ] 2. Validate dependencies (validate) — HARD GATE: must pass
- [ ] 3. Route to workflow
- [ ] 4. Execute workflow (feature/bugfix/refactor)
- [ ] 5. Validate completion (validate)
- [ ] 6. Signal completion
```

**Rules:**
- Display checklist at start of workflow
- Step 2 is a HARD GATE — do NOT proceed if dependencies not merged
- Step 4 invokes another workflow which has its own checklist
- Report progress after each completed item: "✅ [item] — [brief summary]"

---

## Execution

Execute by selecting and sequencing primitives to achieve key results.

### Phase 1: Understand Task

**Primitive:** `orient` — See `primitives/orient.md`

Read task spec, understand:
- Goal and Key Results
- Dependencies
- Acceptance criteria
- Files likely affected

### Phase 2: Validate Dependencies

**Primitive:** `validate` — See `primitives/validate.md`

Check all dependencies are merged to main.

**On failure (dependencies not merged):**
```
BLOCKED: Dependencies not satisfied

Unmerged dependencies:
- [task-name] (not merged)

Action: Wait for dependencies to complete and merge before starting.
```

**Critical:** This is a hard gate. Do NOT proceed.

### Phase 3: Route to Workflow

Analyze task spec to determine workflow:

| Task Indicators | Workflow |
|-----------------|----------|
| "implement", "add", "create", "build" | feature_workflow |
| "fix", "bug", "error", "issue" | bugfix_workflow |
| "refactor", "improve", "clean up" | refactor_workflow |

**If unclear:** Ask user which workflow to use.

### Phase 4: Execute Workflow

Invoke selected workflow with task context:
- For feature: "Implement [Goal] with success criteria [Key Results]"
- For bugfix: "Fix [Goal] - expected behavior: [Key Results]"
- For refactor: "Refactor [Goal] - improvement targets: [Key Results]"

### Phase 5: Validate Completion

**Primitive:** `validate` — See `primitives/validate.md`

Verify:
- All acceptance criteria satisfied
- All key results achieved
- Tests pass
- All changes committed
- Branch is clean and mergeable

**On failure:** Loop back to Phase 4, continue workflow execution.

**Iteration bounds:** Max 3 iterations before escalating.

### Phase 6: Signal Completion

Output completion signal:
```markdown
## Task Complete: [Task Name]

**Status:** Ready for merge
**Branch:** parallel/[task-name]

**Acceptance Criteria:**
- [x] Criterion 1 - [evidence]
- [x] Criterion 2 - [evidence]

**Tests:** All passing

**Merge command:**
git merge parallel/[task-name]
```

---

## Preconditions

**Must be provided:** Task spec (file or content)

**Self-satisfiable:** Dependency status (check main branch), available workflows

**Prerequisites:** Running in git worktree, task spec exists, dependencies merged

---

## Postconditions

**Success:** Task implemented per spec, criteria met, tests pass, signaled ready for merge.

**Failure:** Dependencies not merged, task type unclear, cannot satisfy criteria.

---

## Recovery

Per `recovery` protocol — check for existing trace on startup.

**Step-specific notes:**
- Re-verify dependencies (may have been merged)
- If workflow in progress, that workflow has its own recovery
- Check if already signaled completion

---

## Customization Points

**Prototype:** Simplified validation (tests pass, changes committed).

**Production:** Full validation against all acceptance criteria, code review may be required.

**Library:** API compatibility checks, documentation requirements.
