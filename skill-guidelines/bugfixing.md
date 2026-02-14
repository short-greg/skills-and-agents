# Bugfixing - Complete Skill Suite

Consolidated setup guide for systematic bug fixing skills: workflow orchestration and hypothesis-based implementation.

---

## Overview

The **Bugfixing Suite** provides a systematic, hypothesis-driven approach to debugging and fixing bugs. This guide contains setup instructions for all related skills in one place.

### Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `bugfix-workflow` | End-to-end debugging orchestration with hypothesis testing | Orchestrator |
| `bugfix-impl` | Fix bugs using hypothesis-based debugging with root cause analysis | Atomic |

### When to Use Each Skill

**Use `bugfix-workflow` when:**
- Starting bug investigation from scratch
- Want systematic hypothesis-driven debugging
- Need complete orchestration with review gates
- Unsure what debugging has been done (it detects and resumes)

**Use `bugfix-impl` when:**
- Have identified the root cause
- Ready to implement the minimal fix
- Want TDD approach (write failing test first)
- Implementing fix with regression testing

### How Skills Work Together

```
bugfix-workflow (orchestrator)
    │
    ├─► Phase 1: Reproduce Issue
    │       └─► Creates failing test
    │
    ├─► Phase 2: Generate Hypotheses
    │       └─► Creates 3+ potential root causes
    │
    ├─► Phase 3: Review Hypotheses (User approval gate)
    │
    ├─► Phase 4: Test Hypotheses
    │       └─► Systematically test each with debug instrumentation
    │
    ├─► Phase 5: Identify Root Cause
    │       └─► Analyze results and confirm actual cause
    │
    ├─► Phase 6: Review Root Cause (User approval gate)
    │
    ├─► Phase 7: Implement Fix
    │       └─► Calls bugfix-impl with minimal fix
    │
    └─► Phase 8: Verify Fix
            └─► Run tests, confirm issue resolved
```

---

## Phase 0: Repository Detection

Follow the standard repository detection process to understand your project structure.

### Reference: setup-guidelines/repo-detection.md

The repository detection process helps customize skills to your project. Key steps:

1. **Detect Spec Directory** - Where debugging documentation is stored
2. **Detect Tool Configuration** - AI tool's config directory
3. **Read Project Conventions** - Testing practices, debugging standards
4. **Check Existing Artifacts** - Review existing bug reports or hypotheses
5. **Analyze Testing Structure** - Understand test organization

### Bugfixing Suite Specific Checks

```bash
# Find spec directory
for dir in specs dev-docs docs/planning .docs planning docs/bugs; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# Find existing bug documentation
find ${SPEC_DIR} -name "*-hypotheses.md" -o -name "*-debug.md" 2>/dev/null
find ${SPEC_DIR} -name "*-bug.md" -o -name "*-issue.md" 2>/dev/null

# Check for test framework
ls -la tests/ test/ __tests__/ 2>/dev/null

# Identify test command
which pytest >/dev/null 2>&1 && echo "Testing framework: pytest"
[ -f "package.json" ] && grep -q "jest\|mocha\|vitest" package.json && echo "JS testing framework detected"

# Review project conventions
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null
```

### Detection Summary Template

```markdown
## Bugfixing Suite Detection Summary

- **Spec directory**: ${SPEC_DIR}
- **Tool config**: ${TOOL_CONFIG}
- **Document naming pattern**: [e.g., YYYY-MM-DD-bug-name-hypotheses.md]
- **Project type**: [language/framework]
- **Test location**: [detected test directory]
- **Test framework**: [e.g., pytest, jest, cargo test]
- **Existing bug reports**: [count]
- **Key debugging practices**: [summary]
```

**Gate:** Do not proceed until spec directory and tool config are identified.

---

## Phase 1: Prerequisites

Follow the standard prerequisites validation to ensure all required directories and access exist.

### Reference: setup-guidelines/prerequisites.md

Standard validation steps:

1. **Verify Spec Directory Exists** - Create if needed
2. **Verify Tool Skills Directory Exists** - Create base structure
3. **Test File Access** - Ensure read/write permissions
4. **Verify Project Documentation Access** - Can read conventions
5. **Identify Skill Consumers** - Who/what uses these skills

### Bugfixing Suite Specific Prerequisites

```bash
# Create spec directory if needed
mkdir -p ${SPEC_DIR}

# Create skill directories
mkdir -p ${TOOL_CONFIG}/skills/bugfix-workflow
mkdir -p ${TOOL_CONFIG}/skills/bugfix-impl

# Verify test directory exists
mkdir -p tests/

# Verify testing framework installed
pytest --version 2>/dev/null || echo "pytest not installed - run: pip install pytest"
npm list jest 2>/dev/null || echo "jest not installed (if JS project)"

# Verify access
touch ${SPEC_DIR}/.test-access && rm ${SPEC_DIR}/.test-access
touch ${TOOL_CONFIG}/skills/.test-access && rm ${TOOL_CONFIG}/skills/.test-access
```

### Prerequisites Checklist

```markdown
## Bugfixing Suite Prerequisites

- [ ] Spec directory exists and is writable: ${SPEC_DIR}
- [ ] Skill directories created under ${TOOL_CONFIG}/skills/
- [ ] Test directory exists
- [ ] Testing framework installed
- [ ] File access verified
- [ ] Project documentation accessible (CLAUDE.md or README.md)
```

**Gate:** Do not proceed until all directories exist and testing framework is available.

---

## Phase 2: Skill Definitions

Complete SKILL.md templates for all skills in the bugfixing suite.

---

## Skill 1: bugfix-workflow

**Purpose:** Systematic debugging workflow using hypothesis-based approach with review gates.

**File:** `${TOOL_CONFIG}/skills/bugfix-workflow/SKILL.md`

```markdown
---
name: bugfix-workflow
description: Hypothesis-based debugging workflow with systematic root cause analysis
disable-model-invocation: true
---

# Bugfix Workflow

Systematic debugging workflow using hypothesis-driven investigation.

## Phase 0: Detect Current State

**Check if debugging already started:**

### Look for Hypotheses Document
Check ${SPEC_DIR} for existing debugging documentation:
- `*-hypotheses.md` files
- `*-debug.md` files

If exists:
- Read to see what's been tested
- Skip to appropriate phase based on progress

### Look for Bug Report
Check ${SPEC_DIR} for bug documentation:
- `*-bug.md` files
- `*-issue.md` files

If exists but no hypotheses:
- Start from Phase 1 (Reproduce)

**Output:** Starting phase

---

## Phase 1: Reproduce Issue

**Purpose:** Confirm the bug exists and is reproducible.

### Gather Information
Ask user:
- What is the expected behavior?
- What is the actual behavior?
- Steps to reproduce?
- Error messages or logs?

### Attempt Reproduction
Try to reproduce the issue:
1. Follow reproduction steps
2. Observe behavior
3. Collect error messages
4. Note any relevant context

### Create Failing Test
Write a test that demonstrates the bug:
```python
def test_bug_reproduction():
    """Test that reproduces the bug."""
    # Arrange
    setup_conditions()

    # Act
    result = trigger_bug()

    # Assert - This should fail until bug is fixed
    assert result == expected_behavior
```

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 BUGFIX-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Reproduce issue [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Generate hypotheses
⏸️ Phase 3: Review hypotheses
⏸️ Phase 4: Test hypotheses
⏸️ Phase 5: Identify root cause
⏸️ Phase 6: Review root cause
⏸️ Phase 7: Implement fix
⏸️ Phase 8: Verify fix

CURRENT TASK:
Phase 1: Reproducing issue and creating failing test
Status: Writing reproduction test
Started: [TIME]

CHECKLIST:
✅ Gather reproduction steps
✅ Attempt to reproduce
🔲 Create failing test
🔲 Verify test fails consistently
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Confirmed reproduction steps and failing test

**Gate:** Bug must be reproducible before proceeding.

---

## Phase 2: Generate Hypotheses

**Purpose:** Create 3+ potential root causes.

Based on the bug symptoms, generate hypotheses:

### Hypothesis Format
```markdown
## Hypothesis 1: [Name]
**Theory:** [What might be causing this]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute this]

## Hypothesis 2: [Name]
**Theory:** [Different potential cause]
**Test:** [How to verify]
**Evidence Needed:** [What confirms/refutes]

## Hypothesis 3: [Name]
**Theory:** [Another potential cause]
**Test:** [How to verify]
**Evidence Needed:** [What confirms/refutes]
```

**Requirements:**
- At least 3 distinct hypotheses
- Each with a testable prediction
- Cover different potential root causes (not variations of same idea)

**Save to:** `${SPEC_DIR}/YYYY-MM-DD-bug-name-hypotheses.md`

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
🔄 Phase 2: Generate hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Review hypotheses
...

CURRENT TASK:
Phase 2: Generating hypotheses for root cause
Status: Created 3 distinct hypotheses
Started: [TIME]

CHECKLIST:
✅ Hypothesis 1: Invalid state handling
✅ Hypothesis 2: Race condition in async code
✅ Hypothesis 3: Missing error boundary
🔲 Ensure hypotheses are distinct
🔲 Verify each is testable
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Hypotheses document in ${SPEC_DIR}/

---

## Phase 3: Review Hypotheses

**User Review Gate**

Present hypotheses to user:
1. Read each hypothesis
2. Explain testing strategy for each
3. Ask: "Do these hypotheses cover the likely causes?"

**If user wants to add/modify:**
- Update hypotheses document
- Return to this phase

**If user approves:**
- Proceed to Phase 4

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
🔄 Phase 3: Review hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Test hypotheses
...

CURRENT TASK:
Phase 3: User review of hypotheses
Status: Waiting for user approval
Started: [TIME]

REVIEW CHECKLIST:
✅ All hypotheses are distinct
✅ Each hypothesis is testable
✅ Cover different potential causes
🔲 User approval received
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 4: Test Hypotheses

**Purpose:** Systematically test each hypothesis.

For each hypothesis:

### Add Debug Instrumentation
- Add print statements
- Add logging
- Add assertions
- Create minimal reproducer

### Run Tests
- Execute code with instrumentation
- Collect output
- Analyze results

### Document Findings
```markdown
## Hypothesis 1: [Name]
**Test Results:** [What happened]
**Evidence:** [Logs, outputs, observations]
**Status:** CONFIRMED / REFUTED / INCONCLUSIVE

## Hypothesis 2: [Name]
**Test Results:** [What happened]
**Evidence:** [Logs, outputs, observations]
**Status:** CONFIRMED / REFUTED / INCONCLUSIVE

## Hypothesis 3: [Name]
**Test Results:** [What happened]
**Evidence:** [Logs, outputs, observations]
**Status:** CONFIRMED / REFUTED / INCONCLUSIVE
```

**IMPORTANT:** Test ALL hypotheses systematically - don't skip to the "likely" one.

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
✅ Phase 3: Review hypotheses
🔄 Phase 4: Test hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Identify root cause
⏸️ Phase 6: Review root cause
⏸️ Phase 7: Implement fix
⏸️ Phase 8: Verify fix

CURRENT TASK:
Phase 4: Testing hypotheses systematically
Status: Testing hypothesis 2 of 3
Started: [TIME]

TESTING PROGRESS:
✅ Hypothesis 1: REFUTED (async timing correct)
🔄 Hypothesis 2: Testing (added logging to state handler)
⏸️ Hypothesis 3: Not yet tested

CHECKLIST:
✅ Test hypothesis 1
🔲 Test hypothesis 2
🔲 Test hypothesis 3
🔲 Document all evidence
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Test results for each hypothesis with evidence

---

## Phase 5: Identify Root Cause

**Purpose:** Synthesize findings to identify the actual cause.

Based on test results:
1. Which hypothesis was confirmed?
2. What is the underlying issue?
3. Why did this happen?
4. What is the minimal fix?

**Present findings:**
```markdown
## Root Cause Analysis

**Confirmed Hypothesis:** [Which one and why]

**Root Cause:** [The actual underlying problem]

**Why It Happened:** [Context, explanation, history]

**Proposed Minimal Fix:** [What needs to change]

**Test Strategy:** [How to verify the fix works]
```

**Save to:** `${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md`

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
✅ Phase 3: Review hypotheses
✅ Phase 4: Test hypotheses
🔄 Phase 5: Identify root cause [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Review root cause
⏸️ Phase 7: Implement fix
⏸️ Phase 8: Verify fix

CURRENT TASK:
Phase 5: Analyzing test results to identify root cause
Status: Confirmed Hypothesis 2 - invalid state handling
Started: [TIME]

ANALYSIS:
✅ Hypothesis 1: REFUTED
✅ Hypothesis 2: CONFIRMED (state not validated on update)
✅ Hypothesis 3: REFUTED
✅ Root cause identified
🔲 Minimal fix proposed
🔲 Test strategy defined
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Root cause analysis document

---

## Phase 6: Review Root Cause

**User Review Gate**

Present root cause analysis:
1. Explain the confirmed hypothesis
2. Show evidence from testing
3. Propose the minimal fix
4. Ask: "Does this root cause make sense? Approve the fix approach?"

**If user disagrees:**
- May need to test more hypotheses
- Return to Phase 4

**If user approves:**
- Proceed to Phase 7

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
✅ Phase 3: Review hypotheses
✅ Phase 4: Test hypotheses
✅ Phase 5: Identify root cause
🔄 Phase 6: Review root cause [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 7: Implement fix
⏸️ Phase 8: Verify fix

CURRENT TASK:
Phase 6: User review of root cause analysis
Status: Waiting for approval of fix approach
Started: [TIME]

REVIEW SUMMARY:
✅ Root cause: Invalid state not validated on update
✅ Evidence: Logs show state transitions without validation
✅ Proposed fix: Add validation before state update
🔲 User approval received
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 7: Implement Fix

**Execute the minimal fix:**

```
/bugfix-impl ${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md
```

Follow hypothesis-based implementation:
1. Implement minimal fix
2. Add regression test
3. Verify fix works
4. Ensure no side effects

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
✅ Phase 3: Review hypotheses
✅ Phase 4: Test hypotheses
✅ Phase 5: Identify root cause
✅ Phase 6: Review root cause
🔄 Phase 7: Implement fix [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 8: Verify fix

CURRENT TASK:
Phase 7: Implementing minimal fix via bugfix-impl
Status: Running /bugfix-impl
Started: [TIME]

IMPLEMENTATION PROGRESS:
🔄 Delegated to bugfix-impl skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Bug fixed with passing tests

---

## Phase 8: Verify Fix

**Purpose:** Confirm the issue is resolved.

### Run Full Test Suite
```bash
pytest tests/
```

### Manual Verification
- Reproduce original issue
- Confirm it no longer occurs
- Test edge cases

### Present Results
```
Fix Verification:
✓ Original issue resolved
✓ All tests passing
✓ No regressions detected
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce issue
✅ Phase 2: Generate hypotheses
✅ Phase 3: Review hypotheses
✅ Phase 4: Test hypotheses
✅ Phase 5: Identify root cause
✅ Phase 6: Review root cause
✅ Phase 7: Implement fix
🔄 Phase 8: Verify fix [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 8: Final verification of bug fix
Status: Running full test suite
Started: [TIME]

VERIFICATION:
✅ Original reproduction test now passes
✅ Regression test passes
✅ Full test suite passes (42/42)
✅ Manual verification successful
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Verified bug fix

---

## Completion

When all phases complete:

```markdown
## Bugfix Complete ✅

**Bug:** [Description]
**Root Cause:** [Confirmed cause]
**Fix:** [What was changed]

### Debugging Process
- Hypotheses tested: 3
- Confirmed hypothesis: [Name]
- Evidence: [Summary]

### Quality Metrics
- Original test: ✅ Now passes
- Regression test: ✅ Added and passes
- Full test suite: ✅ 42/42 passed
- Manual verification: ✅ Complete

### Files Changed
**Modified:**
- [List of modified files]

**Created:**
- tests/test_bug_regression.py

### Documentation
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-hypotheses.md
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md

**Bug is fixed and verified.**
```

---

## Notes

- Always generate at least 3 hypotheses
- Test systematically - don't skip to a "likely" cause
- Document all findings for future reference
- Minimal fix is preferred over large refactors
- Root cause document serves as historical record
```

---

## Skill 2: bugfix-impl

**Purpose:** Fix bugs using hypothesis-based debugging with systematic root cause analysis.

**File:** `${TOOL_CONFIG}/skills/bugfix-impl/SKILL.md`

```markdown
---
name: bugfix-impl
description: Hypothesis-based debugging with systematic root cause analysis
disable-model-invocation: true
argument-hint: "[bug description or root cause file]"
---

# Bugfix Implementation

Fix bugs using hypothesis-based debugging with systematic testing and root cause analysis.

## When to Use

Use when:
- Have a bug to fix
- Need systematic debugging approach
- Want hypothesis-driven investigation
- Need root cause documentation

Do NOT use when:
- Building new features (use feature-impl)
- Refactoring code (use refactor-impl)
- Root cause is already known (just implement fix)

## Input

- Bug description (text)
- OR path to root cause analysis file
- OR issue number/URL

## Output

- Bug fix with regression test
- Root cause documentation
- All tests passing
- No new regressions

## Process

## Goal

Fix bugs with minimal changes, strong evidence-based root cause analysis, and comprehensive regression prevention.

## OKRs

**Objective:** Deliver verified bug fixes that resolve root causes without introducing regressions

**Key Results:**
- KR1: Root cause correctly identified with concrete evidence from hypothesis testing
- KR2: Minimal fix applied (no refactoring, no scope creep, targeted change only)
- KR3: Regression test added and passing (bug cannot recur)
- KR4: Full test suite passes (no new failures introduced)
- KR5: Bug analysis documentation complete (all hypotheses tested, evidence documented)

## Evaluation Criteria

This skill is **complete** when ALL of the following are verified:

**Correctness:**
- [ ] Original reproduction test now PASSES: `pytest tests/test_bug.py::test_bug_reproduction -v`
- [ ] Regression test created and PASSES: Test prevents bug from recurring
- [ ] Full test suite PASSES: No new failures introduced
- [ ] Bug no longer reproducible: Manual verification confirms fix

**Minimal Fix Verification:**
- [ ] Only root cause addressed: No refactoring performed
- [ ] No scope creep: No additional features or improvements
- [ ] Targeted change: Minimal lines of code modified
- [ ] No unrelated changes: `git diff` shows only bug-related modifications

**Evidence Quality:**
- [ ] All hypotheses tested: At least 3 distinct hypotheses generated and tested
- [ ] Hypothesis testing documented: Evidence collected for each (confirmed/refuted)
- [ ] Root cause clear: Analysis document explains WHY bug occurred, not just WHAT
- [ ] Evidence scratchpad complete: Documented in `${SPEC_DIR}/*-evidence.md`

**Completeness:**
- [ ] All files documented: Analysis lists all modified files
- [ ] Test suite green: `pytest tests/` shows 100% pass rate
- [ ] No TODO/FIXME left: `grep -r "TODO\|FIXME" [modified files]` returns 0
- [ ] Documentation complete: Analysis saved to `${SPEC_DIR}/*-analysis.md`

**⚠️ Common Over-Evaluation Traps:**

**Trap: "Bug seems fixed" ≠ Regression test proves it**
- ❌ "Manual testing shows bug is gone" - Not verified without regression test
- ✅ Regression test written that would FAIL if bug returns, currently PASSES

**Trap: "Fixed the symptom" ≠ Fixed the root cause**
- ❌ "Added null check and it works now" - Did you test WHY it was null?
- ✅ All hypotheses tested with evidence, confirmed root cause addressed

**Trap: "Tests pass" ≠ Full test suite passes with no regressions**
- ❌ "The new test passes" - Did you run the ENTIRE test suite?
- ✅ Full test suite passes: `pytest tests/` shows all tests green

**Trap: "Made improvements while fixing" ≠ Minimal fix**
- ❌ "Fixed bug and refactored the module for better readability"
- ✅ ONLY the bug fix changed, no refactoring or improvements

**Trap: "Looks like a race condition" ≠ Evidence-based diagnosis**
- ❌ "It's probably a race condition" - Did you test this hypothesis?
- ✅ Multiple hypotheses tested, race condition confirmed/refuted with evidence

### Phase 1: Reproduce Bug

**Purpose:** Confirm the bug exists and create a failing test that demonstrates it.

**Steps:**

1. **Gather Information**
   Ask user or read bug report:
   - What is the expected behavior?
   - What is the actual behavior?
   - Steps to reproduce?
   - Error messages or logs?

2. **Create Reproduction Steps**
   Document exact steps to trigger the bug:
   ```markdown
   ## Reproduction Steps
   1. [Step 1]
   2. [Step 2]
   3. [Expected: X, Actual: Y]
   ```

3. **Write Failing Test**
   Create a test that captures the bug:
   ```python
   def test_bug_reproduction():
       """
       Test that reproduces bug: [description]

       Expected: [correct behavior]
       Actual: [buggy behavior]
       """
       # Arrange
       setup = create_test_setup()

       # Act
       result = trigger_buggy_behavior(setup)

       # Assert - This will fail until bug is fixed
       assert result == expected_correct_behavior
   ```

4. **Verify Test Fails Consistently**
   Run the test multiple times:
   ```bash
   pytest tests/test_bug.py::test_bug_reproduction -v
   ```

   Expected: Test fails every time with same error.

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 BUGFIX-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Reproduce bug [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Generate hypotheses
⏸️ Phase 3: Test hypotheses
⏸️ Phase 3.5: Analyze evidence scratchpad
⏸️ Phase 4: Identify root cause
⏸️ Phase 5: Implement minimal fix
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 1: Reproducing bug and creating failing test
Status: Writing reproduction test
Started: [TIME]

CHECKLIST:
✅ Gather bug information
✅ Document reproduction steps
🔲 Write failing test
🔲 Verify test fails consistently
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Failing test that reproduces the issue

**Gate:** Test must fail consistently before proceeding.

---

### Phase 2: Generate Hypotheses

**Purpose:** Create at least 3 distinct hypotheses about potential root causes.

**Requirements:**
- At least 3 different hypotheses
- Each hypothesis should be testable
- Cover different potential causes (not variations of the same idea)

**Example format:**
```markdown
## Debugging Hypotheses

### Hypothesis 1: [Name]
**Theory:** [What might be causing this]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute]

### Hypothesis 2: [Name]
**Theory:** [Different potential cause]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute]

### Hypothesis 3: [Name]
**Theory:** [Another different potential cause]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute]
```

**Example (Python bug):**
```markdown
### Hypothesis 1: State Validation Missing
**Theory:** State updates don't validate input before applying changes
**Test:** Add logging before state update, check if validation is called
**Evidence Needed:** Logs showing state change without validation

### Hypothesis 2: Race Condition in Async Code
**Theory:** Concurrent state updates interfere with each other
**Test:** Add delays, run with single thread, check if bug disappears
**Evidence Needed:** Bug only occurs under concurrent load

### Hypothesis 3: Error Boundary Not Catching Exception
**Theory:** Exception is thrown but not properly caught
**Test:** Add try-catch with logging around suspected area
**Evidence Needed:** Uncaught exception in logs
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
🔄 Phase 2: Generate hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Test hypotheses
⏸️ Phase 3.5: Analyze evidence scratchpad
⏸️ Phase 4: Identify root cause
⏸️ Phase 5: Implement minimal fix
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 2: Generating testable hypotheses for root cause
Status: Created 3 distinct hypotheses
Started: [TIME]

HYPOTHESES:
✅ Hypothesis 1: State validation missing
✅ Hypothesis 2: Race condition in async code
✅ Hypothesis 3: Error boundary not catching exception
🔲 Verify hypotheses are distinct
🔲 Verify each is testable
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Document with 3+ testable hypotheses

---

### Phase 3: Test Hypotheses Systematically

**Purpose:** Test each hypothesis using debug instrumentation.

**For each hypothesis:**

1. **Add Debug Instrumentation**
   - Print statements at key points
   - Logging with relevant context
   - Assertions to check assumptions
   - Minimal reproducers

2. **Run Tests and Collect Data**
   - Execute code with instrumentation
   - Capture output and logs
   - Note unexpected behavior
   - Record timing information

3. **Document Findings**
   ```markdown
   ### Hypothesis 1: State Validation Missing
   **Test Results:** Added logging before state update. Logs show state
   changes being applied without any validation calls.
   **Evidence:**
   ```
   [2024-02-14 10:30:15] Updating state: old=valid, new=invalid
   [2024-02-14 10:30:15] No validation check performed
   [2024-02-14 10:30:15] State updated successfully
   ```
   **Status:** CONFIRMED - validation is indeed missing

   ### Hypothesis 2: Race Condition in Async Code
   **Test Results:** Ran with single thread. Bug still occurs.
   **Evidence:** Bug reproduces even without concurrency
   **Status:** REFUTED - not a race condition

   ### Hypothesis 3: Error Boundary Not Catching Exception
   **Test Results:** Added try-catch. No exception is thrown.
   **Evidence:** Code executes without exceptions
   **Status:** REFUTED - not an error handling issue
   ```

**IMPORTANT:** Test ALL hypotheses systematically - don't skip to the "likely" one.

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
🔄 Phase 3: Test hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3.5: Analyze evidence scratchpad
⏸️ Phase 4: Identify root cause
⏸️ Phase 5: Implement minimal fix
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 3: Testing hypotheses systematically
Status: Testing hypothesis 2 of 3
Started: [TIME]

TESTING PROGRESS:
✅ Hypothesis 1: CONFIRMED (validation missing)
🔄 Hypothesis 2: Testing (running single-threaded)
⏸️ Hypothesis 3: Not yet tested

EVIDENCE COLLECTED:
- Logs showing state changes without validation
- Single-threaded test still reproducing bug
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Test results for each hypothesis with evidence

---

### Phase 3.5: Analyze Evidence Scratchpad

**Purpose:** Document evidence from hypothesis testing BEFORE jumping to conclusions about root cause.

**Create:** `${SPEC_DIR}/YYYY-MM-DD-bug-name-evidence.md`

**Template:**

````markdown
# Evidence Analysis: [Bug Name]

**Date:** YYYY-MM-DD
**Bug Report:** [Link or description]
**Hypotheses Document:** ${SPEC_DIR}/YYYY-MM-DD-bug-name-hypotheses.md

## Hypothesis Testing Results

| Hypothesis | Status | Evidence Summary |
|------------|--------|------------------|
| H1: [Name] | ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE | [One-line summary] |
| H2: [Name] | ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE | [One-line summary] |
| H3: [Name] | ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE | [One-line summary] |

## Detailed Evidence

### Hypothesis 1: [Name]

**Test Performed:**
[What instrumentation/test was added]

**Results:**
```
[Logs, output, observations]
```

**Analysis:**
[What this evidence tells us]

**Conclusion:** ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE

**Reasoning:**
[Why we reached this conclusion]

---

### Hypothesis 2: [Name]

**Test Performed:**
[What instrumentation/test was added]

**Results:**
```
[Logs, output, observations]
```

**Analysis:**
[What this evidence tells us]

**Conclusion:** ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE

**Reasoning:**
[Why we reached this conclusion]

---

### Hypothesis 3: [Name]

**Test Performed:**
[What instrumentation/test was added]

**Results:**
```
[Logs, output, observations]
```

**Analysis:**
[What this evidence tells us]

**Conclusion:** ✅ CONFIRMED / ❌ REFUTED / ⚠️ INCONCLUSIVE

**Reasoning:**
[Why we reached this conclusion]

---

## Root Cause Analysis

**Confirmed Hypothesis:** [Which hypothesis was confirmed]

**Evidence Supporting Root Cause:**
- [Evidence point 1]
- [Evidence point 2]
- [Evidence point 3]

**Why Other Hypotheses Were Ruled Out:**
- H1: [Brief reason it was refuted]
- H2: [Brief reason it was refuted]

**Root Cause Statement:**
[Clear statement of the actual underlying problem, not just symptoms]

**Why This Bug Occurred:**
[Context, history, explanation - helps prevent similar bugs]

## Minimal Fix Approach

**Proposed Change:**
[Describe the minimal change that will fix the root cause]

**Files to Modify:**
- path/to/file1.py - [What change]
- path/to/file2.py - [What change]

**Why This Is Minimal:**
[Explain why this is the smallest change that fixes the root cause]

**What We're NOT Changing:**
[Explicitly list things we could improve but won't touch during bug fix]

## Self-Review: Cause vs Symptom

**Question:** Am I fixing the root cause or just the symptom?

**Answer:**
[Explain how the proposed fix addresses the underlying cause, not just the visible symptom]

**Evidence:**
[Reference specific evidence from hypothesis testing that confirms this is the root cause]

## Regression Test Strategy

**Test Name:** test_[descriptive_name]

**What It Tests:**
[What specific condition/behavior the regression test verifies]

**How It Would Fail If Bug Returns:**
[Describe what would happen if someone reintroduced this bug]

**Test Implementation Approach:**
```python
def test_regression_bug_name():
    """
    Regression test for: [Bug description]

    Ensures: [What this prevents from recurring]
    """
    # Arrange
    [Setup that triggers the bug condition]

    # Act
    [Action that would trigger bug if it existed]

    # Assert
    [Verification that bug doesn't occur]
```
````

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
🔄 Phase 3.5: Analyze evidence scratchpad [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Identify root cause
⏸️ Phase 5: Implement minimal fix
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 3.5: Creating evidence analysis scratchpad
Status: Documenting hypothesis test results before jumping to fix
Started: [TIME]

CHECKLIST:
✅ Document all hypothesis test results
✅ Analyze evidence for each hypothesis
🔲 Identify confirmed root cause with evidence
🔲 Propose minimal fix approach
🔲 Self-review: cause vs symptom
🔲 Plan regression test strategy
```

**Gate:** Evidence scratchpad complete with clear root cause identified

---

### Phase 4: Identify Root Cause

**Purpose:** Synthesize findings to identify the actual root cause.

**Analysis:**
- Which hypothesis was confirmed by evidence?
- What is the underlying issue?
- Why did this happen (not just how)?
- What is the minimal fix?

**Document root cause:**
```markdown
## Root Cause Analysis

**Confirmed Hypothesis:** Hypothesis 1 - State Validation Missing

**Root Cause:**
The state update function in `state_manager.py` applies new state values
directly without validating that the new state is valid. This allows
invalid states to be set, causing downstream errors.

**Why It Happened:**
The original implementation assumed all state updates would come from
trusted internal sources. When we added the API endpoint in v2.0, external
input started flowing through the same update function without validation.

**Proposed Minimal Fix:**
Add validation check in `StateManager.update_state()` before applying changes:
```python
def update_state(self, new_state):
    # Add this validation
    if not self.validate_state(new_state):
        raise InvalidStateError(f"Invalid state: {new_state}")

    # Existing code
    self._state = new_state
```

**Test Strategy:**
1. Original reproduction test should pass
2. Add regression test for invalid state rejection
3. Run full test suite to ensure no side effects
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
✅ Phase 3.5: Analyze evidence scratchpad
🔄 Phase 4: Identify root cause [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Implement minimal fix
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 4: Finalizing root cause from evidence analysis
Status: Confirmed missing state validation from scratchpad
Started: [TIME]

ANALYSIS:
✅ Evidence scratchpad reviewed
✅ Hypothesis 1: CONFIRMED (with evidence)
✅ Hypothesis 2: REFUTED (with evidence)
✅ Hypothesis 3: REFUTED (with evidence)
✅ Root cause identified with supporting evidence
✅ Minimal fix proposed
✅ Test strategy defined
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Root cause analysis document (based on evidence scratchpad)

---

### Phase 5: Implement Minimal Fix

**Purpose:** Implement the smallest change that fixes the root cause.

**Principles:**
- Fix the root cause, not symptoms
- Minimal change - don't refactor or add features
- Preserve existing behavior elsewhere
- Add regression test

**Steps:**

1. **Implement the Fix**
   Based on root cause analysis:
   ```python
   # state_manager.py

   def update_state(self, new_state):
       """Update state with validation."""
       # NEW: Add validation before update
       if not self.validate_state(new_state):
           raise InvalidStateError(
               f"Invalid state transition: {self._state} -> {new_state}"
           )

       # EXISTING: Apply state change (unchanged)
       self._state = new_state
       self._notify_observers()
   ```

2. **Ensure Original Test Passes**
   Run the reproduction test:
   ```bash
   pytest tests/test_bug.py::test_bug_reproduction -v
   ```
   Expected: Test now passes.

3. **Add Regression Test**
   Create test to prevent bug from returning:
   ```python
   def test_state_update_validates_input():
       """
       Regression test for bug #123: Invalid state allowed.

       Ensures state updates are validated before being applied.
       """
       manager = StateManager()
       manager.set_state("valid")

       # Invalid state should be rejected
       with pytest.raises(InvalidStateError):
           manager.update_state("invalid")

       # State should remain unchanged
       assert manager.get_state() == "valid"
   ```

4. **Run All Related Tests**
   ```bash
   pytest tests/test_state_manager.py -v
   ```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
✅ Phase 3.5: Analyze evidence scratchpad
✅ Phase 4: Identify root cause
🔄 Phase 5: Implement minimal fix [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Regression testing
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 5: Implementing minimal fix for root cause
Status: Adding validation to state update
Started: [TIME]

IMPLEMENTATION:
✅ Add validation check
✅ Original reproduction test passes
✅ Regression test created
🔲 Run full test suite
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Bug fixed with minimal changes

---

### Phase 6: Regression Testing

**Purpose:** Verify the fix doesn't break existing functionality.

**Run:**
```bash
# Full test suite
pytest tests/

# If project uses other test commands
npm test           # JavaScript
cargo test         # Rust
pytest --cov=.     # Python with coverage
```

**Check:**
- All existing tests still pass
- No new failures introduced
- Performance not degraded
- Coverage not decreased

**If any tests fail:**
1. Analyze the failure
2. Determine if it's a real regression or test needs update
3. Fix the issue
4. Re-run full test suite

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
✅ Phase 3.5: Analyze evidence scratchpad
✅ Phase 4: Identify root cause
✅ Phase 5: Implement minimal fix
🔄 Phase 6: Regression testing [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 7: Assess fix quality
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 6: Running full test suite for regressions
Status: All tests passing
Started: [TIME]

TEST RESULTS:
✅ Original bug test: PASS
✅ Regression test: PASS
✅ Full test suite: 42/42 PASS
✅ No new failures
✅ No performance degradation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Full test suite passing

---

### Phase 7: Assess Fix Quality

**Purpose:** Comprehensive quality assessment of the bug fix before marking complete.

**Assessment Types:**

#### 7.1: Correctness Assessment

**Verify the bug is actually fixed:**

```bash
# Run original reproduction test
pytest tests/test_bug.py::test_bug_reproduction -v

# Run regression test
pytest tests/test_bug_regression.py -v

# Run full test suite
pytest tests/ -v

# Manual verification (if applicable)
# [Describe manual steps to verify bug no longer occurs]
```

**Checklist:**
- [ ] Original reproduction test PASSES
- [ ] Regression test created and PASSES
- [ ] Full test suite PASSES (no new failures)
- [ ] Manual verification confirms bug resolved

**Output:**
```
Correctness Assessment:
✅ Original test passes: test_bug_reproduction PASSED
✅ Regression test passes: test_bug_regression PASSED
✅ Full test suite: 42/42 PASSED (no regressions)
✅ Manual verification: Bug no longer reproducible
```

**Gate:** All correctness checks pass

---

#### 7.2: Minimality Assessment

**Verify the fix is truly minimal (no refactoring or scope creep):**

```bash
# Check what changed
git diff HEAD

# Count lines changed
git diff --stat HEAD

# Check for refactoring patterns
git diff HEAD | grep -E "rename|move|reorganize"
```

**Checklist:**
- [ ] Only bug-related code changed (no refactoring)
- [ ] No scope creep (no additional features)
- [ ] Minimal lines changed (< 20 lines for most bugs)
- [ ] No unrelated file modifications

**Manual Review:**
Review `git diff` output and ask:
1. Does every change directly address the root cause?
2. Did I refactor anything while fixing the bug?
3. Did I "improve" code that wasn't related to the bug?
4. Could this fix be smaller?

**Output:**
```
Minimality Assessment:
✅ Lines changed: 8 lines (minimal)
✅ No refactoring detected
✅ No scope creep
✅ All changes directly address root cause
✅ Files modified: 2 (state_manager.py, test_state_manager.py)
```

**Gate:** Fix is minimal and targeted

---

#### 7.3: Evidence Quality Assessment

**Verify debugging process was systematic and well-documented:**

```bash
# Check evidence scratchpad exists
ls -la ${SPEC_DIR}/*-evidence.md

# Verify all hypotheses documented
grep "Hypothesis [0-9]:" ${SPEC_DIR}/*-evidence.md

# Check hypothesis testing results
grep "CONFIRMED\|REFUTED\|INCONCLUSIVE" ${SPEC_DIR}/*-evidence.md

# Verify root cause analysis exists
ls -la ${SPEC_DIR}/*-root-cause.md
```

**Checklist:**
- [ ] Evidence scratchpad complete (Phase 3.5)
- [ ] At least 3 hypotheses tested
- [ ] Each hypothesis has documented evidence
- [ ] Confirmed hypothesis clearly identified
- [ ] Root cause explains WHY, not just WHAT

**Manual Review:**
Review evidence documents and ask:
1. Did I test all hypotheses or jump to the "obvious" one?
2. Is the evidence concrete (logs, outputs) or vague ("seems like")?
3. Does root cause explain the underlying problem?
4. Could someone else understand why this bug happened?

**Output:**
```
Evidence Quality Assessment:
✅ Evidence scratchpad: ${SPEC_DIR}/2024-02-14-state-bug-evidence.md
✅ Hypotheses tested: 3 (all with evidence)
✅ Confirmed: H1 - State validation missing (logs show no validation calls)
✅ Refuted: H2 - Race condition (single-threaded test reproduced bug)
✅ Refuted: H3 - Error handling (no exceptions thrown)
✅ Root cause clear: External input bypasses validation added in v2.0
```

**Gate:** Evidence is thorough and systematic

---

#### 7.4: Completeness Assessment

**Verify nothing is left incomplete:**

```bash
# Check for TODO/FIXME in modified files
git diff --name-only HEAD | xargs grep -n "TODO\|FIXME\|XXX\|HACK"

# Verify all files documented in analysis
ls -la ${SPEC_DIR}/*-analysis.md

# Check git status
git status
```

**Checklist:**
- [ ] No TODO/FIXME left in modified code
- [ ] All modified files listed in analysis doc
- [ ] Test suite fully green
- [ ] Documentation complete

**Output:**
```
Completeness Assessment:
✅ No TODOs left in modified files
✅ Analysis document complete: ${SPEC_DIR}/2024-02-14-state-bug-analysis.md
✅ All files documented in analysis
✅ Test suite: 42/42 PASS
```

**Gate:** Implementation is complete

---

#### 7.5: Modularity Check (Optional)

**Reference:** `/Users/shortg/Development/skills-and-agents/skill-guidelines/modularity.md`

**Purpose:** Ensure the fix doesn't harm code modularity.

**When to perform this check:**
- Bug fix modified core modules
- Fix added new dependencies
- Concerned about code quality impact
- Optional for simple fixes

**Quick modularity check:**

```bash
# Check complexity didn't increase significantly
radon cc [modified_file.py] -a

# Check dependencies didn't increase
pydeps [modified_file.py] --show-deps

# Verify coupling is still reasonable
pylint --enable=R0904,R0902 [modified_file.py]
```

**Checklist:**
- [ ] Cyclomatic complexity: No functions >10 added/modified
- [ ] Dependencies: No new circular dependencies
- [ ] Coupling: Fix doesn't tightly couple unrelated modules
- [ ] Cohesion: Fix doesn't mix unrelated concerns

**Note:** This is a lightweight check, not full modularity assessment.
For full modularity assessment framework, see: skill-guidelines/modularity.md

**When to do full assessment:**
If this quick check reveals concerns, consider running the full 8-criteria
modularity assessment from modularity.md after the bug fix is complete.

**Output:**
```
Modularity Check:
✅ Complexity: validate_state() = 3 (low)
✅ No new dependencies added
✅ No coupling issues introduced
⚠️ Consider: Validation logic could be extracted to separate validator class (future refactoring)
```

**Gate:** Fix doesn't harm modularity (✅ or ⚠️ acceptable, ❌ requires revision)

---

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
✅ Phase 3.5: Analyze evidence scratchpad
✅ Phase 4: Identify root cause
✅ Phase 5: Implement minimal fix
✅ Phase 6: Regression testing
🔄 Phase 7: Assess fix quality [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 8: Document findings

CURRENT TASK:
Phase 7: Running comprehensive quality assessment
Status: Evaluating correctness, minimality, evidence quality
Started: [TIME]

ASSESSMENT RESULTS:
✅ 7.1 Correctness: All tests pass, bug resolved
✅ 7.2 Minimality: 8 lines changed, no refactoring
✅ 7.3 Evidence Quality: 3 hypotheses tested with evidence
✅ 7.4 Completeness: No TODOs, all docs complete
✅ 7.5 Modularity: No modularity issues introduced
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Comprehensive quality assessment complete

**Gate:** All quality assessments pass

---

### Phase 8: Document Findings

**Purpose:** Record the debugging process for future reference.

**Document:**

1. **Hypotheses Tested** (all of them)
   ```markdown
   ## Debugging Process

   ### Hypotheses Generated
   1. State validation missing - CONFIRMED
   2. Race condition in async code - REFUTED
   3. Error boundary not catching exception - REFUTED
   ```

2. **Evidence Collected**
   ```markdown
   ### Evidence
   - Logs showed state changes without validation calls
   - Single-threaded test still reproduced bug (ruled out race condition)
   - No exceptions thrown (ruled out error handling issue)
   ```

3. **Root Cause Identified**
   ```markdown
   ### Root Cause
   State update function applies changes without validation, allowing
   invalid states when external input is provided via API.
   ```

4. **Fix Implemented**
   ```markdown
   ### Fix
   Added validation check in StateManager.update_state() before applying
   state changes. Minimal fix - only added validation, no refactoring.

   Files changed:
   - state_manager.py (added validation)
   - tests/test_state_manager.py (added regression test)
   ```

**Why this matters:**
- Helps team understand the issue
- Prevents similar bugs in the future
- Shows systematic debugging approach
- Valuable for future debugging

**Save to:** `${SPEC_DIR}/YYYY-MM-DD-bug-name-analysis.md`

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
✅ Phase 3: Test hypotheses
✅ Phase 3.5: Analyze evidence scratchpad
✅ Phase 4: Identify root cause
✅ Phase 5: Implement minimal fix
✅ Phase 6: Regression testing
✅ Phase 7: Assess fix quality
🔄 Phase 8: Document findings [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 8: Documenting debugging process
Status: Creating analysis document
Started: [TIME]

DOCUMENTATION:
✅ Hypotheses documented (in evidence scratchpad)
✅ Evidence recorded (in evidence scratchpad)
✅ Root cause explained (in root cause doc)
✅ Fix described (in analysis doc)
✅ Quality assessment results documented
🔲 Save to ${SPEC_DIR}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Complete bug report with root cause documentation and quality assessment

---

## Completion

When all phases complete:

```markdown
## Bugfix Complete ✅

**Bug:** [Description]
**Root Cause:** State validation missing in update function
**Fix:** Added validation check before state updates

### Debugging Metrics
- Hypotheses generated: 3
- Hypotheses tested: 3
- Confirmed hypothesis: State validation missing
- Evidence: Logs + single-threaded test
- Evidence scratchpad: ${SPEC_DIR}/YYYY-MM-DD-bug-name-evidence.md

### Quality Metrics
- Original test: ✅ Now passes
- Regression test: ✅ Added and passes
- Full test suite: ✅ 42/42 passed
- Fix minimality: ✅ 8 lines changed, no refactoring
- Evidence quality: ✅ All hypotheses tested with concrete evidence
- Completeness: ✅ No TODOs, all docs complete
- Modularity: ✅ No modularity issues introduced

### Files Changed
**Modified:**
- state_manager.py (added validation - 5 lines)

**Created:**
- tests/test_state_manager.py::test_state_update_validates_input
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-evidence.md (Phase 3.5)
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md (Phase 4)
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-analysis.md (Phase 8)

### Assessment Results (Phase 7)
- 7.1 Correctness: ✅ All tests pass, bug resolved
- 7.2 Minimality: ✅ 8 lines changed, no refactoring
- 7.3 Evidence Quality: ✅ 3 hypotheses with concrete evidence
- 7.4 Completeness: ✅ All documentation complete
- 7.5 Modularity: ✅ No modularity harm

### Documentation
Complete debugging trail:
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-evidence.md (Evidence analysis)
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md (Root cause)
- ${SPEC_DIR}/YYYY-MM-DD-bug-name-analysis.md (Complete analysis)

**Bug is fixed, tested, assessed, and documented.**
```

---

## Notes

- **Always generate at least 3 hypotheses** - forces thorough thinking
- **Test systematically** - don't jump to conclusions
- **Use evidence scratchpad (Phase 3.5)** - document evidence BEFORE jumping to root cause
- **Minimal fix preferred** - resist urge to refactor
- **Assess fix quality (Phase 7)** - verify correctness, minimality, and evidence quality
- **Document everything** - helps prevent similar issues
- Root cause document is historical record, not just notes
- Reference modularity.md for optional post-fix modularity check

---

## Best Practices

### Hypothesis Generation
- Create at least 3 truly different hypotheses
- Don't create variations of the same idea
- Each should be independently testable
- Cover different categories (logic, concurrency, input validation, etc.)

### Systematic Testing
- Test ALL hypotheses, even "unlikely" ones
- Collect concrete evidence (logs, outputs, measurements)
- Don't skip to the "obvious" answer
- Document what DIDN'T work (helps future debugging)

### Evidence Analysis (Phase 3.5)
- Use evidence scratchpad to document findings BEFORE jumping to root cause
- Record concrete evidence for each hypothesis (not vague impressions)
- Clearly mark hypotheses as CONFIRMED/REFUTED/INCONCLUSIVE
- Self-review: "Am I fixing the cause or just the symptom?"
- Evidence scratchpad prevents premature conclusions

### Minimal Fixes
- Fix root cause, not symptoms
- Don't refactor while fixing bugs
- Don't add features while fixing bugs
- Keep changes small and focused
- Verify minimality in Phase 7.2 assessment

### Quality Assessment (Phase 7)
- Run comprehensive assessment before marking complete
- Verify correctness: all tests pass, bug resolved
- Verify minimality: no refactoring or scope creep
- Verify evidence quality: systematic hypothesis testing
- Optional modularity check: ensure fix doesn't harm code quality
- Don't skip quality gates

### Documentation
- Record all hypotheses tested (in evidence scratchpad)
- Show evidence for confirmed/refuted hypotheses
- Explain why the bug happened (not just what was wrong)
- Include lessons learned for future prevention
- Create complete debugging trail for historical reference
```

---

## Phase 3: Integration

### Complete Workflow Integration

The bugfixing skills form a cohesive workflow:

```
┌─────────────────────────────────────────────────────────────┐
│                   bugfix-workflow                            │
│                   (Orchestrator)                             │
└─────────────────────────────────────────────────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
┌─────────┐         ┌──────────┐         ┌──────────┐
│ Phase 0 │         │ Phase 1  │         │ Phase 2  │
│ Detect  │────────▶│Reproduce │────────▶│Generate  │
│ State   │         │ Issue    │         │Hypotheses│
└─────────┘         └──────────┘         └──────────┘
                                              │
                          ┌───────────────────┼───────────────┐
                          ▼                   ▼               ▼
                    ┌──────────┐        ┌─────────┐    ┌─────────┐
                    │ Phase 3  │        │ Phase 4 │    │ Phase 5 │
                    │  Review  │───────▶│  Test   │───▶│Identify │
                    │Hypotheses│        │Hypotheses│    │Root Cause│
                    └──────────┘        └─────────┘    └─────────┘
                                                             │
                          ┌──────────────────────────────────┤
                          ▼                                  ▼
                    ┌──────────┐                       ┌─────────┐
                    │ Phase 6  │                       │ Phase 7 │
                    │  Review  │──────────────────────▶│Implement│
                    │Root Cause│                       │  Fix    │
                    └──────────┘                       └─────────┘
                                                             │
                                                             ▼
                                                    ┌────────────────┐
                                                    │  bugfix-impl   │
                                                    │   (Atomic)     │
                                                    └────────────────┘
                                                             │
                                                             ▼
                                                       ┌─────────┐
                                                       │ Phase 8 │
                                                       │ Verify  │
                                                       │  Fix    │
                                                       └─────────┘
```

### State Detection Flow

The orchestrator detects existing debugging work and resumes appropriately:

```
User invokes: /bugfix-workflow

                    ┌─────────────┐
                    │   Detect    │
                    │   State     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
    ┌─────────┐      ┌─────────┐     ┌─────────┐
    │Hypotheses│      │Root     │     │Fix      │
    │exist?   │      │cause    │     │applied? │
    │         │      │exists?  │     │         │
    └────┬────┘      └────┬────┘     └────┬────┘
         │                │               │
    YES  │  NO       YES  │  NO      YES  │  NO
         │                │               │
    ┌────▼───┐       ┌────▼───┐      ┌────▼───┐
    │Resume  │       │Resume  │      │Resume  │
    │from    │       │from    │      │from    │
    │Phase 4 │       │Phase 7 │      │Phase 8 │
    │(Test)  │       │(Impl)  │      │(Verify)│
    └────────┘       └────────┘      └────────┘
         │                │               │
         └────────────────┴───────────────┘
                          │
                          ▼
                   ┌──────────────┐
                   │   Continue   │
                   │   Workflow   │
                   └──────────────┘
```

### Skill Composition Patterns

**Sequential Composition** (workflow calls bugfix-impl):
```
bugfix-workflow
  └─► Phase 1: Reproduce
        └─► Phase 2: Generate hypotheses
              └─► Phase 3: Review (user gate)
                    └─► Phase 4: Test hypotheses
                          └─► Phase 5: Identify root cause
                                └─► Phase 6: Review (user gate)
                                      └─► Phase 7: bugfix-impl
                                            └─► Produces: Fixed code + tests
```

**Standalone Usage** (direct bugfix-impl):
```
User: "Fix the bug where state validation is missing"
  └─► bugfix-impl
        └─► Generates hypotheses internally
              └─► Tests systematically
                    └─► Identifies root cause
                          └─► Implements minimal fix
                                └─► Produces: Fixed code + tests + documentation
```

### Data Flow

**Artifacts produced at each stage:**

| Phase | Skill | Input | Output |
|-------|-------|-------|--------|
| 1 | bugfix-workflow | Bug description | Failing test |
| 2 | bugfix-workflow | Failing test | `*-hypotheses.md` |
| 3 | User review | `*-hypotheses.md` | Approval |
| 4 | bugfix-workflow | Hypotheses | Test results |
| 5 | bugfix-workflow | Test results | `*-root-cause.md` |
| 6 | User review | `*-root-cause.md` | Approval |
| 7 | bugfix-impl | `*-root-cause.md` | Fixed code + tests |
| 8 | bugfix-workflow | Fixed code | Verification report |

**File naming conventions:**
- Hypotheses: `${SPEC_DIR}/YYYY-MM-DD-bug-name-hypotheses.md`
- Root cause: `${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md`
- Analysis: `${SPEC_DIR}/YYYY-MM-DD-bug-name-analysis.md`

---

## Phase 4: Testing & Validation

### Validation Strategy

Test each skill individually, then test the complete workflow.

### Test 1: bugfix-impl Standalone

**Scenario:** Fix a simple bug with clear root cause

**Steps:**
1. Create a bug in test code (e.g., missing validation)
2. Invoke: `/bugfix-impl "Function allows invalid input"`
3. Verify hypothesis generation (3+ hypotheses)
4. Check systematic testing of each hypothesis
5. Verify root cause identification
6. Check minimal fix implementation
7. Verify regression test added

**Expected Output:**
- At least 3 hypotheses generated
- All hypotheses tested with evidence
- Root cause correctly identified
- Minimal fix applied (no refactoring)
- Regression test created and passing
- Full test suite still passes
- Documentation created in ${SPEC_DIR}

### Test 2: bugfix-workflow Complete

**Scenario:** End-to-end bug investigation and fix

**Steps:**
1. Report a bug: `/bugfix-workflow`
2. Describe bug when prompted
3. Review hypotheses at gate (approve or request changes)
4. Review root cause at gate (approve fix approach)
5. Verify implementation completes
6. Check final verification

**Expected Output:**
- Hypotheses document created
- User review gates honored
- All hypotheses tested
- Root cause analysis created
- Fix implemented via bugfix-impl
- Full test suite passes
- Complete documentation trail

### Test 3: State Detection

**Scenario:** Resume from existing hypotheses

**Steps:**
1. Create hypotheses document manually
2. Invoke: `/bugfix-workflow`
3. Verify it detects existing hypotheses
4. Verify it resumes from Phase 4 (Test Hypotheses)

**Expected Output:**
- Workflow detects hypotheses exist
- Skips Phase 1-2 (already done)
- Starts from Phase 4 (Test Hypotheses)

### Test 4: Hypothesis Quality

**Scenario:** Verify systematic hypothesis testing

**Steps:**
1. Create a bug with specific root cause
2. Run bugfix-impl
3. Verify at least 3 truly different hypotheses
4. Verify all hypotheses tested (not just "likely" one)
5. Check evidence collected for each

**Expected Output:**
- 3+ distinct hypotheses (not variations)
- Evidence for CONFIRMED hypothesis
- Evidence for REFUTED hypotheses
- Clear documentation of what didn't work

### Test 5: Minimal Fix

**Scenario:** Verify fix is minimal, not refactoring

**Steps:**
1. Create bug in messy code (code that could use refactoring)
2. Run bugfix-impl or bugfix-workflow
3. Verify fix only addresses root cause
4. Check that no refactoring occurred

**Expected Output:**
- Only buggy code changed
- No "while we're here" improvements
- No style fixes or refactoring
- Messy code remains messy (except the bug)

---

## Phase 5: Best Practices

### Progress Tracking

**Why it matters:**
- Makes AI report what it's doing (less likely to skip steps)
- Provides audit trail of debugging process
- User can see where investigation is
- Can resume from any phase

**How to use:**
- ALL workflow and implementation skills include progress tracking
- Progress updates after each phase completion
- Shows current hypothesis being tested
- Displays evidence collected

**Example from bugfix-impl:**
```
PHASES:
✅ Phase 1: Reproduce bug
✅ Phase 2: Generate hypotheses
🔄 Phase 3: Test hypotheses [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Identify root cause

CURRENT TASK:
Phase 3: Testing hypotheses systematically
Status: Testing hypothesis 2 of 3
Started: [TIME]

TESTING PROGRESS:
✅ Hypothesis 1: REFUTED (logs show validation is called)
🔄 Hypothesis 2: Testing (added concurrency instrumentation)
⏸️ Hypothesis 3: Not yet tested
```

### Systematic Testing

**Why it matters:**
- Prevents jumping to wrong conclusions
- Creates complete debugging record
- Finds actual root cause, not symptoms
- Documents what doesn't work (valuable for future)

**How to use:**
- Generate at least 3 distinct hypotheses
- Test ALL hypotheses (even "unlikely" ones)
- Collect concrete evidence for each
- Document confirmed AND refuted hypotheses

**Anti-pattern:**
```
❌ "This is probably a race condition" → jumps to fix
✅ Generate 3 hypotheses including race condition → test all → confirm actual cause
```

### Minimal Fixes

**Why it matters:**
- Reduces risk of introducing new bugs
- Makes code review easier
- Faster to test and deploy
- Clearer what changed and why

**How to use:**
- Fix ONLY the root cause
- Don't refactor while fixing
- Don't add features while fixing
- Don't fix unrelated issues

**Examples:**
```
✅ GOOD: Add validation check where missing
❌ BAD: Add validation + refactor entire module + add new feature

✅ GOOD: Fix race condition with lock
❌ BAD: Fix race condition + improve performance + update dependencies

✅ GOOD: Handle null case correctly
❌ BAD: Handle null + rename variables + reorganize code
```

### Documentation

**Why it matters:**
- Helps team understand what happened
- Prevents similar bugs in future
- Shows systematic debugging approach
- Creates institutional knowledge

**What to document:**
- All hypotheses (not just confirmed one)
- Evidence for each hypothesis
- Why the bug happened (not just what)
- Minimal fix applied
- Lessons learned

**Required artifacts:**
- `*-hypotheses.md` - All hypotheses and test results
- `*-root-cause.md` - Confirmed cause and fix approach
- `*-analysis.md` - Complete debugging record

### Review Gates

**Purpose:**
- User validation of hypotheses before testing
- User approval of fix approach before implementation
- Catch issues in debugging approach early
- Ensure user understands root cause

**When to use:**
- After hypothesis generation (are these the right questions?)
- After root cause identification (is this the real problem?)

**Optional, not mandatory:**
- Users can skip gates if they trust the investigation
- Quick bugs may not need review gates
- Complex bugs benefit from validation

---

## Summary

### Skills Created

After following this guide, you will have:

- `${TOOL_CONFIG}/skills/bugfix-workflow/SKILL.md` - Orchestrator
- `${TOOL_CONFIG}/skills/bugfix-impl/SKILL.md` - Implementation

### Key Capabilities

**Systematic Debugging:**
- Hypothesis-driven investigation
- At least 3 distinct hypotheses per bug
- Systematic testing with evidence
- Root cause analysis

**Minimal Fixes:**
- Fix root cause, not symptoms
- No refactoring during bug fix
- Regression testing
- Full test suite validation

**Documentation:**
- Complete debugging record
- Evidence for all hypotheses
- Root cause explanation
- Lessons learned

**Orchestration:**
- State detection (resume from any point)
- Review gates (user validation)
- Progress tracking throughout
- Complete workflow management

### Workflow Patterns

**Complete Bugfix Workflow:**
```
Bug Report
  → Reproduce issue (failing test)
    → Generate hypotheses (3+)
      → Review gate
        → Test all hypotheses systematically
          → Identify root cause
            → Review gate
              → Implement minimal fix
                → Verify fix (regression test + full suite)
                  → Document findings
```

**State-Based Resume:**
```
Hypotheses exist → Resume from testing
Root cause identified → Resume from implementation
Fix applied → Resume from verification
```

### Next Steps

1. **Create skills** using templates in this guide
2. **Test individually** using validation scenarios
3. **Test complete workflow** end-to-end
4. **Customize** for your project conventions
5. **Document** any project-specific adaptations

### Related Guides

- **Feature Development:** For building new features with TDD
- **Refactoring:** For improving code quality
- **Code Review:** For automated review feedback

---

## Appendix: Quick Reference

### Invocation Commands

```bash
# Orchestrator (detects state, runs complete workflow)
/bugfix-workflow

# Direct implementation (when you know the bug)
/bugfix-impl "Bug description"
/bugfix-impl ${SPEC_DIR}/YYYY-MM-DD-bug-name-root-cause.md
```

### File Locations

```
${SPEC_DIR}/
├── YYYY-MM-DD-bug-name-hypotheses.md     # Hypotheses
├── YYYY-MM-DD-bug-name-root-cause.md     # Root cause
├── YYYY-MM-DD-bug-name-analysis.md       # Complete analysis
└── ...

${TOOL_CONFIG}/skills/
├── bugfix-workflow/SKILL.md
└── bugfix-impl/SKILL.md
```

### Decision Tree

```
Do you have a bug to fix?
├─ YES → Start with /bugfix-workflow or /bugfix-impl
└─ NO → Not a bug - use feature-impl or refactor-impl

Do you know the root cause?
├─ YES → Use /bugfix-impl directly
└─ NO → Use /bugfix-workflow for investigation

Have you generated hypotheses?
├─ YES → Resume workflow from testing phase
└─ NO → Start workflow from beginning

Is the fix minimal?
├─ YES → Good, proceed with implementation
└─ NO → Review - are you refactoring or fixing?
```

### Hypothesis Quality Checklist

```
Good hypotheses are:
✅ Distinct (not variations of same idea)
✅ Testable (can collect evidence)
✅ Specific (clear what to check)
✅ Cover different categories (logic, concurrency, validation, etc.)

Bad hypotheses:
❌ All about same potential cause
❌ Untestable ("maybe it's just broken")
❌ Vague ("something is wrong with X")
❌ All about one category
```
