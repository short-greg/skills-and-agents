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
---

# Refactor Workflow

**Goal:** Improve code structure, clarity, or maintainability without changing external behavior.

**Intent:** Enable safe refactoring that preserves correctness while improving code quality. Prevent "refactoring" that introduces bugs, changes behavior, or makes code harder to understand.

**Scope:** Structural code improvement. Includes: identifying refactoring targets and goals, defining what should NOT change (behavior preservation), designing the refactoring approach, implementing changes incrementally, and validating behavior is preserved. The workflow produces improved code with verified behavior preservation.

---

## Workflow Type

**Type:** Deterministic

**Style:** Imperative

**Assumptions (what makes this deterministic):**
- Behavior preservation is binary — either behavior changed or it didn't
- Tests provide reliable behavior verification
- Refactoring targets are clear from orient phase
- No discovery of hidden dependencies (tests catch breaking changes)

**Note:** While deterministic in structure, validation failures may reveal issues requiring investigation.

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand code to be refactored, identify patterns, understand test coverage

**Inputs:** Code to refactor and improvement goal from user

**Outputs:**
- Code structure understood (dependencies, interfaces, complexity)
- Current test coverage identified
- Refactoring scope boundaries identified (what's in scope, what's adjacent)
- Existing patterns and conventions identified

**Self-satisfiable:** Reads code, tests, and project conventions

### Step 2: Define Refactoring Scope

**Primitive:** define

**Purpose:** Establish what will change (structure), what will NOT change (behavior), and how to verify preservation

**Inputs:**
- Code understanding from Step 1
- Improvement goal from user
- Existing tests from Step 1

**Outputs:**
- Refactoring goals (what quality dimensions to improve: clarity, modularity, efficiency, etc.)
- Behavior preservation criteria (what must stay the same: API contracts, outputs, side effects)
- Success criteria (tests pass, behavior preserved, quality improved)
- Out of scope (functionality changes, feature additions)

**Examples of what's in scope:**
- Rename variables/functions for clarity
- Extract methods to reduce complexity
- Reorganize code structure
- Improve modularity (split god objects, reduce coupling)
- Simplify control flow
- Remove duplication

**Examples of what's out of scope:**
- Changing API contracts (unless internal)
- Adding features or functionality
- Changing outputs or behavior
- Modifying requirements

### Step 3: Validate Scope Definition

**Primitive:** validate

**Purpose:** Verify scope is clear and behavior preservation is verifiable before designing refactoring

**Inputs:**
- Refactoring scope from Step 2
- Behavior preservation criteria from Step 2

**Outputs:**
- Pass/fail verdict
- Verification that tests exist to catch behavior changes
- Confirmation that scope is achievable without behavior changes

**On failure:**
1. Check if adequate tests exist (need tests before safe refactoring)
2. Clarify behavior preservation criteria
3. Loop back to Step 2 with refinements

**Gate:** User confirms scope and preservation criteria

### Step 4: Design Refactoring Approach

**Primitive:** design

**Purpose:** Plan incremental refactoring steps that preserve behavior at each increment

**Inputs:**
- Validated scope from Steps 2-3
- Code structure from Step 1
- Test coverage from Step 1

**Outputs:**
- Refactoring steps (small, incremental changes)
- Order of changes (dependencies between steps)
- Validation checkpoints (run tests after each increment)
- Risk assessment (which changes are risky, which are safe)

**Incremental approach:**
- Each step is a small, verifiable change
- Tests run after each step (catch regressions immediately)
- Changes build on each other (early steps enable later steps)
- Can stop at any increment if issues arise

**Example incremental steps:**
1. Extract complex logic into well-named helper function
2. Add tests for helper function (if not covered by existing tests)
3. Replace duplicated code with calls to helper
4. Run tests after each replacement
5. Remove dead code
6. Verify final state

### Step 5: Validate Design

**Primitive:** validate OR critique

**Purpose:** Verify refactoring plan preserves behavior and achieves improvement goals

**Inputs:**
- Refactoring design from Step 4
- Scope from Step 2

**Outputs:**
- Pass/fail verdict
- Verification that plan achieves goals
- Verification that incremental steps preserve behavior

**On failure:**
1. Identify which steps might break behavior
2. Identify missing increments or unsafe jumps
3. Loop back to Step 4 with refinements

**Alternative:** Use `critique` for qualitative assessment of refactoring strategy

**Gate:** User confirms approach before implementation

### Step 6: Implement Refactoring

**Primitive:** implement

**Purpose:** Execute refactoring plan incrementally, running tests after each change

**Inputs:**
- Validated refactoring design from Steps 4-5
- Code from Step 1
- Tests from Step 1

**Outputs:**
- Refactored code
- Test results after each increment (all pass)
- Documentation updates (if refactoring affects public APIs or architecture)

**Implementation discipline:**
- Make one small change at a time
- Run tests after EACH change (catch regressions immediately)
- Commit after each passing increment (can rollback if needed)
- Stop if tests fail — fix before continuing
- Don't add features or change behavior

**If tests fail mid-refactoring:**
1. Identify what changed to break tests
2. Either:
   - Fix the breaking change (preserve behavior)
   - Revert the change (roll back to last passing state)
3. Update refactoring plan if necessary
4. Continue incrementally

### Step 7: Validate Refactoring

**Primitive:** validate

**Purpose:** Confirm behavior is preserved and quality is improved

**Inputs:**
- Refactored code from Step 6
- Original code from Step 1
- Behavior preservation criteria from Step 2
- Success criteria from Step 2

**Outputs:**
- Pass/fail verdict with evidence
- Test results (all tests pass)
- Quality assessment (improvement in target dimensions: clarity, modularity, etc.)
- Behavior diff (should be none, or only internal changes)

**Validation criteria:**
1. All tests pass (behavior preserved)
2. No API contract changes (unless internal)
3. Quality improved in target dimensions (measured via code review or metrics)
4. No new complexity introduced (cyclomatic complexity, coupling, etc.)

**On failure:**
1. Invoke `investigate` to determine what changed behaviorally
2. Determine if:
   - Tests are inadequate (add tests)
   - Refactoring broke behavior (revert and retry)
   - Quality didn't improve (refactoring missed goal)
3. Update checklist and remediate

**Iteration bounds:** Max 2 refactoring iterations before escalating to user

---

## Preconditions

**Must be provided:**
- code to refactor: what code needs improvement
- improvement goal: what quality dimension to improve (clarity, modularity, performance, etc.)

**Self-satisfiable:**
- test coverage: identify existing tests
- code structure: read and understand code to refactor
- project conventions: identify patterns to follow

---

## Postconditions

**Success:**
- Code structure is improved
- Behavior is preserved (all tests pass)
- Quality improved in target dimension(s)
- No regressions introduced

**Failure (blocked):**
- Insufficient test coverage to verify behavior preservation (blocked at Step 3)
- Refactoring plan cannot preserve behavior (blocked at Step 5)
- Implementation breaks tests (blocked at Step 6)
- Behavior changed unintentionally (blocked at Step 7)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify last completed increment from trace
   - Check test status (did last change pass tests?)
   - Resume from appropriate point
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1 (Orient):** Safe to re-run from beginning if interrupted
- **Step 2 (Define):** If partial scope exists, read and continue
- **Step 4 (Design):** If partial plan exists, read and continue
- **Step 6 (Implement):**
  - **Critical:** Check test status first
  - If tests passing: review last increment, continue with next
  - If tests failing: revert last change, then continue
  - Read trace to understand which increments are complete
- **Step 7 (Validate):** Safe to re-run from beginning if interrupted

**Special recovery consideration:** Step 6 uses incremental commits — can rollback to any passing state.

---

## On Validation Failure

**Validation failure indicates behavior changed unintentionally:**

1. **Validate reports failure** — which behavior changed or which tests fail
2. **Invoke investigate** — determine which refactoring step caused the change:
   - Review commit history / trace of increments
   - Identify which increment broke behavior
3. **Remediation options:**
   - Revert the breaking increment
   - Fix the increment to preserve behavior
   - Update tests if behavior change is acceptable (confirm with user)
4. **Update trace** and checklist
5. **Re-run from corrected point**
6. **Re-validate**

**Iteration bounds:** Max 2 refactoring iterations before escalating to user

---

## Quality Gates

This workflow includes quality gates for scope verification and behavior preservation:

| Step | Gate | Purpose |
|------|------|---------|
| Step 3 | Validate scope | Verify scope is achievable and verifiable |
| Step 5 | Validate design | Verify plan preserves behavior |
| Step 7 | Validate refactoring | Verify behavior preserved and quality improved |

**User approval gates:**
- After Step 3: User confirms scope and preservation criteria
- After Step 5: User confirms refactoring approach

---

## Incremental Refactoring Pattern

This workflow uses small, verifiable increments:

```
Change 1 → Run tests → Pass → Commit
  ↓
Change 2 → Run tests → Pass → Commit
  ↓
Change 3 → Run tests → Fail → Revert or Fix → Re-run tests → Pass → Commit
  ↓
...continue until complete
```

**Why incremental?**
- Catch regressions immediately (know which change broke what)
- Can rollback to any passing state
- Reduces risk of large, breaking changes
- Easier to review (small diffs)

**Increment size:**
- Small enough to isolate failures (one logical change)
- Large enough to make progress (not single-line commits)
- Typically: one method extraction, one rename, one reorganization

---

## Behavior Preservation

**What is behavior preservation?**

Behavior is preserved when:
- Public API contracts unchanged (inputs, outputs, side effects)
- All existing tests pass
- No observable changes from client perspective

Behavior is NOT necessarily preserved when:
- Internal implementation details change (this is fine)
- Performance characteristics change (unless performance is specified behavior)
- Private/internal APIs change (this is fine if tests pass)

**How to verify preservation:**

1. **Run all tests** — pass/fail verdict
2. **Check API contracts** — compare before/after signatures
3. **Review side effects** — database, file system, network calls unchanged
4. **Manual testing** (if test coverage is inadequate)

**When behavior change is acceptable:**

- Fixing a bug (behavior was wrong)
- User explicitly approves behavior change
- "Refactoring" becomes "changing requirements" (use feature workflow instead)

---

## Customization Points

When adapting this workflow for your project:

### Testing Conventions
- Test framework and commands
- How to run full test suite
- How to run tests for specific modules
- Coverage reporting (to verify refactored code is covered)

### Refactoring Tools
- IDE refactoring features (safe renames, extractions)
- Linters and static analysis (to measure quality improvement)
- Code metrics (cyclomatic complexity, coupling, etc.)

### Incremental Commit Strategy
- When to commit (after each increment, after each step)
- Commit message format for refactoring
- Branch strategy (refactor on branch, merge when complete)

### Quality Assessment
- How to measure improvement (manual review, metrics, code smells)
- What quality dimensions to prioritize (clarity, modularity, performance)
- Acceptable tradeoffs (clarity vs. performance, modularity vs. simplicity)

---

## Example Usage

**User:** "Refactor the UserService class — it's too complex and hard to understand"

**Workflow execution:**

1. **Orient** — Read UserService, identify:
   - 500 lines, multiple responsibilities (auth, profile, permissions)
   - Tests exist with 80% coverage
   - Violates Single Responsibility Principle
2. **Define scope:**
   - Goal: Split into separate services (AuthService, ProfileService, PermissionService)
   - Preserve: All existing behavior, API contracts
   - Success: All tests pass, each service < 200 lines
3. **Validate scope** — Tests exist to verify behavior → PASS
4. **Design approach:**
   - Step 1: Extract auth methods to AuthService
   - Step 2: Update UserService to delegate to AuthService
   - Step 3: Extract profile methods to ProfileService
   - Step 4: Update UserService to delegate to ProfileService
   - Step 5: Extract permission methods to PermissionService
   - Step 6: Update UserService to be orchestrator only
   - Run tests after each step
5. **Validate design** — Plan preserves behavior incrementally → PASS
6. **Implement incrementally:**
   - Create AuthService, extract methods → tests pass → commit
   - Update UserService to use AuthService → tests pass → commit
   - Create ProfileService, extract methods → tests pass → commit
   - Update UserService to use ProfileService → tests pass → commit
   - Create PermissionService, extract methods → tests pass → commit
   - Update UserService to orchestrate → tests pass → commit
7. **Validate refactoring:**
   - All tests pass ✓
   - Each service < 200 lines ✓
   - Clear responsibilities ✓
   - → PASS

**Outcome:** UserService refactored into modular services, all tests pass, behavior preserved.

---

## Best Practices

- **Tests first** — Can't safely refactor without tests
- **Small increments** — Catch regressions immediately
- **Run tests constantly** — After every increment
- **Commit frequently** — Can rollback to any passing state
- **Don't add features** — Refactoring changes structure, not behavior
- **Preserve behavior** — If behavior needs to change, that's a different workflow

---

## Common Anti-Patterns

**Anti-Pattern 1: Refactoring without tests**
- ❌ "I'll refactor, then add tests"
- ✅ Add tests first, then refactor

**Anti-Pattern 2: Big-bang refactoring**
- ❌ "I'll rewrite the whole module, then run tests"
- ✅ Small increments, run tests after each

**Anti-Pattern 3: Mixing refactoring and features**
- ❌ "While refactoring, I'll add this new feature"
- ✅ Refactor first (behavior preservation), then add feature (behavior change)

**Anti-Pattern 4: Changing behavior "because it's wrong"**
- ❌ "This behavior seems wrong, I'll fix it while refactoring"
- ✅ That's a bug fix (use bugfix workflow), not refactoring

**Anti-Pattern 5: Premature optimization**
- ❌ "I'll make it faster while refactoring"
- ✅ Performance optimization changes behavior (timing); separate from refactoring

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [modularity.md](../protocols/modularity.md) — Principles for modular design
  - [project_quality.md](../protocols/project_quality.md) — Quality assessment framework
  - [doc_maintenance.md](../protocols/doc_maintenance.md) — When to update docs

---

## Related Primitives

This workflow composes these primitives in sequence:

1. [orient](../primitives/orient.md) — Understand code to refactor
2. [define](../primitives/define.md) — Define refactoring scope and preservation criteria
3. [validate](../primitives/validate.md) — Verify scope and behavior preservation
4. [design](../primitives/design.md) — Plan incremental refactoring steps
5. [implement](../primitives/implement.md) — Execute refactoring incrementally
6. [investigate](../primitives/investigate.md) — Diagnose issues if validation fails (optional)
