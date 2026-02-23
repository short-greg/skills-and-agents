**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: refactor-workflow
description: >
  Use when refactoring code to improve structure without changing behavior.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Behavior is preserved — all existing tests pass, external behavior unchanged
2. Code quality is improved — target dimension(s) measurably better (clarity, maintainability, etc.)
3. No regressions introduced — no new bugs, no broken functionality
4. Changes are incremental — each commit is self-contained and reversible

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task (incremental commits allow rollback to any passing state, check test status first on recovery, if tests failing: revert last change then continue)
3. Tests must exist before refactoring (can't verify preservation without tests)
4. Run tests after EACH incremental change
5. Commit after each passing increment (can rollback)
6. Stop immediately if tests fail — fix before continuing
7. On validation failure, investigate before continuing
8. Per `modularity.md` — apply modularity principles: reduce coupling, improve cohesion
9. Per `documentation.md` — update documentation if refactor changes structure
10. Iterate up to 2 times if refactoring fails before escalating

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Code to refactor, improvement goal

**Elicit if not provided:**
- Test coverage (verify from existing tests)
- Code structure (read and analyze)
- Project conventions (from codebase)

**Optional:** None

## Postconditions

The resulting state after the skill is finished.

**Success:** Code structure improved, all tests pass, behavior preserved, no regressions.

**Failure:** Insufficient test coverage, cannot preserve behavior, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason about refactor
2. Understand context (orient)
3. Define scope
4. Design approach
5. Implement incrementally
6. Validate refactoring

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Refactor (→ KR1, KR2)

Per `reasoning.md` — before beginning, reason about:

1. **Test coverage** — Adequate tests to verify behavior preservation?
2. **Scope assessment** — What needs to change? What must NOT change?
3. **Increment strategy** — How to break into small, verifiable steps?
4. **Risk identification** — Which changes are risky? Which are safe?

Output your reasoning.

### Understand Context (→ KR1, KR2)

Use `orient` primitive. Understand code to refactor, identify test coverage, existing patterns.

### Define Scope (→ KR1, KR2)

Use `define` primitive. Establish what will change (structure) and what will NOT change (behavior).

- **Refactoring goals:** What quality dimensions to improve
- **Preservation criteria:** What must stay the same (API, outputs, side effects)
- **Out of scope:** Feature additions, behavior changes

**Gate:** Verify tests exist to catch behavior changes. Ask user to confirm.

### Design Approach (→ KR4)

Use `design` primitive. Plan incremental refactoring steps. Each step is small and verifiable.

Per `modularity.md` — ensure steps maintain clear component boundaries.

**Gate:** Ask user to confirm design.

### Implement Incrementally (→ KR1, KR3, KR4)

Use `implement` primitive. Execute refactoring plan:

1. Make one small change
2. Run tests
3. If pass → commit → next change
4. If fail → revert or fix → re-run tests → continue

### Validate Refactoring (→ KR1, KR2, KR3)

Use `validate` primitive. Confirm: all tests pass, quality improved, no API changes, no new complexity.

**On failure:**
- Invoke `investigate` to determine which increment broke behavior
- Revert breaking increment or fix to preserve behavior
- Update checklist
- Re-validate

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand code to refactor, identify test coverage, existing patterns
- `define` — Establish what will change (structure) and what will NOT change (behavior)
- `design` — Plan incremental refactoring steps
- `implement` — Execute refactoring plan with continuous testing
- `validate` — Confirm behavior preserved and quality improved
- `critique` — Review intermediate states for issues
- `investigate` — Diagnose test failures or unexpected behavior

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

**Behavior Preservation:**

Behavior is preserved when:
- Public API contracts unchanged
- All existing tests pass
- No observable changes from client perspective

Internal changes are fine:
- Implementation details may change
- Private/internal APIs may change (if tests pass)

**Customization Points:**

- **Prototype:** Lighter test requirements, manual verification acceptable
- **Production:** Full test coverage required, all tests must pass after each increment, code review required
- **Library:** Must verify public API unchanged, consider downstream consumers
