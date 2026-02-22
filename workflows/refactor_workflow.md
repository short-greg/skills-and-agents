---
name: refactor-workflow
description: >
  Use when refactoring code to improve structure without changing behavior.
  Triggers on: "refactor this code", "improve code structure", "safe refactoring".
argument-hint: "[code to refactor and improvement goal]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Refactor Workflow

**Goal:** Improve code structure, clarity, or maintainability without changing external behavior.

**Intent:** Enable safe refactoring that preserves correctness while improving quality. Prevent "refactoring" that introduces bugs or changes behavior.

**Scope:** Structural code improvement with behavior preservation: identify targets, define preservation criteria, design incremental approach, implement with continuous verification.

---

## Key Results

1. Uncertainties about scope and approach are resolved before starting
2. Behavior is preserved — all existing tests pass, external behavior unchanged
3. Code quality is improved — target dimension(s) measurably better (clarity, maintainability, etc.)
4. No regressions introduced — no new bugs, no broken functionality
5. API contracts unchanged — public interfaces remain stable (unless internal-only refactoring)
6. Changes are incremental — each commit is self-contained and reversible
7. Documentation is updated if refactoring changes patterns or reveals missing docs

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress through analyze → plan → refactor → validate phases
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist after reasoning about the refactor. Update it dynamically.
- `reasoning.md` — Reason about refactor approach before starting. Verify behavior preserved after.
- `goals_and_objectives.md` — Define what "improved" means without changing behavior.
- `modularity.md` — Apply modularity principles: reduce coupling, improve cohesion.
- `documentation.md` — Update documentation if refactor changes structure.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to refactor. If you do not understand a primitive, read it before using it.

- `orient` — Understand code to refactor, identify test coverage, existing patterns.
- `define` — Establish what will change (structure) and what will NOT change (behavior).
- `design` — Plan incremental refactoring steps.
- `implement` — Execute refactoring plan with continuous testing.
- `validate` — Confirm behavior preserved and quality improved.
- `critique` — Review intermediate states for issues.
- `investigate` — Diagnose test failures or unexpected behavior.

---

## Constraints

- Tests must exist before refactoring (can't verify preservation without tests)
- Run tests after EACH incremental change
- Commit after each passing increment (can rollback)
- Stop immediately if tests fail — fix before continuing
- Don't add features or change behavior
- On validation failure, investigate before continuing

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Refactor (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Test coverage** — Adequate tests to verify behavior preservation?
2. **Scope assessment** — What needs to change? What must NOT change?
3. **Increment strategy** — How to break into small, verifiable steps?
4. **Risk identification** — Which changes are risky? Which are safe?

Output your reasoning.

### Understand Context (→ KR1)

Use `orient` primitive. Understand code to refactor, identify test coverage, existing patterns.

### Define Scope (→ KR2, KR3, KR5)

Use `define` primitive. Establish what will change (structure) and what will NOT change (behavior).

- **Refactoring goals:** What quality dimensions to improve
- **Preservation criteria:** What must stay the same (API, outputs, side effects)
- **Out of scope:** Feature additions, behavior changes

**Gate:** Verify tests exist to catch behavior changes. Ask user to confirm.

### Design Approach (→ KR6)

Use `design` primitive. Plan incremental refactoring steps. Each step is small and verifiable.

Per `modularity.md` — ensure steps maintain clear component boundaries.

**Gate:** Ask user to confirm design.

### Implement Incrementally (→ KR2, KR4, KR6)

Use `implement` primitive. Execute refactoring plan:

1. Make one small change
2. Run tests
3. If pass → commit → next change
4. If fail → revert or fix → re-run tests → continue

### Validate Refactoring (→ KR2, KR3, KR4, KR5, KR7)

Use `validate` primitive. Confirm: all tests pass, quality improved, no API changes, no new complexity.

**On failure:**
- Invoke `investigate` to determine which increment broke behavior
- Revert breaking increment or fix to preserve behavior
- Update checklist
- Re-validate

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist after reasoning about the refactor, based on what you learned
- Expand checklist into specific increments after design task
- Run tests after EACH increment — if fail, stop and investigate
- Mark items complete immediately after finishing each task
- Report progress after each completed item

---

## Preconditions

**Must be provided:** Code to refactor, improvement goal

**Self-satisfiable:** Test coverage, code structure, project conventions (read docs and code)

---

## Postconditions

**Success:** Code structure improved, all tests pass, behavior preserved, no regressions.

**Failure:** Insufficient test coverage, cannot preserve behavior, or user aborts.

---

## Recovery

Per `recovery.md` — check for existing trace on startup, resume from last completed task.

**Task-specific notes:**
- Incremental commits allow rollback to any passing state
- Check test status first on recovery
- If tests failing: revert last change, then continue

---

## Iteration

Per `checklists.md`:
- Max 2 refactoring iterations before escalating
- If same test fails repeatedly, escalate immediately

---

## Behavior Preservation

**Behavior is preserved when:**
- Public API contracts unchanged
- All existing tests pass
- No observable changes from client perspective

**Internal changes are fine:**
- Implementation details may change
- Private/internal APIs may change (if tests pass)

---

## Customization Points

**Prototype:** Lighter test requirements, manual verification acceptable.

**Production:** Full test coverage required, all tests must pass after each increment, code review required.

**Library:** Must verify public API unchanged, consider downstream consumers.
