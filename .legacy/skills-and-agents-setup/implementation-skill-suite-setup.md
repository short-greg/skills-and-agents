# Feature Implementation Suite Setup

Setup instructions for skills that implement features using Test-Driven Development.

---

## Overview

The **Feature Implementation Suite** provides TDD-based implementation workflows with mandatory code review and comprehensive testing.

**This suite includes:**
- `feature-impl` - Implement features using TDD
- `bugfix-impl` - Fix bugs with root cause analysis
- `refactor-impl` - Refactor code with safety checks

**When to use:**
- After development plan is approved
- Implementing features with test coverage
- Bug fixes requiring root cause analysis
- Code refactoring with safety guarantees

**Output:** Working implementation with passing tests, code review, and validated acceptance criteria

---

## Phase 0: Repository Detection

Before setting up the implementation suite, detect and validate the repository structure.

### Step 1: Detect Spec Directory

```bash
# Find where development plans are stored
for dir in specs dev-docs docs/planning .docs planning; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done
```

**Verify plans exist:**
```bash
# Find existing development plans (inputs for implementation)
find ${SPEC_DIR} -name "*-plan.md" 2>/dev/null
```

### Step 2: Detect Test Directory

```bash
# Find where tests are located
for dir in tests test __tests__ spec; do
  if [ -d "$dir" ]; then
    echo "Found test directory: $dir"
    TEST_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$TEST_DIR" ]; then
  echo "Where should tests be stored? (e.g., tests/, test/)"
fi
```

### Step 3: Detect Testing Framework

```bash
# Check for testing framework
# Python
which pytest >/dev/null 2>&1 && echo "Testing framework: pytest"

# JavaScript
[ -f "package.json" ] && grep -q "jest\|mocha\|vitest" package.json && echo "Testing framework detected in package.json"

# Other languages
# ...
```

### Step 4: Read Project Conventions

```bash
# Read project documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - Code quality standards
# - Testing requirements
# - Coverage requirements
# - Commit message format
# - Code review checklist
```

### Step 5: Identify Framework

```bash
# Check if project uses a framework
ls framework/README.md 2>/dev/null && echo "Framework: framework/"

# Framework docs inform implementation patterns
```

**Output Phase 0 Summary:**
```markdown
## Repository Detection Summary

- **Spec directory**: specs/ [or detected location]
- **Test directory**: tests/ [or detected location]
- **Testing framework**: pytest [or detected]
- **Coverage requirement**: 80%+ [or from project conventions]
- **Tool config**: .claude/ [or detected location]
- **Framework**: framework/ [if applicable]
- **Existing plans**: 3 ready for implementation [or count]
```

**Gate:** Do not proceed until test directory exists and testing framework is identified.

---

## Phase 1: Validate Prerequisites

Ensure all prerequisites are met for TDD implementation.

### Step 1: Verify Test Directory Exists

```bash
# Create if doesn't exist
mkdir -p ${TEST_DIR}
```

### Step 2: Verify Testing Framework Installed

```bash
# Python example
pytest --version 2>/dev/null || echo "pytest not installed - run: pip install pytest"

# JavaScript example
npm list jest 2>/dev/null || echo "jest not installed - run: npm install --save-dev jest"
```

### Step 3: Verify Development Plans Available

```bash
# Test reading a plan
ls ${SPEC_DIR}/*-plan.md 2>/dev/null || echo "No plans found - create one with plan-dev first"
```

### Step 4: Run Existing Tests

```bash
# Verify existing tests pass before starting
pytest ${TEST_DIR}/ || echo "WARNING: Existing tests are failing"
```

### Step 5: Verify Tool Skills Directory

```bash
# Create skills directories
mkdir -p ${TOOL_CONFIG}/skills/feature-impl
mkdir -p ${TOOL_CONFIG}/skills/bugfix-impl
mkdir -p ${TOOL_CONFIG}/skills/refactor-impl
```

**Output Phase 1 Summary:**
```markdown
## Prerequisites Validation

- [✓] Test directory exists: ${TEST_DIR}
- [✓] Testing framework installed
- [✓] Development plans available
- [✓] Existing tests passing
- [✓] Tool skills directory exists
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Define Skills in Suite

This suite includes three skills following the naming conventions.

### Skill 1: `feature-impl`

**Name:** `feature-impl` (follows `{category}-{action}` pattern)
**Alternative names:** `impl-task`, `implement-feature`

**Purpose:** Implement features using TDD with mandatory code review.

**Phases:**
0. Orchestration Check (optional) - Check for worktree/task context
1. Design Review and Validation - Ensure design fits repository
2. Write Acceptance Tests - Create failing tests
3. Implement Incrementally - Make tests pass with TDD
4. Code Quality Review (MANDATORY) - Review against standards
5. Validate Against Acceptance Criteria - Verify all criteria met
6. Manual Testing - Guide user through manual validation
7. Final Review and Completion - Complete implementation

**Output:** Working implementation with passing tests, committed and ready for merge

### Skill 2: `bugfix-impl`

**Name:** `bugfix-impl` (follows `{category}-{action}` pattern)

**Purpose:** Fix bugs using hypothesis-based debugging with systematic root cause analysis.

**Phases:**
1. Reproduce Bug - Confirm issue and create failing test
2. Generate Hypotheses - Create 3+ potential root causes
3. Test Hypotheses - Systematically test each with debug instrumentation
4. Identify Root Cause - Analyze test results and confirm actual cause
5. Implement Minimal Fix - Fix the root cause, not symptoms
6. Regression Testing - Ensure fix doesn't break anything
7. Document Findings - Record hypotheses tested and root cause

**Output:** Bug fix with regression test and root cause documentation

### Skill 3: `refactor-impl`

**Name:** `refactor-impl` (follows `{category}-{action}` pattern)

**Purpose:** Refactor code with safety checks and test coverage.

**Phases:**
1. Baseline Tests - Ensure tests pass before refactoring
2. Identify Refactoring Opportunity - What needs improvement
3. Incremental Refactoring - Small, safe changes
4. Continuous Testing - Tests pass after each change
5. Code Review - Verify improvement

**Output:** Refactored code with same behavior, improved structure

---

## Phase 3: Provide SKILL.md Templates

Complete template for the feature-impl skill (primary skill).

### Template: feature-impl

**File:** `${TOOL_CONFIG}/skills/feature-impl/SKILL.md`

```markdown
---
name: feature-impl
description: Implement features using Test-Driven Development with mandatory code review
disable-model-invocation: true
argument-hint: "[path-to-plan or plan description]"
---

# Impl Feature

Implement a feature using Test-Driven Development with comprehensive testing and code review.

## When to Use

Use this skill when:
- Have an approved development plan
- Ready to implement feature
- Need TDD workflow with tests first
- Want comprehensive code review

Do NOT use when:
- Creating the plan (use plan-dev)
- Just prototyping or exploring
- Fixing a quick bug (consider bugfix-impl)

## Input

- Development plan file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`)
- OR plan description text

## Output

Working implementation with:
- Passing unit and integration tests
- Code reviewed against quality standards
- Validated acceptance criteria
- Manually tested with user
- Committed and ready for merge

## Process

### Phase 0: Orchestration Check (Optional)

**Purpose:** If running as part of an orchestrated workflow, integrate with task management.

**When to include:** Add this phase if the skill will be used by the worktree orchestrator.

**Steps:**

1. **Check for Orchestration Context**
   ```bash
   # Check if environment variables are set
   if [ -n "$TASK_FILE" ] && [ -n "$TASK_ID" ]; then
     # This is an orchestrated task
   fi
   ```

2. **Read Task State** (if orchestrated)
   - Read the task file specified by `$TASK_FILE`
   - Find the task identified by `$TASK_ID`
   - Check for PRD/plan overrides
   - Verify all dependencies have `status: completed`

3. **Handle Blocking Dependencies**
   - If dependencies not met: exit with "Blocked on: [list of task IDs]"
   - If dependencies met: mark task as `in_progress`

4. **Apply Overrides**
   - PRD/plan requirements override default behavior
   - Task-specific constraints take precedence
   - Merge orchestration context with input plan

**Output:**
```markdown
## Orchestration Context
Task ID: T3
Dependencies: T1 (completed), T2 (completed)
Status: Ready to proceed
Overrides: [Any specific requirements from PRD]
```

---

### Phase 1: Design Review and Validation

**Purpose:** Ensure the design from the development plan aligns with repository conventions and doesn't duplicate code.

**Steps:**

1. **Read Development Plan**
   - If argument is a file path, read it
   - If argument is text, capture it
   - Extract: requirements, design, acceptance criteria, constraints

2. **Review Repository Structure**

   Check for existing patterns and utilities:

   ```markdown
   ## Repository Review Checklist

   - [ ] Read project conventions (CLAUDE.md, README.md)
   - [ ] Check for similar features/patterns in codebase
   - [ ] Identify reusable utilities or base classes
   - [ ] Verify proposed design follows naming conventions
   - [ ] Confirm no duplication of existing functionality
   - [ ] Check framework/library usage is consistent
   ```

3. **Validate Design Choices**

   Compare plan's design against repository:

   | Design Aspect | Plan Proposes | Repository Has | Action |
   |---------------|---------------|----------------|--------|
   | Class structure | New base class | Existing base class | Use existing |
   | Utility function | New helper | Similar utility exists | Reuse existing |
   | Pattern | Custom approach | Established pattern | Follow pattern |
   | Import structure | Unclear | Specific convention | Follow convention |

4. **Identify Design Gaps**

   Check if the plan missed anything:
   - Are there edge cases not covered?
   - Does the design integrate with existing systems?
   - Are error handling patterns consistent?
   - Is logging/monitoring approach aligned?

5. **Propose Design Adjustments**

   If misalignments found:
   ```markdown
   ## Proposed Design Adjustments

   ### Adjustment 1: Use Existing Base Class
   **Plan proposed:** Creating new `ProcessorBase`
   **Repository has:** `framework.BaseProcessor` with similar functionality
   **Recommendation:** Extend `framework.BaseProcessor` instead

   ### Adjustment 2: Follow Import Convention
   **Plan shows:** Mixed import order
   **Repository convention:** stdlib → third-party → framework → local
   **Recommendation:** Update imports in implementation
   ```

6. **Get User Approval**

   Present findings and get confirmation:
   ```markdown
   ## Design Validation Summary

   **Alignment with Repository:** Good / Needs Adjustments
   **Code Reuse Opportunities:** [List]
   **Pattern Consistency:** Follows established patterns / Deviates (with reason)

   **Recommended Adjustments:**
   1. [Adjustment 1]
   2. [Adjustment 2]

   Proceed with implementation using adjusted design?
   ```

**Output:** Validated and potentially adjusted design ready for implementation.

**Gate:** Do not proceed until user approves the design adjustments.

---

### Phase 2: Write Acceptance Tests

**Purpose:** Create failing tests that encode the requirements and acceptance criteria.

**Steps:**

1. **Review Acceptance Criteria**

   Extract testable criteria from the plan:
   ```markdown
   ## Testable Acceptance Criteria

   From plan:
   - [ ] "System processes input X and produces output Y"
     → Test: test_process_input_produces_expected_output()

   - [ ] "Handles error case Z gracefully"
     → Test: test_handles_error_case_z()

   - [ ] "Integrates with component A"
     → Test: test_integration_with_component_a()
   ```

2. **Design Test Structure**

   Plan the test file organization:
   ```
   tests/
     test_feature_name_unit.py      # Unit tests for individual components
     test_feature_name_integration.py  # Integration tests
     test_feature_name_edge_cases.py   # Edge cases and error handling
   ```

3. **Write Failing Tests**

   Create tests before implementation:

   ```python
   # tests/test_feature_name_unit.py

   import pytest
   from feature_module import FeatureClass

   class TestFeatureClass:
       """Test suite for FeatureClass."""

       def test_basic_functionality(self):
           """Test basic feature operation."""
           # Arrange
           feature = FeatureClass(config)
           input_data = create_test_input()

           # Act
           result = feature.process(input_data)

           # Assert
           assert result.status == "success"
           assert result.value == expected_value

       def test_error_handling(self):
           """Test error case handling."""
           feature = FeatureClass(config)

           with pytest.raises(ValidationError) as exc_info:
               feature.process(invalid_input)

           assert "expected error message" in str(exc_info.value)

       @pytest.mark.asyncio
       async def test_async_operation(self):
           """Test asynchronous operation."""
           # Test async functionality
           pass
   ```

4. **Write Integration Tests**

   Test how components work together:

   ```python
   # tests/test_feature_name_integration.py

   class TestFeatureIntegration:
       """Integration tests for feature."""

       def test_end_to_end_workflow(self):
           """Test complete workflow from input to output."""
           # Test full integration
           pass

       def test_integration_with_existing_system(self):
           """Test integration with existing components."""
           # Test integration points
           pass
   ```

5. **Run Tests and Confirm They Fail**

   ```bash
   pytest tests/test_feature_name_*.py -v
   ```

   Expected: All tests fail (because implementation doesn't exist yet)

   Document the failure:
   ```markdown
   ## Test Baseline

   Created test files:
   - tests/test_feature_name_unit.py (8 tests)
   - tests/test_feature_name_integration.py (3 tests)

   Test run results: 0 passed, 11 failed

   This confirms tests are properly written and waiting for implementation.
   ```

**Output:** Complete test suite that fails appropriately.

**Gate:** Confirm all tests fail with expected errors before proceeding.

---

### Phase 3: Implement Incrementally

**Purpose:** Build the implementation incrementally, making tests pass one by one.

**Steps:**

1. **Implementation Order**

   Implement in dependency order:
   ```markdown
   ## Implementation Sequence

   1. **Data models** (no dependencies)
      - Define classes/dataclasses
      - Add validation
      → Makes tests: test_model_creation, test_validation pass

   2. **Core logic** (depends on models)
      - Implement main algorithms
      - Add business logic
      → Makes tests: test_basic_functionality, test_processing pass

   3. **Error handling** (depends on core logic)
      - Add exception handling
      - Implement recovery logic
      → Makes tests: test_error_handling, test_edge_cases pass

   4. **Integration** (depends on everything)
      - Wire components together
      - Connect to existing systems
      → Makes tests: test_integration_* pass
   ```

2. **Implement in Small Steps**

   For each component:
   - Write minimal code to make 1-2 tests pass
   - Run tests to verify
   - Refactor if needed (keeping tests passing)
   - Move to next component

3. **Follow Red-Green-Refactor Cycle**

   ```
   🔴 RED: Test fails
       ↓
   🟢 GREEN: Write minimal code to pass test
       ↓
   ♻️ REFACTOR: Improve code while keeping tests green
       ↓
   Repeat for next test
   ```

4. **Progress Tracking**

   Keep a running log of test progress:
   ```markdown
   ## Implementation Progress

   ### Iteration 1: Data Models
   - ✅ test_model_creation
   - ✅ test_model_validation
   - ✅ test_model_serialization
   Tests: 3 passed, 8 failed

   ### Iteration 2: Core Logic
   - ✅ test_basic_functionality
   - ✅ test_processing_valid_input
   Tests: 5 passed, 6 failed

   [Continue until all tests pass]
   ```

5. **Handle Unexpected Failures**

   If tests reveal design issues:
   - Document the issue
   - Adjust design if needed
   - Update tests if requirements changed
   - Get user approval for significant changes

**Output:** Working implementation with passing tests.

**Gate:** All tests must pass before proceeding to review.

---

### Phase 4: Code Quality Review

**Purpose:** Review implementation for code quality, maintainability, and adherence to standards.

**Critical Requirement:** This review is MANDATORY and must be thorough.

**Steps:**

1. **Self-Review Checklist**

   Review your own implementation:

   ```markdown
   ## Code Quality Checklist

   ### Code Standards
   - [ ] Follows project naming conventions (PascalCase classes, snake_case functions)
   - [ ] Proper import ordering (stdlib → third-party → framework → local)
   - [ ] Uses type hints consistently
   - [ ] Docstrings for public APIs (PEP 257)
   - [ ] No hardcoded values (use config/parameters)
   - [ ] Appropriate error handling (no silent failures)

   ### Design Principles
   - [ ] Single Responsibility: Each class/function does one thing
   - [ ] DRY: No duplicated code
   - [ ] Modular: Components are loosely coupled
   - [ ] Coherent: Related functionality is together
   - [ ] Composable: Components can be reused

   ### Maintainability
   - [ ] Code is self-documenting (clear names)
   - [ ] Complex logic has explanatory comments
   - [ ] No over-engineering (YAGNI principle)
   - [ ] Edge cases are handled
   - [ ] Logging at appropriate levels

   ### Testing
   - [ ] All tests pass
   - [ ] Tests cover happy path
   - [ ] Tests cover error cases
   - [ ] Tests cover edge cases
   - [ ] No test duplication
   ```

2. **Compare Against Design**

   Verify implementation matches approved design:
   - File structure matches plan
   - Classes/functions match specifications
   - Dependencies are as designed
   - Data flow follows the plan

3. **Check Repository Conventions**

   Ensure consistency with codebase:
   - Same patterns as similar features
   - Consistent error handling approach
   - Aligned with framework usage patterns
   - Matches project style guide

4. **Identify Improvements**

   Look for refactoring opportunities:
   ```markdown
   ## Refactoring Opportunities

   ### Required (blocking)
   - [ ] Extract duplicated prompt formatting (lines 45, 78, 112)
   - [ ] Add error handling in process_input() (line 67)

   ### Recommended (non-blocking)
   - [ ] Consider extracting validation logic to separate class
   - [ ] Could simplify conditional logic in analyze() (line 134)

   ### Nice to have
   - [ ] Add type hints to helper_function() (line 201)
   ```

5. **Apply Required Fixes**

   Make any required changes:
   - Fix code quality issues
   - Apply necessary refactoring
   - Re-run tests after each change
   - Update review checklist

**Output:** Code review report with all items checked or fixed.

**Gate:** All "Required" items must be addressed before proceeding.

---

### Phase 5: Validate Against Acceptance Criteria

**Purpose:** Systematically verify that all acceptance criteria from the plan are met.

**Steps:**

1. **Extract Criteria from Plan**

   List all acceptance criteria:
   ```markdown
   ## Acceptance Criteria Validation

   From development plan:

   1. [ ] All unit tests pass
   2. [ ] Integration tests cover happy path and error cases
   3. [ ] Performance meets requirement: processes 1000 items/sec
   4. [ ] Code follows project conventions
   5. [ ] No regression in existing features
   6. [ ] Documentation updated (if applicable)
   ```

2. **Validate Each Criterion**

   For each criterion, provide evidence:

   ```markdown
   ### Criterion 1: All unit tests pass
   **Status:** ✅ Met
   **Evidence:**
   ```bash
   pytest tests/test_feature_name_unit.py -v
   ======================== 8 passed in 1.2s =========================
   ```

   ### Criterion 2: Integration tests cover happy path and error cases
   **Status:** ✅ Met
   **Evidence:**
   - Happy path: test_end_to_end_workflow (PASSED)
   - Error cases: test_invalid_input, test_timeout (PASSED)

   ### Criterion 3: Performance requirement
   **Status:** ✅ Met
   **Evidence:**
   ```bash
   pytest tests/test_performance.py -v
   test_throughput: 1247 items/sec (target: 1000) ✅
   ```

   ### Criterion 4: Code conventions
   **Status:** ✅ Met
   **Evidence:** Verified in Phase 4 code review

   ### Criterion 5: No regressions
   **Status:** ✅ Met
   **Evidence:**
   ```bash
   pytest tests/ -v
   ======================== 42 passed in 5.3s =========================
   ```

   ### Criterion 6: Documentation
   **Status:** ✅ Met
   **Evidence:** Updated README.md with feature usage
   ```

3. **Run Full Test Suite**

   Verify no regressions:
   ```bash
   # Run all project tests
   pytest tests/ -v

   # Run any project-specific validation
   # (e.g., streamlit app, integration tests, etc.)
   ```

4. **Document Unmet Criteria**

   If any criteria not met:
   ```markdown
   ## Unmet Criteria

   ### Criterion 3: Performance requirement
   **Status:** ❌ Not Met
   **Current:** 847 items/sec
   **Target:** 1000 items/sec
   **Issue:** Bottleneck in data serialization
   **Options:**
   1. Optimize serialization (estimated effort: 2-3 hours)
   2. Adjust requirement if acceptable
   3. Defer optimization to future iteration
   ```

**Output:** Complete validation report showing all criteria met or issues documented.

**Gate:** All critical acceptance criteria must be met. User decides on non-critical items.

---

### Phase 6: Manual Testing

**Purpose:** Guide the developer through manual validation and user acceptance testing.

**Steps:**

1. **Identify Manual Test Cases**

   Determine what needs manual validation:
   ```markdown
   ## Manual Test Plan

   ### Test Case 1: Feature Workflow
   **Purpose:** Verify end-to-end user workflow
   **Steps:**
   1. [Step-by-step instructions]
   2. [What to do]
   3. [What to observe]
   **Expected:** [What should happen]

   ### Test Case 2: UI/UX Validation (if applicable)
   **Purpose:** Verify user interface behavior
   **Steps:** [...]
   **Expected:** [...]

   ### Test Case 3: Integration Check
   **Purpose:** Verify integration with existing system
   **Steps:** [...]
   **Expected:** [...]
   ```

2. **Provide Setup Instructions**

   Tell developer how to run the feature:
   ```bash
   # Start the application
   streamlit run app.py

   # Or run specific command
   python -m feature_module --option value
   ```

3. **Walk Through Each Test Case**

   Guide developer step-by-step:
   ```markdown
   ### Walking Through Test Case 1

   **Step 1:** Open the application
   → Have you opened it? Ready to proceed?

   **Step 2:** Navigate to [location] and click [button]
   → What do you see?

   **Step 3:** Enter [input] and submit
   → Did you get the expected [output]?

   **Step 4:** Try [edge case]
   → Does it handle this correctly?
   ```

4. **Collect Feedback**

   Document manual test results:
   ```markdown
   ## Manual Test Results

   ✅ Test Case 1: Feature Workflow - PASSED
   ✅ Test Case 2: UI/UX Validation - PASSED
   ⚠️  Test Case 3: Integration - MINOR ISSUE
      - Issue: [Description]
      - Severity: Low
      - Action: Create follow-up task / Fix now / Accept as-is

   **Overall Assessment:** Feature is ready / Needs minor fixes / Major issues
   ```

5. **Address Issues Found**

   If issues discovered:
   - Determine severity (blocker, major, minor)
   - Fix blocking/major issues immediately
   - Document minor issues for follow-up
   - Re-run affected manual tests

**Output:** Manual test report with developer sign-off.

**Gate:** All manual tests must pass or have documented, accepted issues.

---

### Phase 7: Final Review and Completion

**Purpose:** Conduct final review of all implementation work.

**Critical Requirement:** This is the FINAL checkpoint before considering the work done.

**Steps:**

1. **Complete Review Checklist**

   ```markdown
   ## Final Review Checklist

   ### Implementation
   - [ ] All automated tests pass (unit + integration)
   - [ ] All manual tests completed successfully
   - [ ] Code reviewed and quality standards met
   - [ ] All acceptance criteria validated

   ### Code Quality
   - [ ] No TODOs or FIXMEs in production code
   - [ ] No commented-out code
   - [ ] No debug print statements
   - [ ] No unused imports or variables

   ### Documentation
   - [ ] Code is self-documenting or has comments
   - [ ] README updated (if needed)
   - [ ] API documentation added (if applicable)
   - [ ] Change log updated (if applicable)

   ### Repository Health
   - [ ] No regression in existing tests
   - [ ] No new warnings or errors
   - [ ] Follows all project conventions
   - [ ] No framework code modified (if prohibited)
   ```

2. **Generate Summary**

   ```markdown
   ## Implementation Summary

   **Feature:** [Name]
   **Status:** Complete

   ### What Was Built
   - [Component 1]: [Brief description]
   - [Component 2]: [Brief description]
   - [Component 3]: [Brief description]

   ### Files Modified/Created
   Created:
   - src/feature_module.py
   - tests/test_feature_module.py

   Modified:
   - src/main.py (added feature registration)
   - requirements.txt (added dependency X)

   ### Test Results
   - Unit tests: 8/8 passed
   - Integration tests: 3/3 passed
   - Existing tests: 42/42 passed (no regressions)
   - Manual tests: 3/3 passed

   ### Acceptance Criteria
   All 6 criteria met ✅

   ### Performance
   - Throughput: 1247 items/sec (target: 1000) ✅
   - Latency: 15ms avg (target: <20ms) ✅
   ```

3. **Review Code One More Time**

   Do a final pass through the implementation:
   - Scan each file for obvious issues
   - Check that naming is consistent
   - Verify imports are clean
   - Look for any last-minute improvements

4. **Prepare for Handoff**

   Create handoff documentation:
   ```markdown
   ## Developer Handoff

   ### How to Use This Feature
   [Brief usage guide]

   ### Key Design Decisions
   1. [Decision 1]: [Rationale]
   2. [Decision 2]: [Rationale]

   ### Known Limitations
   - [Limitation 1]
   - [Limitation 2]

   ### Future Enhancements
   - [Enhancement 1] (nice-to-have)
   - [Enhancement 2] (future iteration)

   ### Maintenance Notes
   - [Important note for future maintainers]
   ```

**Output:** Complete implementation package ready for merge/deployment.

---

## Completion

When all phases are complete:

```markdown
## Implementation Complete ✅

**Feature:** [Name]
**Development Plan:** [Path to plan]
**Implementation Summary:** [Key achievements]

### Quality Metrics
- Tests: [X passed / X total]
- Code Review: ✅ All standards met
- Acceptance Criteria: ✅ All criteria validated
- Manual Testing: ✅ All cases passed

### Files Changed
**Created:**
- [List of new files]

**Modified:**
- [List of modified files]

### Next Steps
1. ✅ Implementation complete - ready for merge/deployment
2. Consider: [Any follow-up items]
3. Monitor: [What to watch after deployment]

**Ready to merge to main branch.**
```

### Orchestration Reporting

If running as part of an orchestrated workflow:

1. Update task status to `completed`
2. Record outputs (files created/modified)
3. Record completion timestamp
4. Note any blockers lifted (tasks that depended on this one)

---

## Best Practices

### Design Validation
- **Check repository first**: Don't reinvent existing functionality
- **Follow patterns**: Consistency is more important than cleverness
- **Ask about deviations**: If you must deviate from patterns, get approval

### Test-Driven Development
- **Write tests first**: Tests encode requirements
- **Small steps**: Make one test pass at a time
- **Keep tests green**: If a test breaks, fix it immediately
- **Test behavior, not implementation**: Focus on what, not how

### Code Review
- **Be thorough**: This is non-negotiable
- **Be honest**: Document real issues, don't gloss over problems
- **Refactor boldly**: If code smells bad, fix it before proceeding
- **Review incrementally**: Don't wait until the end

### Acceptance Criteria
- **Be specific**: "Tests pass" is specific, "works well" is not
- **Provide evidence**: Show test output, measurements, observations
- **Document gaps**: If criteria not met, explain why

### Manual Testing
- **Be interactive**: Walk through tests with developer
- **Be realistic**: Test like a real user would
- **Document everything**: Record what was tested and results

---

## Anti-Patterns to Avoid

1. **Skipping design validation**: Building without checking repository patterns
2. **Implementation before tests**: Writing code before tests
3. **Ignoring test failures**: Continuing with broken tests
4. **Superficial code review**: Checklist without actual review
5. **Skipping manual tests**: Assuming automated tests are enough
6. **No final review**: Finishing without one last check
7. **Incomplete acceptance**: Not validating all criteria

---

```
<!-- End of feature-impl SKILL.md template -->

---

### Template: bugfix-impl

**File:** `${TOOL_CONFIG}/skills/bugfix-impl/SKILL.md`

```markdown
---
name: bugfix-impl
description: Hypothesis-based debugging with systematic root cause analysis
disable-model-invocation: true
argument-hint: "[bug description or issue number]"
---

# Impl Bugfix

Fix bugs using hypothesis-based debugging with systematic testing and root cause analysis.

## Process

### Phase 1: Reproduce Bug
Confirm the bug exists and create a failing test that demonstrates it.

**Steps:**
1. Gather information about the bug (expected vs actual behavior)
2. Create reproduction steps
3. Write a failing test that captures the bug
4. Verify the test fails consistently

**Output:** Failing test that reproduces the issue

### Phase 2: Generate Hypotheses
Create at least 3 distinct hypotheses about potential root causes.

**Requirements:**
- At least 3 different hypotheses
- Each hypothesis should be testable
- Cover different potential causes (not variations of the same idea)

**Example format:**
```markdown
## Hypothesis 1: [Name]
**Theory:** [What might be causing this]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute]

## Hypothesis 2: [Name]
...

## Hypothesis 3: [Name]
...
```

**Output:** Document with 3+ testable hypotheses

### Phase 3: Test Hypotheses Systematically
Test each hypothesis using debug instrumentation.

**For each hypothesis:**
1. Add debug instrumentation:
   - Print statements at key points
   - Logging with relevant context
   - Assertions to check assumptions
   - Minimal reproducers

2. Run tests and collect data:
   - Execute code with instrumentation
   - Capture output and logs
   - Note unexpected behavior

3. Document findings:
   ```markdown
   ## Hypothesis N: [Name]
   **Test Results:** [What happened]
   **Evidence:** [Logs, outputs, observations]
   **Status:** CONFIRMED / REFUTED / INCONCLUSIVE
   ```

**IMPORTANT:** Test ALL hypotheses systematically - don't skip to the "likely" one.

**Output:** Test results for each hypothesis with evidence

### Phase 4: Identify Root Cause
Synthesize findings to identify the actual root cause.

**Analysis:**
- Which hypothesis was confirmed by evidence?
- What is the underlying issue?
- Why did this happen (not just how)?
- What is the minimal fix?

**Document root cause:**
```markdown
## Root Cause Analysis

**Confirmed Hypothesis:** [Which one and why]

**Root Cause:** [The actual underlying problem]

**Why It Happened:** [Context, explanation, history]

**Proposed Minimal Fix:** [What needs to change]

**Test Strategy:** [How to verify the fix works]
```

**Output:** Root cause analysis document

### Phase 5: Implement Minimal Fix
Implement the smallest change that fixes the root cause.

**Principles:**
- Fix the root cause, not symptoms
- Minimal change - don't refactor or add features
- Preserve existing behavior elsewhere
- Add regression test

**Steps:**
1. Implement the fix based on root cause analysis
2. Ensure the failing test now passes
3. Add regression test if needed
4. Run full test suite

**Output:** Bug fixed with minimal changes

### Phase 6: Regression Testing
Verify the fix doesn't break existing functionality.

**Run:**
```bash
# Full test suite
pytest tests/

# If project uses other test commands, run those too
```

**Check:**
- All existing tests still pass
- No new failures introduced
- Performance not degraded

**Output:** Full test suite passing

### Phase 7: Document Findings
Record the debugging process for future reference.

**Document:**
- Hypotheses tested (all of them)
- Evidence collected
- Root cause identified
- Fix implemented

**Why this matters:**
- Helps team understand the issue
- Prevents similar bugs
- Shows systematic approach
- Valuable for future debugging

**Output:** Complete bug report with root cause documentation

## Notes

- **Always generate at least 3 hypotheses** - forces thorough thinking
- **Test systematically** - don't jump to conclusions
- **Minimal fix preferred** - resist urge to refactor
- **Document everything** - helps prevent similar issues
- Root cause document is historical record, not just notes
```

---

### Template: refactor-impl

**File:** `${TOOL_CONFIG}/skills/refactor-impl/SKILL.md`

```markdown
---
name: refactor-impl
description: Refactor code with safety checks and test coverage
disable-model-invocation: true
argument-hint: "[file or component to refactor]"
---

# Impl Refactor

Refactor code safely while maintaining behavior.

## Process

### Phase 1: Baseline Tests
Ensure all tests pass before refactoring.

### Phase 2: Identify Refactoring
Document what needs improvement and why.

### Phase 3: Incremental Refactoring
Make small, safe changes one at a time.

### Phase 4: Continuous Testing
Run tests after each change.

### Phase 5: Code Review
Verify improvement without behavior change.
```

---

## Phase 4: Integration Instructions

How the implementation skills integrate with other suites.

### Integration with Dev Plan Suite

Implementation consumes development plans:

```bash
# 1. Create and approve plan
plan-dev ${SPEC_DIR}/YYYY-MM-DD-feature-prd.md
plan-review ${SPEC_DIR}/YYYY-MM-DD-feature-plan.md

# 2. Implement
feature-impl ${SPEC_DIR}/YYYY-MM-DD-feature-plan.md
```

**Plan provides:**
- Design → Architecture to follow
- Acceptance criteria → Tests to write
- System OKRs → Validation targets
- Task breakdown → Implementation order

### Integration with Orchestration Suite

For orchestrated parallel tasks:

```bash
# 1. Orchestrator creates worktrees
orchestrate-parallel setup

# 2. In each worktree, worker implements
cd ../Worktrees/task1
worktree-implement ${SPEC_DIR}/YYYY-MM-DD-task1-spec.md
# (worktree-implement uses feature-impl process internally)

# 3. Orchestrator reviews and merges
orchestrate-parallel review
orchestrate-parallel merge
```

**Orchestration provides:**
- Task spec → What to implement
- Dependencies → What must be merged first
- Task state → Progress tracking

### Standalone Usage

Implementation skills can also be used independently:
- Quick feature implementation from verbal plan
- Bug fixes without formal planning
- Code refactoring sessions
- Prototype development

---

## Phase 5: Testing & Validation

Test the implementation skills with various scenarios.

### Test 1: Simple Feature Implementation

Implement a simple, well-defined feature:

```bash
feature-impl ${SPEC_DIR}/2026-02-12-add-search-bar-plan.md
```

**Expected:**
- Tests written first (TDD)
- Implementation makes tests pass
- Code review catches any issues
- Manual testing validates UX

**Success criteria:**
- All tests pass
- Code review approved
- Acceptance criteria met
- Feature works as expected

### Test 2: Complex Feature with Multiple Components

Implement a feature with multiple interacting components:

```bash
feature-impl ${SPEC_DIR}/2026-02-12-batch-moderation-plan.md
```

**Expected:**
- Multiple test files (unit, integration)
- Incremental implementation
- Multiple code review iterations
- Comprehensive manual testing

**Success criteria:**
- Full test coverage
- Clean code review
- All acceptance criteria validated
- Integration tested

### Test 3: Bug Fix

Fix a reported bug:

```bash
bugfix-impl "Search returns wrong results for queries with special characters"
```

**Expected:**
- Failing test reproduces bug
- Root cause identified
- Minimal fix implemented
- Regression tests pass

**Success criteria:**
- Bug test passes
- No regressions
- Root cause documented

### Test 4: Code Refactoring

Refactor complex code:

```bash
refactor-impl "src/processor.py - extract validation logic"
```

**Expected:**
- Tests pass before refactoring
- Incremental improvements
- Tests still pass after each change
- Code is cleaner

**Success criteria:**
- All tests still pass
- Code is more maintainable
- No behavior change

---

## Troubleshooting

### Issue: Tests take too long to run

**Solutions:**
- Use test markers to run subsets (`pytest -m unit`)
- Parallelize tests if possible
- Mock slow dependencies in unit tests
- Run integration tests separately

### Issue: Code review finds many issues

**Solutions:**
- Review incrementally (don't wait until the end)
- Run linters early (`pylint`, `eslint`, etc.)
- Follow project conventions from the start
- Use IDE with linting enabled

### Issue: Acceptance criteria unclear

**Solutions:**
- Go back to plan and clarify
- Ask user for specifics
- Make criteria measurable
- Update plan for future reference

### Issue: Manual testing reveals issues

**Solutions:**
- Add automated tests for the issues
- Fix and re-test
- Update acceptance criteria if needed
- Document edge cases discovered

---

## Customization

### Adapting to Your Project

1. **Test directory**: Replace `${TEST_DIR}` with your actual directory
2. **Testing framework**: Adapt commands to your framework
3. **Coverage requirements**: Adjust based on project standards
4. **Code review checklist**: Customize for your conventions
5. **Commit format**: Match project commit message pattern

### Adding Custom Phases

Common additions:
```markdown
### Phase X: Performance Testing
Verify performance meets requirements.

### Phase Y: Security Scanning
Run security analysis tools.

### Phase Z: Documentation Update
Update relevant documentation.
```

---

## Summary

**This suite provides:**
- ✅ TDD-based implementation workflow
- ✅ Mandatory code review
- ✅ Comprehensive testing (unit, integration, manual)
- ✅ Acceptance criteria validation
- ✅ Bug fixing with root cause analysis
- ✅ Safe code refactoring

**Key skills:**
- `feature-impl` - Implement features using TDD
- `bugfix-impl` - Fix bugs with root cause analysis
- `refactor-impl` - Refactor code safely

**Prerequisites met:**
- Test directory identified and created
- Testing framework installed
- Development plans available
- Existing tests passing
- Tool skills directory configured

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths and testing framework
3. Test with an approved development plan
4. Use for TDD implementation!

---

**Last Updated:** 2026-02-12
**Related:**
- [Skills and Agents Guidelines](skills-and-agents-setup-guidelines-and-conventions.md)
- [Dev Plan Skill Suite](dev-plan-skill-suite-setup.md) - Provides input plans
- [Orchestration Skill Suite](orchestration-skill-suite-setup.md) - Uses feature-impl in worktrees
