---
name: worktree-orchestrate-workflow
description: >
  Use when breaking large features into parallelizable tasks across git worktrees.
  Triggers on: "parallelize this work", "break into tasks", "coordinate parallel development".
argument-hint: "[PRD or plan to decompose into parallel tasks]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Worktree Orchestrate Workflow

**Goal:** Break a large plan into independent parallelizable tasks, manage dependencies, and coordinate safe integration.

**Intent:** Enable faster development by identifying task boundaries, setting up isolated worktrees for parallel execution, and merging completed work in correct dependency order. Prevent merge conflicts and incorrect sequencing.

**Scope:** Task decomposition and coordination: analyze plan for boundaries, create dependency graph, generate task specs, setup worktrees, track status, merge completed work.

---

## Key Results

1. Decomposition approach is reasoned about before starting
2. Tasks are independent — 2-6 tasks with minimal file overlap within execution waves
3. Dependencies are correct — no circular dependencies, graph is valid
4. Task specs are complete — each task has goal, key results, dependencies, acceptance criteria
5. Merges are safe — completed tasks merged in dependency order with passing tests
6. Worktrees are ready — created for wave 1 tasks (if user wants parallel execution now)

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track task status: not started → in progress → ready for merge → merged
- `recovery.md` — On startup, check for existing parallel plan. Resume from current state.
- `checklists.md` — Create a checklist after reasoning about decomposition. Update dynamically.
- `reasoning.md` — Reason about decomposition strategy before starting.
- `goals_and_objectives.md` — Ensure task specs have clear success criteria.
- `risk_management.md` — Identify overlap risks and dependency issues.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these for orchestration. If you do not understand a primitive, read it before using it.

- `orient` — Understand PRD/plan, project structure, identify parallelization opportunities.
- `brainstorm` — Explore decomposition options.
- `define` — Create task specs with goals, dependencies, acceptance criteria.
- `validate` — Verify task specs, check merge safety.
- `implement` — Create worktrees, execute merges.

---

## Constraints

- Tasks should be independent (minimize file overlap)
- Merge only in dependency order
- Test after EACH merge
- Stop immediately on merge conflict or test failure
- Clean up worktrees after merge

---

## Tasks

Execute these tasks to achieve the key results. Select and sequence based on your reasoning.

### Reason About Decomposition

Per `reasoning.md` — before beginning, reason about:

1. **Decomposition strategy** — By feature? By layer? By component?
2. **Task granularity** — How many tasks? (2-6 optimal)
3. **Dependency complexity** — Simple waves or complex graph?
4. **Overlap risk** — Which files might be touched by multiple tasks?

Output your reasoning.

### Understand Context

Use `orient` primitive. Understand PRD/plan, project structure, identify parallelization opportunities.

### Brainstorm Decomposition

Use `brainstorm` primitive. Explore decomposition options:

- By component (frontend, backend, database)
- By feature (user stories, use cases)
- By layer (API, service, data)

Recommend approach that minimizes file overlap.

### Define Task Specs

Use `define` primitive. Create task specs (one per task) with:

- Goal and Key Results
- Dependencies (which tasks must complete first)
- Files likely affected
- Acceptance criteria

Create dependency graph with execution waves.

**Gate:** Ask user to confirm decomposition.

### Setup Worktrees

Use `implement` primitive. Create worktrees for Wave 1 tasks (no dependencies):

```bash
git worktree add ../{repo}-worktrees/{task-name} -b parallel/{task-name}
```

Copy task spec to each worktree.

### Track and Merge

Use `validate` and `implement` primitives. Monitor status, merge completed tasks in dependency order:

1. Check task status (not started, in progress, ready for merge, merged)
2. Merge ready tasks respecting dependencies
3. Run tests after each merge
4. On pass: remove worktree, setup next wave tasks
5. On fail: STOP, report issue

**On merge conflict:** Stop immediately, user must resolve.

**On test failure:** Stop immediately, user must fix before continuing.

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist after reasoning about decomposition, based on what you learned
- Expand task specs after decomposition is decided
- Track task status: not started → in progress → ready for merge → merged
- Mark items complete immediately after finishing each task
- Report progress after each completed item

---

## Preconditions

**Must be provided:** PRD or plan to decompose

**Self-satisfiable:** Project structure, git setup (read and verify)

**Prerequisites:** Git 2.5+, clean working directory, stable main branch

---

## Postconditions

**Success:** All tasks defined, worktrees created for ready tasks, completed tasks merged with passing tests.

**Failure:** Cannot decompose into independent tasks, circular dependencies, merge conflicts, or test failures.

---

## Recovery

Per `recovery.md` — check for existing parallel plan on startup.

**Task-specific notes:**
- Check which worktrees exist
- Check which branches are merged
- Resume from current state
- If interrupted mid-merge, verify test status before continuing

---

## Task Spec Format

```markdown
# Task: [Task Name]

## Goal
[What this task should accomplish]

## Key Results
1. [Measurable outcome]

## Dependencies
- [Task names this depends on, or "None"]

## Files Likely Affected
- [List of files/directories]

## Acceptance Criteria
- [ ] [Criterion]
```

---

## Customization Points

**Prototype:** Simplified task specs, fewer dependency checks.

**Production:** Full task specs, strict dependency enforcement, required code review before merge.

**Library:** Task specs include API compatibility notes, changelog updates per task.
