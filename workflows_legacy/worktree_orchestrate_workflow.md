**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: worktree-orchestrate-workflow
description: >
  Use when breaking large features into parallelizable tasks across git worktrees.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Decomposition approach is reasoned about before starting
2. Tasks are independent — 2-6 tasks with minimal file overlap within execution waves
3. Dependencies are correct — no circular dependencies, graph is valid
4. Task specs are complete — each task has goal, key results, dependencies, acceptance criteria
5. Merges are safe — completed tasks merged in dependency order with passing tests
6. Worktrees are ready — created for wave 1 tasks (if user wants parallel execution now)

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work, track task status: not started → in progress → ready for merge → merged
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing parallel plan on startup, check which worktrees exist and which branches merged, resume from current state
3. Tasks should be independent — minimize file overlap per `risk_management.md`
4. Merge only in dependency order
5. Test after EACH merge
6. Stop immediately on merge conflict or test failure
7. Clean up worktrees after merge
8. Per `goals_and_objectives.md` — ensure task specs have clear success criteria
9. Iterate up to 2 times if decomposition needs revision

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** PRD or plan to decompose, Git 2.5+, clean working directory, stable main branch

**Elicit if not provided:**
- Project structure (read and analyze)
- Git setup (verify worktree support)
- Decomposition strategy preference (by feature, layer, component)

**Optional:** Task granularity preference, specific dependency constraints

## Postconditions

The resulting state after the skill is finished.

**Success:** All tasks defined, worktrees created for ready tasks, completed tasks merged with passing tests.

**Failure:** Cannot decompose into independent tasks, circular dependencies, merge conflicts, test failures, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason About Decomposition
2. Understand Context
3. Brainstorm Decomposition
4. Define Task Specs
5. Setup Worktrees
6. Track and Merge

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Decomposition (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Decomposition strategy** — By feature? By layer? By component?
2. **Task granularity** — How many tasks? (2-6 optimal)
3. **Dependency complexity** — Simple waves or complex graph?
4. **Overlap risk** — Which files might be touched by multiple tasks?

Output your reasoning.

### Understand Context (→ KR1, KR2)

Use `orient` primitive. Understand PRD/plan, project structure, identify parallelization opportunities.

### Brainstorm Decomposition (→ KR2)

Use `brainstorm` primitive. Explore decomposition options:

- By component (frontend, backend, database)
- By feature (user stories, use cases)
- By layer (API, service, data)

Recommend approach that minimizes file overlap.

### Define Task Specs (→ KR2, KR3, KR4)

Use `define` primitive. Create task specs (one per task) with:

- Goal and Key Results
- Dependencies (which tasks must complete first)
- Files likely affected
- Acceptance criteria

Create dependency graph with execution waves.

**Gate:** Ask user to confirm decomposition.

### Setup Worktrees (→ KR6)

Use `implement` primitive. Create worktrees for Wave 1 tasks (no dependencies):

```bash
git worktree add ../{repo}-worktrees/{task-name} -b parallel/{task-name}
```

Copy task spec to each worktree.

### Track and Merge (→ KR5)

Use `validate` and `implement` primitives. Monitor status, merge completed tasks in dependency order:

1. Check task status (not started, in progress, ready for merge, merged)
2. Merge ready tasks respecting dependencies
3. Run tests after each merge
4. On pass: remove worktree, setup next wave tasks
5. On fail: STOP, report issue

**On merge conflict:** Stop immediately, user must resolve.

**On test failure:** Stop immediately, user must fix before continuing.

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand PRD/plan, project structure, identify parallelization opportunities
- `brainstorm` — Explore decomposition options
- `define` — Create task specs with goals, dependencies, acceptance criteria
- `validate` — Verify task specs, check merge safety
- `implement` — Create worktrees, execute merges

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

**Task Spec Format:**

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

**Customization Points:**

- **Prototype:** Simplified task specs, fewer dependency checks
- **Production:** Full task specs, strict dependency enforcement, required code review before merge
- **Library:** Task specs include API compatibility notes, changelog updates per task
