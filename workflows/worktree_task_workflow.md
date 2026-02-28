**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: worktree-task-workflow
description: >
  Use when executing a task in a git worktree based on a task spec.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Task approach is reasoned about before starting
2. Task is understood — goal, dependencies, acceptance criteria are clear
3. Dependencies are satisfied — all required tasks merged before starting
4. Acceptance criteria are met — all specified criteria pass
5. Task is merge-ready — tests pass, changes committed, completion signaled
6. Documentation is updated if task changes behavior or reveals gaps

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work, routed workflow has its own checklist
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, re-verify dependencies (may have been merged), check if already signaled completion
3. Do NOT proceed if dependencies aren't merged (hard gate)
4. Route to correct workflow based on task type per `reasoning.md`
5. Validate against ALL acceptance criteria, not just tests per `goals_and_objectives.md`
6. Signal completion so orchestrator knows status
7. Stay in worktree (don't modify main branch directly)
8. Per `documentation.md` — update documentation if task changes behavior
9. Iterate up to 3 times if workflow execution fails before escalating

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Task spec (file or content), running in git worktree, task spec exists, dependencies merged

**Elicit if not provided:**
- Dependency status (check main branch)
- Available workflows (feature, bugfix, refactor)

**Optional:** Workflow preference if task type is ambiguous

## Postconditions

The resulting state after the skill is finished.

**Success:** Task implemented per spec, criteria met, tests pass, signaled ready for merge.

**Failure:** Dependencies not merged, task type unclear, cannot satisfy criteria, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason About Task
2. Understand Task
3. Validate Dependencies
4. Route to Workflow
5. Execute Workflow
6. Validate Completion
7. Signal Completion

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

### Execute Workflow (→ KR4, KR6)

Invoke selected workflow with task context:

- For feature: "Implement [Goal] with success criteria [Key Results]"
- For bugfix: "Fix [Goal] - expected behavior: [Key Results]"
- For refactor: "Refactor [Goal] - improvement targets: [Key Results]"

### Validate Completion (→ KR4, KR5)

Use `validate` primitive. Verify:

- All acceptance criteria satisfied
- All key results achieved
- Tests pass
- All changes committed
- Branch is clean and mergeable

**On failure:** Loop back to Execute Workflow, continue execution (up to 3 iterations per REQ9).

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

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Read task spec, understand goal and dependencies
- `validate` — Check dependencies are merged, verify completion criteria
- `implement` — Execute the routed workflow

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

**Customization Points:**

- **Prototype:** Simplified validation (tests pass, changes committed)
- **Production:** Full validation against all acceptance criteria, code review may be required
- **Library:** API compatibility checks, documentation requirements
