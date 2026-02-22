---
name: refactor-workflow
description: >
  Use when refactoring code to improve structure without changing behavior.
  Triggers on: "refactor this code", "improve code structure", "safe refactoring".
argument-hint: "[code to refactor and improvement goal]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task
protocols:
  - tracking
  - recovery
  - checklist_management
  - reasoning_patterns
  - modularity
---

# Refactor Workflow

**Goal:** Improve code structure, clarity, or maintainability without changing external behavior.

**Intent:** Enable safe refactoring that preserves correctness while improving quality. Prevent "refactoring" that introduces bugs or changes behavior.

**Scope:** Structural code improvement with behavior preservation: identify targets, define preservation criteria, design incremental approach, implement with continuous verification.

---

## Workflow Type

**Type:** Deterministic

**Style:** Hybrid (declarative goals with imperative test-after-each-change)

---

## Key Results

1. Behavior is preserved (all tests pass)
2. Code quality is improved in target dimension(s)
3. No regressions introduced
4. API contracts unchanged (unless internal)
5. Changes are incremental and reversible

---

## Available Primitives

`orient`, `define`, `design`, `implement`, `validate`, `critique`, `investigate`

---

## Constraints

- Tests must exist before refactoring (can't verify preservation without tests)
- Run tests after EACH incremental change
- Commit after each passing increment (can rollback)
- Stop immediately if tests fail — fix before continuing
- Don't add features or change behavior
- On validation failure, investigate before continuing

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Test coverage** — Adequate tests to verify behavior preservation?
2. **Scope assessment** — What needs to change? What must NOT change?
3. **Increment strategy** — How to break into small, verifiable steps?
4. **Risk identification** — Which changes are risky? Which are safe?

Output reasoning before proceeding.

---

## Execution

Execute by selecting and sequencing primitives to achieve key results. The following phases provide guidance, not rigid steps.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand code to refactor, identify test coverage, existing patterns.

### Phase 2: Define Scope

**Primitive:** `define` — See `primitives/define.md`

Establish what will change (structure) and what will NOT change (behavior).

- **Refactoring goals:** What quality dimensions to improve
- **Preservation criteria:** What must stay the same (API, outputs, side effects)
- **Out of scope:** Feature additions, behavior changes

**Gate:** Invoke `validate` on scope. Verify tests exist to catch behavior changes. User confirms.

### Phase 3: Design Approach

**Primitive:** `design` — See `primitives/design.md`

Plan incremental refactoring steps. Each step is small and verifiable.

Per `modularity` protocol — ensure steps maintain clear component boundaries.

**Gate:** Invoke `validate` or `critique` on design. User confirms.

### Phase 4: Implement Incrementally

**Primitive:** `implement` — See `primitives/implement.md`

Execute refactoring plan:
1. Make one small change
2. Run tests
3. If pass → commit → next change
4. If fail → revert or fix → re-run tests → continue

### Phase 5: Validate Refactoring

**Primitive:** `validate` — See `primitives/validate.md`

Confirm: all tests pass, quality improved, no API changes, no new complexity.

**On failure:** Per `checklist_management` protocol:
1. Invoke `investigate` to determine which increment broke behavior
2. Revert breaking increment or fix to preserve behavior
3. Update checklist
4. Re-validate

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

Per `recovery` protocol — check for existing trace on startup, resume from last completed step.

**Step-specific notes:**
- Phase 4 uses incremental commits — can rollback to any passing state
- Check test status first on recovery
- If tests failing: revert last change, then continue

---

## Iteration

Per `checklist_management` protocol:
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
