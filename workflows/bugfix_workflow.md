---
name: bugfix-workflow
description: >
  Use when fixing bugs systematically with hypothesis-driven investigation.
  Triggers on: "fix this bug", "debug this issue", "systematic bugfix".
argument-hint: "[bug description or reproduction steps]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Bugfix Workflow

**Goal:** Fix bugs systematically by reproducing, diagnosing root cause, implementing minimal fix, and preventing regression.

**Intent:** Prevent haphazard debugging where developers jump to conclusions, fix symptoms rather than causes, and introduce regressions. This workflow ensures root causes are identified through hypothesis-driven investigation and minimal fixes are applied with regression tests.

**Scope:** Complete bug investigation and fix. Includes: reproducing the bug with a failing test, generating and testing hypotheses about root causes, confirming the root cause, implementing a minimal fix, and validating the fix with regression tests. The workflow produces a verified fix with documented root cause analysis and regression prevention.

---

## Workflow Type

**Type:** Adaptive

**Style:** Imperative

**Uncertainty Handling:**
- If bug cannot be reproduced, invoke `investigate` to understand reproduction conditions
- If initial hypotheses don't explain symptoms, generate additional hypotheses
- If root cause is unclear after testing hypotheses, invoke `investigate` with instrumentation/logging
- If validation fails after fix, invoke `investigate` to determine if fix was incomplete or incorrect

**Iteration Handling:**
- Hypothesis generation and testing may iterate until root cause is confirmed
- Max 5 hypothesis iterations before escalating to user
- If fix validation fails, max 2 fix iterations before escalating

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand the codebase structure, identify relevant code areas, understand testing conventions

**Inputs:** Bug description or reproduction steps from user

**Outputs:**
- Project conventions identified (testing framework, debugging tools, logging patterns)
- Relevant code areas identified (likely files/modules involved)
- Context for where bug manifests

**Self-satisfiable:** Reads project documentation, relevant code, and conventions

### Step 2: Reproduce Bug

**Primitive:** investigate (focus: reproduction)

**Purpose:** Create a failing test that demonstrates the bug — proof that bug exists and criterion for knowing it's fixed

**Inputs:**
- Bug description from user
- Project context from Step 1

**Outputs:**
- Failing test (unit, integration, or end-to-end depending on bug scope)
- Reproduction steps documented
- Expected vs actual behavior clearly stated

**On failure to reproduce:**
- Gather more information from user about reproduction conditions
- Check environment-specific factors (OS, dependencies, configuration)
- Consider intermittent/timing issues

**Why this matters:** Can't fix what you can't reproduce. Failing test becomes the success criterion.

### Step 3: Generate Hypotheses

**Primitive:** brainstorm

**Purpose:** Generate potential root causes based on symptoms and codebase analysis

**Inputs:**
- Failing test from Step 2
- Bug symptoms and behavior
- Relevant code from Step 1

**Outputs:**
- List of hypotheses about root cause (ranked by likelihood)
- Evidence supporting each hypothesis
- Instrumentation strategy to test each hypothesis

**Hypothesis quality:**
- Specific and testable (not vague)
- Based on evidence (not pure speculation)
- Falsifiable (can prove wrong)

### Step 4: Investigate Root Cause

**Primitive:** investigate (focus: diagnosis)

**Purpose:** Test hypotheses through instrumentation, logging, and analysis until root cause is confirmed

**Inputs:**
- Hypotheses from Step 3
- Failing test from Step 2
- Codebase

**Outputs:**
- Confirmed root cause with evidence
- Hypothesis testing results (which hypotheses were ruled out and why)
- Understanding of why bug occurs

**Investigation techniques:**
- Add logging/print statements to trace execution
- Use debugger to inspect state
- Binary search through code history (git bisect)
- Analyze stack traces and error messages
- Check edge cases and boundary conditions

**Iteration:**
- Test most likely hypothesis first
- If hypothesis is ruled out, test next hypothesis
- If all hypotheses are ruled out, generate new hypotheses (loop back to Step 3)
- Max 5 hypothesis iterations before escalating

**Completion criteria:** Root cause is clearly identified with supporting evidence

### Step 5: Define Fix Scope

**Primitive:** define

**Purpose:** Define minimal fix scope — what should change and what should not

**Inputs:**
- Root cause from Step 4
- Failing test from Step 2

**Outputs:**
- Fix scope: specific files/functions/logic to change
- Fix constraints: what must NOT change (avoid overengineering)
- Success criteria: failing test passes, no regressions, minimal changes
- Out of scope: related improvements that aren't necessary for bug fix

**Why minimal?** Minimal fixes reduce regression risk and code review complexity.

### Step 6: Implement Fix

**Primitive:** implement

**Purpose:** Apply minimal fix that addresses root cause without introducing regressions

**Inputs:**
- Fix scope from Step 5
- Root cause from Step 4
- Failing test from Step 2

**Outputs:**
- Code changes fixing the bug
- Regression test (ensuring failing test now passes)
- Documentation update (if fix changes behavior or API)

**Implementation constraints:**
- Fix only the root cause (no "while I'm here" improvements)
- Preserve existing behavior except for the bug
- Add regression test to prevent recurrence

### Step 7: Validate Fix

**Primitive:** validate

**Purpose:** Confirm fix resolves the bug without introducing regressions

**Inputs:**
- Implementation from Step 6
- Failing test from Step 2 (should now pass)
- Success criteria from Step 5

**Outputs:**
- Pass/fail verdict
- Test results (failing test now passes, all other tests still pass)
- Regression analysis (no new bugs introduced)

**Validation criteria:**
1. Originally failing test now passes
2. All existing tests still pass (no regressions)
3. Fix addresses root cause (not just symptom)
4. Changes are minimal (only what's necessary)

**On failure:**
1. Invoke `investigate` to determine why fix didn't work
2. Determine if:
   - Fix was incomplete (incorrect understanding) → loop back to Step 4
   - Fix introduced regression → loop back to Step 6 with constraint to preserve behavior
3. Update checklist with remediation tasks
4. Re-run from loop-back step

**Iteration bounds:** Max 2 fix iterations before escalating to user

---

## Preconditions

**Must be provided:**
- bug description: what's wrong, what's expected — ask user for reproduction steps if not clear

**Self-satisfiable:**
- project context: read project docs and relevant code to understand structure
- testing setup: identify test framework and conventions

---

## Postconditions

**Success:**
- Bug is fixed and verified
- Root cause is documented
- Regression test prevents recurrence
- All existing tests still pass
- Changes are minimal

**Failure (blocked):**
- Bug cannot be reproduced (blocked at Step 2)
- Root cause cannot be identified (blocked at Step 4)
- Fix introduces regressions (blocked at Step 7)
- User aborts workflow

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify last completed step from trace
   - Check for partial work (failing tests, hypothesis docs, partial fixes)
   - Resume from appropriate point
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 2 (Reproduce):** If partial failing test exists, review and complete
- **Step 3 (Hypothesize):** If hypothesis list exists, read and continue
- **Step 4 (Investigate):** If instrumentation was added, review findings before continuing
- **Step 6 (Implement):** If partial fix exists:
  - Review changes for correctness
  - Check if fix addresses root cause
  - Complete or revise as needed
- **Step 7 (Validate):** Safe to re-run from beginning if interrupted

---

## On Validation Failure

**Validation failure triggers investigation and iteration:**

1. **Validate reports failure** — which criteria failed (regression, incomplete fix)
2. **Invoke investigate** — determine why fix didn't work:
   - "Fix addressed symptom not root cause" → loop to Step 4
   - "Fix correct but introduced regression" → loop to Step 6 with regression constraint
3. **Update trace** — record iteration: "Fix validation failed, root cause: [X], looping to Step [N]"
4. **Add remediation tasks** — update checklist with specific fixes
5. **Re-run from loop-back step**
6. **Re-validate**

**Iteration bounds:**
- Max 2 fix iterations before escalating to user
- If same regression recurs, escalate immediately

---

## Quality Gates

This workflow includes quality gates for root cause confirmation and fix validation:

| Step | Gate | Purpose |
|------|------|---------|
| Step 4 | Root cause confirmed | Verify hypothesis is proven before implementing fix |
| Step 7 | Validate fix | Verify bug is fixed and no regressions introduced |

**User approval gates:**
- After Step 4: User confirms root cause diagnosis (especially for critical bugs)
- After Step 7: User confirms fix is acceptable (optional)

---

## Hypothesis-Driven Investigation

This workflow uses a systematic hypothesis-driven approach:

```
Observe symptoms → Generate hypotheses → Test hypotheses → Confirm root cause → Fix
```

**Why hypothesis-driven?**
- Prevents jumping to conclusions
- Ensures systematic exploration of possibilities
- Provides audit trail of investigation
- Catches edge cases and subtle issues

**Hypothesis iteration:**
```
generate hypotheses (Step 3)
  ↓
test hypotheses (Step 4)
  ↓
root cause confirmed? → YES → proceed to Step 5
  ↓ NO
generate new hypotheses (loop to Step 3)
```

**Bounds:** Max 5 hypothesis iterations before escalating to user

---

## Customization Points

When adapting this workflow for your project:

### Repository Type

**Prototype:**
- Simplified reproduction (manual reproduction acceptable)
- Fewer hypothesis iterations before proceeding
- Regression test optional (but recommended)
- Documentation of root cause optional

**Production:**
- Full hypothesis-driven investigation required
- Automated regression test mandatory
- Root cause documentation required
- Code review required for fix

**Library:**
- Must consider API compatibility when fixing
- Regression test must cover public API
- Changelog entry required for behavioral changes
- May require deprecation notice if fix changes API

### Testing Conventions
- Test framework (pytest, jest, rspec, etc.)
- Test file naming and organization
- How to run tests (command, configuration)
- Regression test location (same file, separate suite)

### Debugging Tools
- Debugger setup (pdb, node inspect, gdb, etc.)
- Logging framework (Winston, log4j, Python logging)
- Profiling tools (if performance bug)
- Environment setup for reproduction

### Documentation Standards
- Where to document root cause (inline comments, separate doc, issue tracker)
- Bug fix commit message format
- Regression test documentation

### Fix Scope Guidelines
- When minimal fix is insufficient (security bugs, data integrity)
- When to include refactoring (if bug reveals structural issue)
- Review requirements for bug fixes

---

## Example Usage

**User:** "The login function crashes when email is null"

**Workflow execution:**

1. **Orient** — Read project, identify auth module, testing framework is pytest
2. **Reproduce** — Create failing test:
   ```python
   def test_login_with_null_email():
       result = login(email=None, password="test123")
       # Currently crashes with AttributeError
   ```
3. **Generate hypotheses**:
   - H1: Email validation missing (most likely)
   - H2: Database query doesn't handle null (possible)
   - H3: Email normalization assumes string (possible)
4. **Investigate** — Test H1: Add logging before email validation → confirms no validation
5. **Define fix scope** — Add email validation before processing, no other changes
6. **Implement** — Add validation:
   ```python
   def login(email, password):
       if email is None:
           return {"error": "Email required"}
       # ... rest of login logic
   ```
7. **Validate** — Failing test passes, all tests pass, no regressions → PASS

**Outcome:** Bug fixed with minimal change, root cause documented, regression prevented.

---

## Best Practices

- **Always reproduce first** — Can't fix what you can't reproduce
- **Don't skip hypothesis generation** — Jumping to solutions causes symptom fixes
- **Test hypotheses systematically** — Use evidence, not intuition
- **Minimal fixes** — Only change what's necessary to fix root cause
- **Regression tests are mandatory** — Prevent bug from returning
- **Document root cause** — Future maintainers benefit from your investigation

---

## Common Anti-Patterns

**Anti-Pattern 1: Guessing at the fix**
- ❌ "I think it's X, let me try changing it"
- ✅ Reproduce, hypothesize, test hypotheses, confirm root cause, then fix

**Anti-Pattern 2: Fixing symptoms**
- ❌ "The crash happens here, I'll add a try/catch"
- ✅ Find why the crash happens, fix the root cause

**Anti-Pattern 3: Over-engineering the fix**
- ❌ "While fixing this, I'll refactor this whole module"
- ✅ Minimal fix for the bug, refactor separately if needed

**Anti-Pattern 4: Skipping regression tests**
- ❌ "I fixed it and manual testing works"
- ✅ Add automated regression test to prevent recurrence

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [manage_complexity_uncertainty_risk.md](../protocols/manage_complexity_uncertainty_risk.md) — Handle investigation complexity
  - [project_quality.md](../protocols/project_quality.md) — Assess fix quality

---

## Related Primitives

This workflow composes these primitives in sequence:

1. [orient](../primitives/orient.md) — Understand codebase
2. [investigate](../primitives/investigate.md) — Reproduce bug, diagnose root cause
3. [brainstorm](../primitives/brainstorm.md) — Generate hypotheses
4. [define](../primitives/define.md) — Define minimal fix scope
5. [implement](../primitives/implement.md) — Apply fix
6. [validate](../primitives/validate.md) — Verify fix works and no regressions
