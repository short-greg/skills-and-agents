---
name: worktree-task-workflow
description: >
  Use when executing a task in a git worktree based on a task spec.
  Triggers on: "execute this task", "work on this worktree task", "implement task spec".
argument-hint: "[task spec file or task description]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Worktree Task Workflow

**Goal:** Execute a single task in a git worktree based on a task specification, routing to the appropriate workflow.

**Intent:** Enable reliable task execution in parallel worktree environments by reading task spec, verifying dependencies, determining appropriate workflow, and executing to completion.

**Scope:** Single task execution: read spec, check dependencies, route to workflow (feature/bugfix/refactor), validate completion, signal readiness for merge.

---

## Key Results

1. Task approach is reasoned about before starting
2. Task is understood — goal, dependencies, acceptance criteria are clear
3. Dependencies are satisfied — all required tasks merged before starting
4. Acceptance criteria are met — all specified criteria pass
5. Task is merge-ready — tests pass, changes committed, completion signaled
6. Documentation is updated if task changes behavior or reveals gaps

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress through task execution.
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist based on the task spec. Update dynamically.
- `reasoning.md` — Reason about task type and workflow selection before starting.
- `goals_and_objectives.md` — Validate against task acceptance criteria.
- `documentation.md` — Update documentation if task changes behavior.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these for task execution. If you do not understand a primitive, read it before using it.

- `orient` — Read task spec, understand goal and dependencies.
- `validate` — Check dependencies are merged, verify completion criteria.
- `implement` — Execute the routed workflow.

---

## Constraints

- Do NOT proceed if dependencies aren't merged (hard gate)
- Route to correct workflow based on task type
- Validate against ALL acceptance criteria, not just tests
- Signal completion so orchestrator knows status
- Stay in worktree (don't modify main branch directly)

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Task (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Task type** — Feature? Bugfix? Refactor? Documentation?
2. **Dependency status** — All dependencies merged?
3. **Workflow selection** — Which workflow matches this task?
4. **Scope clarity** — Are acceptance criteria clear?

Output your reasoning.

### Understand Task (→ KR2)

Use `orient` primitive. Read task spec, understand:

- Goal and Key Results
- Dependencies
- Acceptance criteria
- Files likely affected

### Validate Dependencies (→ KR3)

Use `validate` primitive. Check all dependencies are merged to main.

**On failure (dependencies not merged):**
```
BLOCKED: Dependencies not satisfied

Unmerged dependencies:
- [task-name] (not merged)

Action: Wait for dependencies to complete and merge before starting.
```

**Critical:** This is a hard gate. Do NOT proceed.

### Route to Workflow (→ KR2)

Analyze task spec to determine workflow:

| Task Indicators | Workflow |
|-----------------|----------|
| "implement", "add", "create", "build" | feature_workflow |
| "fix", "bug", "error", "issue" | bugfix_workflow |
| "refactor", "improve", "clean up" | refactor_workflow |

**If unclear:** Ask user which workflow to use.

### Execute Workflow (→ KR4)

Invoke selected workflow with task context:

- For feature: "Implement [Goal] with success criteria [Key Results]"
- For bugfix: "Fix [Goal] - expected behavior: [Key Results]"
- For refactor: "Refactor [Goal] - improvement targets: [Key Results]"

### Validate Completion (→ KR4, KR5, KR6)

Use `validate` primitive. Verify:

- All acceptance criteria satisfied
- All key results achieved
- Tests pass
- All changes committed
- Branch is clean and mergeable

**On failure:** Loop back to Execute Workflow, continue execution.

**Iteration bounds:** Max 3 iterations before escalating.

### Signal Completion (→ KR5)

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

## Progress Tracking

Per `checklists.md` — build checklist using format: `<Skill> - KR<num> - <task>`
- The routed workflow has its own checklist
- Mark items complete immediately after finishing each task
- Report progress after each completed item

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

Per `recovery.md` — check for existing trace on startup.

**Task-specific notes:**
- Re-verify dependencies (may have been merged)
- If workflow in progress, that workflow has its own recovery
- Check if already signaled completion

---

## Customization Points

**Prototype:** Simplified validation (tests pass, changes committed).

**Production:** Full validation against all acceptance criteria, code review may be required.

**Library:** API compatibility checks, documentation requirements.
