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
  - reasoning_patterns
  - manage_complexity_uncertainty_risk
---

# Worktree Orchestrate Workflow

**Goal:** Break a large plan into independent parallelizable tasks, manage dependencies, and coordinate safe integration.

**Intent:** Enable faster development by identifying task boundaries, setting up isolated worktrees for parallel execution, and merging completed work in correct dependency order. Prevent merge conflicts and incorrect sequencing.

**Scope:** Task decomposition and coordination: analyze plan for boundaries, create dependency graph, generate task specs, setup worktrees, track status, merge completed work.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (declarative goals with imperative dependency ordering)

---

## Key Results

1. PRD/plan decomposed into 2-6 independent tasks
2. Dependencies correctly identified (no circular dependencies)
3. Tasks within same wave don't modify same files
4. Worktrees created for ready tasks
5. Completed tasks merged in dependency order with passing tests

---

## Available Primitives

`orient`, `brainstorm`, `define`, `validate`, `implement`

---

## Constraints

- Tasks should be independent (minimize file overlap)
- Merge only in dependency order
- Test after EACH merge
- Stop immediately on merge conflict or test failure
- Clean up worktrees after merge

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Decomposition strategy** — By feature? By layer? By component?
2. **Task granularity** — How many tasks? (2-6 optimal)
3. **Dependency complexity** — Simple waves or complex graph?
4. **Overlap risk** — Which files might be touched by multiple tasks?

Output reasoning before proceeding.

---

## Execution

Execute by selecting and sequencing primitives to achieve key results.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand PRD/plan, project structure, identify parallelization opportunities.

### Phase 2: Brainstorm Decomposition

**Primitive:** `brainstorm` — See `primitives/brainstorm.md`

Explore decomposition options:
- By component (frontend, backend, database)
- By feature (user stories, use cases)
- By layer (API, service, data)

Recommend approach that minimizes file overlap.

### Phase 3: Define Task Specs

**Primitive:** `define` — See `primitives/define.md`

Create task specs (one per task) with:
- Goal and Key Results
- Dependencies (which tasks must complete first)
- Files likely affected
- Acceptance criteria

Create dependency graph with execution waves.

**Gate:** Invoke `validate` on task specs. User confirms decomposition.

### Phase 4: Setup Worktrees

**Primitive:** `implement` — See `primitives/implement.md`

Create worktrees for Wave 1 tasks (no dependencies):
```bash
git worktree add ../{repo}-worktrees/{task-name} -b parallel/{task-name}
```

Copy task spec to each worktree.

### Phase 5: Track and Merge

**Primitives:** `validate`, `implement`

Monitor status, merge completed tasks in dependency order:

1. Check task status (not started, in progress, ready for merge, merged)
2. Merge ready tasks respecting dependencies
3. Run tests after each merge
4. On pass: remove worktree, setup next wave tasks
5. On fail: STOP, report issue

**On merge conflict:** Stop immediately, user must resolve.

**On test failure:** Stop immediately, user must fix before continuing.

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

Per `recovery` protocol — check for existing parallel plan on startup.

**Step-specific notes:**
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
