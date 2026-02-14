# Refactoring - Complete Skill Suite

Consolidated setup guide for safe code refactoring skills: workflow orchestration and implementation with modularity focus.

---

## Overview

The **Refactoring Suite** provides a safe, test-preserving approach to improving code structure while maintaining functionality. This guide contains setup instructions for all related skills in one place.

### Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `refactor-workflow` | End-to-end refactoring orchestration with modularity evaluation | Orchestrator |
| `refactor-impl` | Refactor code with safety checks and test coverage | Atomic |

### When to Use Each Skill

**Use `refactor-workflow` when:**
- Starting a refactoring effort from scratch
- Want systematic planning and modularity measurement
- Need complete orchestration with review gates
- Unsure what refactoring has been planned (it detects and resumes)

**Use `refactor-impl` when:**
- Have an approved refactoring plan
- Ready to make incremental structural changes
- Want safety guarantees (tests before and after)
- Implementing with continuous test verification

### How Skills Work Together

```
refactor-workflow (orchestrator)
    │
    ├─► Phase 0: Detect state
    │       └─► Check for existing refactoring plan
    │
    ├─► Phase 1: Plan Refactoring
    │       └─► Document scope and goals
    │
    ├─► Phase 2: Review Plan (User approval gate)
    │
    ├─► Phase 3: Preserve Tests
    │       └─► Ensure test coverage before changes
    │
    ├─► Phase 4: Refactor Structure
    │       └─► Calls refactor-impl with incremental changes
    │
    ├─► Phase 5: Verify Tests
    │       └─► Confirm tests still pass after refactoring
    │
    ├─► Phase 6: Review Modularity
    │       └─► Measure improvement in cohesion/coupling
    │
    └─► Phase 7: Document Changes
            └─► Update architecture docs if needed
```

---

## Phase 0: Repository Detection

Follow the standard repository detection process to understand your project structure.

### Reference: setup-guidelines/repo-detection.md

The repository detection process helps customize skills to your project. Key steps:

1. **Detect Spec Directory** - Where refactoring plans are stored
2. **Detect Tool Configuration** - AI tool's config directory
3. **Read Project Conventions** - Code structure, testing practices
4. **Check Existing Artifacts** - Review existing refactoring plans
5. **Analyze Codebase Structure** - Understand modularity patterns

### Refactoring Suite Specific Checks

```bash
# Find spec directory
for dir in specs dev-docs docs/planning .docs planning docs/refactoring; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# Find existing refactoring plans
find ${SPEC_DIR} -name "*-refactor.md" -o -name "*-refactor-plan.md" 2>/dev/null

# Check for test framework
ls -la tests/ test/ __tests__/ 2>/dev/null

# Identify test command
which pytest >/dev/null 2>&1 && echo "Testing framework: pytest"
[ -f "package.json" ] && grep -q "jest\|mocha\|vitest" package.json && echo "JS testing framework detected"

# Review project conventions
cat CLAUDE.md README.md CONTRIBUTING.md ARCHITECTURE.md 2>/dev/null
```

### Detection Summary Template

```markdown
## Refactoring Suite Detection Summary

- **Spec directory**: ${SPEC_DIR}
- **Tool config**: ${TOOL_CONFIG}
- **Document naming pattern**: [e.g., YYYY-MM-DD-refactor-name-plan.md]
- **Project type**: [language/framework]
- **Test location**: [detected test directory]
- **Test framework**: [e.g., pytest, jest, cargo test]
- **Existing refactoring plans**: [count]
- **Key modularity patterns**: [summary]
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

### Refactoring Suite Specific Prerequisites

```bash
# Create spec directory if needed
mkdir -p ${SPEC_DIR}

# Create skill directories
mkdir -p ${TOOL_CONFIG}/skills/refactor-workflow
mkdir -p ${TOOL_CONFIG}/skills/refactor-impl

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
## Refactoring Suite Prerequisites

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

Complete SKILL.md templates for all skills in the refactoring suite.

---

## Skill 1: refactor-workflow

**Purpose:** Safe refactoring workflow with modularity evaluation and test preservation.

**File:** `${TOOL_CONFIG}/skills/refactor-workflow/SKILL.md`

```markdown
---
name: refactor-workflow
description: Safe refactoring workflow with modularity evaluation
disable-model-invocation: true
---

# Refactor Workflow

Safe code refactoring with modularity focus and test preservation.

## Phase 0: Detect Current State

**Check if refactoring plan exists:**

```bash
find ${SPEC_DIR} -name "*-refactor.md" -o -name "*-refactor-plan.md"
```

If plan exists:
- Read to understand refactoring scope
- Skip to Phase 3 (Preserve Tests)

**Output:** Starting phase

---

## Phase 1: Plan Refactoring

**Purpose:** Document refactoring scope and goals with modularity focus.

### Analyze Current State
Examine code to be refactored:
- Read target files
- Identify modularity issues
- Note coupling problems
- Find coherence issues

### Define Refactoring Goals
Create plan with:

```markdown
## Refactoring Plan

**Target:** [Files/modules to refactor]

**Current Problems:**
- **Cohesion:** [Description of cohesion issues]
- **Coherence:** [Description of coherence issues]
- **Coupling:** [Description of coupling problems]

**Goals:**
- **Improve Cohesion:** [Specific improvements]
- **Improve Coherence:** [Specific improvements]
- **Reduce Coupling:** [Specific improvements]

**Approach:**
1. [Step 1 - incremental change]
2. [Step 2 - incremental change]
3. [Step 3 - incremental change]

**Success Criteria:**
- **Cohesion:** Each module has single, clear purpose
- **Coherence:** Module boundaries make logical sense
- **Coupling:** Dependencies are minimal and well-defined
- Tests still pass
- Functionality unchanged
```

**Key Aspects to Emphasize:**

**Safety:**
- Preserve existing tests - never change tests during refactoring
- Run tests after each change
- Rollback if tests fail

**Incremental:**
- Small steps, measure improvement
- One refactoring at a time
- Verify tests pass after each step

**Test-Preserving:**
- Tests define correct behavior
- If tests need changes, it's not refactoring (it's changing functionality)
- Tests are the safety net

**Modularity Measurement:**
- **Cohesion:** Does each module do one thing well?
- **Coherence:** Do module boundaries make sense?
- **Coupling:** Are dependencies minimal?

**Output:** Refactoring plan in ${SPEC_DIR}/

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REFACTOR-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Plan refactoring [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Review plan
⏸️ Phase 3: Preserve tests
⏸️ Phase 4: Refactor structure
⏸️ Phase 5: Verify tests
⏸️ Phase 6: Review modularity
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 1: Analyzing current state and planning refactoring
Status: Documenting modularity issues
Started: [TIME]

CHECKLIST:
✅ Analyze current state
✅ Identify modularity issues
🔲 Define refactoring goals
🔲 Document incremental approach
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 2: Review Plan

**User Review Gate**

Present the refactoring plan:
1. Explain current problems (cohesion, coherence, coupling)
2. Describe proposed changes
3. Show step-by-step approach (incremental)
4. Ask: "Does this refactoring plan make sense?"

**If user requests changes:**
- Update plan
- Return to this phase

**If user approves:**
- Proceed to Phase 3

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
🔄 Phase 2: Review plan [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Preserve tests
⏸️ Phase 4: Refactor structure
⏸️ Phase 5: Verify tests
⏸️ Phase 6: Review modularity
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 2: User review of refactoring plan
Status: Waiting for approval
Started: [TIME]

REVIEW SUMMARY:
✅ Current problems identified
✅ Incremental approach defined
✅ Safety measures documented
🔲 User approval received
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 3: Preserve Tests

**Purpose:** Ensure test coverage before making changes.

**CRITICAL:** Tests are the safety net. Never skip this phase.

### Check Test Coverage
- Identify tests for target code
- Verify tests pass currently
- Note any missing coverage

### Run Baseline Tests
```bash
# Run tests for target code
pytest tests/test_target_module.py -v

# Run full test suite
pytest tests/ -v
```

**Document baseline:**
```markdown
## Test Baseline

**Tests for target code:**
- tests/test_target_module.py: 15 tests

**Current status:**
- All 15 tests passing
- Coverage: 85%

**Baseline established:** Tests define correct behavior
```

### Add Tests if Needed
If coverage is insufficient:
- Write additional tests for uncovered behavior
- Focus on behavior preservation
- Ensure all tests pass

**Safety principle:** If tests don't exist, we can't verify refactoring preserves behavior.

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
✅ Phase 2: Review plan
🔄 Phase 3: Preserve tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Refactor structure
⏸️ Phase 5: Verify tests
⏸️ Phase 6: Review modularity
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 3: Establishing test baseline
Status: Running baseline tests
Started: [TIME]

TEST BASELINE:
✅ Target tests: 15/15 passing
✅ Coverage: 85%
🔲 Add missing tests if needed
🔲 Final baseline confirmation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Full test suite passing

**Gate:** All tests must pass before refactoring begins.

---

## Phase 4: Refactor Structure

**Execute the refactoring:**

```
/refactor-impl ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md
```

Follow safe refactoring process:
1. Make small, incremental changes
2. Run tests after each change
3. Preserve functionality
4. Improve structure

**Safety Requirements:**
- Tests run after EACH incremental change
- Any test failure triggers rollback
- No test modifications (tests define correct behavior)
- Functionality must remain unchanged

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
✅ Phase 2: Review plan
✅ Phase 3: Preserve tests
🔄 Phase 4: Refactor structure [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Verify tests
⏸️ Phase 6: Review modularity
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 4: Executing refactoring via refactor-impl
Status: Running /refactor-impl
Started: [TIME]

IMPLEMENTATION PROGRESS:
🔄 Delegated to refactor-impl skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Refactored code

---

## Phase 5: Verify Tests

**Confirm tests still pass after refactoring.**

### Run Full Test Suite
```bash
pytest tests/
```

**If tests fail:**
- Identify what broke
- Determine if refactoring introduced bug
- Fix refactoring issues (never change tests)
- Re-run tests

**If tests pass:**
- Proceed to Phase 6

**CRITICAL:** If any test fails, the refactoring broke something. Tests define correct behavior, so fix the code, not the tests.

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
✅ Phase 2: Review plan
✅ Phase 3: Preserve tests
✅ Phase 4: Refactor structure
🔄 Phase 5: Verify tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Review modularity
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 5: Verifying all tests still pass
Status: Running full test suite
Started: [TIME]

TEST VERIFICATION:
✅ Target tests: 15/15 passing
✅ Full test suite: 42/42 passing
✅ No regressions detected
✅ Functionality preserved
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** All tests passing

**Gate:** All tests must pass before proceeding.

---

## Phase 6: Review Modularity

**Measure modularity improvement.**

### Before vs After Comparison

**Cohesion:**
- **Before:** [Description of cohesion issues]
- **After:** [Description of improvements]
- **Assessment:** ✓/✗ Improved?

**Coherence:**
- **Before:** [Description of coherence issues]
- **After:** [Description of improvements]
- **Assessment:** ✓/✗ Improved?

**Coupling:**
- **Before:** [Description of coupling problems]
- **After:** [Description of improvements]
- **Assessment:** ✓/✗ Improved?

### Modularity Metrics

**Cohesion (each module does one thing):**
- Count of responsibilities per module
- Single Responsibility Principle adherence
- Function/method count per class

**Coherence (boundaries make sense):**
- Related functionality grouped together
- Clear module purposes
- Logical separation of concerns

**Coupling (minimal dependencies):**
- Import count
- Dependency graph complexity
- Interface simplicity

### Present Assessment
```
Modularity Improvement Assessment:
✓/✗ Cohesion improved
✓/✗ Coherence improved
✓/✗ Coupling reduced

Overall: SUCCESS / NEEDS MORE WORK

Details:
- Cohesion: [Specific improvements]
- Coherence: [Specific improvements]
- Coupling: [Specific improvements]
```

**If needs more work:**
- Ask user if they want another iteration
- If yes: Return to Phase 1

**If successful:**
- Proceed to Phase 7

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
✅ Phase 2: Review plan
✅ Phase 3: Preserve tests
✅ Phase 4: Refactor structure
✅ Phase 5: Verify tests
🔄 Phase 6: Review modularity [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 7: Document changes

CURRENT TASK:
Phase 6: Measuring modularity improvement
Status: Comparing before/after metrics
Started: [TIME]

MODULARITY ASSESSMENT:
✅ Cohesion: Improved (3 responsibilities → 1 per module)
✅ Coherence: Improved (related functions now grouped)
✅ Coupling: Improved (15 imports → 8 imports)
🔲 Document final assessment
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 7: Document Changes

**Update architecture documentation if needed.**

### Check for Architecture Docs
```bash
find . -name "ARCHITECTURE.md" -o -name "README.md" -o -name "DESIGN.md"
```

### Update if Needed
If structure changed significantly:
- Update architecture diagrams
- Revise module descriptions
- Update import examples
- Document new patterns

**Update CLAUDE.md:**
- New patterns introduced
- Refactoring lessons learned
- Updated conventions if applicable

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Plan refactoring
✅ Phase 2: Review plan
✅ Phase 3: Preserve tests
✅ Phase 4: Refactor structure
✅ Phase 5: Verify tests
✅ Phase 6: Review modularity
🔄 Phase 7: Document changes [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 7: Updating documentation
Status: Updating ARCHITECTURE.md
Started: [TIME]

DOCUMENTATION:
✅ Update ARCHITECTURE.md
✅ Update CLAUDE.md
✅ Update module READMEs
🔲 Final review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Documentation updated

---

## Completion

When all phases complete:

```markdown
## Refactoring Complete ✅

**Target:** [Files/modules refactored]
**Plan:** ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md

### Modularity Improvement
- **Cohesion:** [Improvement summary]
- **Coherence:** [Improvement summary]
- **Coupling:** [Improvement summary]

### Safety Verification
- Tests before: 42/42 passing
- Tests after: 42/42 passing
- Functionality: ✅ Preserved
- Regressions: ✅ None

### Files Changed
**Modified:**
- [List of refactored files]

**Documentation Updated:**
- ARCHITECTURE.md
- CLAUDE.md

**Refactoring successful. Code structure improved while preserving functionality.**
```

---

## Notes

**Safety First:**
- Always preserve tests before refactoring
- Never change tests during refactoring
- Run tests after each incremental change
- Rollback if any test fails

**Incremental Approach:**
- Make small, safe changes one at a time
- Verify tests pass after each change
- Measure improvement continuously

**Modularity Focus:**
- Cohesion: Each module does one thing well
- Coherence: Module boundaries make logical sense
- Coupling: Dependencies are minimal and well-defined

**Test-Preserving:**
- Tests define correct behavior
- If tests need changes, it's not refactoring
- Tests are the safety net
```

---

## Skill 2: refactor-impl

**Purpose:** Refactor code with safety checks, incremental changes, and continuous testing.

**File:** `${TOOL_CONFIG}/skills/refactor-impl/SKILL.md`

```markdown
---
name: refactor-impl
description: Refactor code with safety checks and test coverage
disable-model-invocation: true
argument-hint: "[refactoring plan file or description]"
---

# Refactor Implementation

Refactor code safely while maintaining behavior through incremental changes and continuous testing.

## When to Use

Use when:
- Have a refactoring plan
- Need to improve code structure
- Want safety guarantees (tests before and after)
- Improving modularity (cohesion, coherence, coupling)

Do NOT use when:
- Building new features (use feature-impl)
- Fixing bugs (use bugfix-impl)
- Tests don't exist (write tests first)

## Input

- Refactoring plan file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md`)
- OR refactoring description text

## Output

- Refactored code with improved structure
- Same functionality (verified by tests)
- Improved modularity metrics
- All tests passing

## Process

### Phase 1: Baseline Tests

**Purpose:** Ensure all tests pass before refactoring.

**CRITICAL:** This is the safety net. If tests don't pass now, refactoring will be unsafe.

**Steps:**

1. **Run Full Test Suite**
   ```bash
   # Run all tests
   pytest tests/ -v

   # Or project-specific command
   npm test
   cargo test
   ```

2. **Document Baseline**
   ```markdown
   ## Test Baseline

   **Test suite:** All tests passing
   **Tests run:** 42
   **Tests passed:** 42
   **Coverage:** 85%

   **Status:** ✅ Baseline established
   ```

3. **Verify No Failures**
   If any tests fail:
   - Fix failing tests first
   - Do NOT proceed with refactoring
   - Refactoring requires green baseline

**Safety principle:** Green tests are required before refactoring begins.

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REFACTOR-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Baseline tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Identify refactoring opportunity
⏸️ Phase 3: Incremental refactoring
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Code review

CURRENT TASK:
Phase 1: Establishing test baseline
Status: Running full test suite
Started: [TIME]

CHECKLIST:
✅ Run full test suite
🔲 Document baseline
🔲 Verify all tests pass
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Passing test suite (baseline)

**Gate:** All tests must pass before proceeding.

---

### Phase 2: Identify Refactoring Opportunity

**Purpose:** Document what needs improvement and why.

**Steps:**

1. **Read Refactoring Plan**
   If plan file provided:
   - Read the plan
   - Extract target files
   - Understand goals

2. **Analyze Current Code**
   Review target code for:

   **Cohesion Issues:**
   - Multiple responsibilities in one module
   - Unrelated functions grouped together
   - Unclear module purpose

   **Coherence Issues:**
   - Illogical module boundaries
   - Related functionality scattered
   - Unclear separation of concerns

   **Coupling Issues:**
   - Too many dependencies
   - Tight coupling between modules
   - Complex dependency graphs

3. **Document Issues**
   ```markdown
   ## Refactoring Analysis

   **Target:** [File/module to refactor]

   **Cohesion Issues:**
   - Module handles both data validation AND business logic
   - Mixing concerns: auth + processing + formatting

   **Coherence Issues:**
   - Helper functions scattered across 3 files
   - Related validation logic in separate modules

   **Coupling Issues:**
   - 15 direct imports
   - Circular dependency with module X
   - Tight coupling to framework internals
   ```

4. **Define Incremental Steps**
   Break refactoring into small, safe steps:
   ```markdown
   ## Incremental Refactoring Steps

   1. **Extract validation** (5 min)
      - Move validation functions to validation.py
      - Update imports
      - Run tests

   2. **Separate business logic** (10 min)
      - Create business_logic.py
      - Move processing functions
      - Update imports
      - Run tests

   3. **Consolidate helpers** (5 min)
      - Merge scattered helper functions
      - Update imports
      - Run tests
   ```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Baseline tests
🔄 Phase 2: Identify refactoring opportunity [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Incremental refactoring
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Code review

CURRENT TASK:
Phase 2: Analyzing code and planning incremental steps
Status: Identifying cohesion/coherence/coupling issues
Started: [TIME]

ANALYSIS:
✅ Cohesion issues identified
✅ Coherence issues identified
✅ Coupling issues identified
🔲 Define incremental steps
🔲 Estimate effort per step
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Refactoring plan with incremental steps

---

### Phase 3: Incremental Refactoring

**Purpose:** Make small, safe changes one at a time.

**CRITICAL RULES:**

1. **One change at a time** - Don't combine multiple refactorings
2. **Test after each change** - Verify tests still pass
3. **Never change tests** - Tests define correct behavior
4. **Rollback if tests fail** - Don't proceed with broken tests

**For each incremental step:**

1. **Make Small Change**
   ```python
   # Step 1: Extract validation to separate file

   # BEFORE (in processor.py):
   def process_data(data):
       if not data:
           raise ValueError("Empty data")
       if len(data) > 100:
           raise ValueError("Too large")
       # ... processing logic ...

   # AFTER (create validation.py):
   def validate_data(data):
       if not data:
           raise ValueError("Empty data")
       if len(data) > 100:
           raise ValueError("Too large")

   # AFTER (in processor.py):
   from validation import validate_data

   def process_data(data):
       validate_data(data)
       # ... processing logic ...
   ```

2. **Run Tests Immediately**
   ```bash
   pytest tests/test_processor.py -v
   ```

   **If tests pass:** ✅ Continue to next step
   **If tests fail:** ❌ Rollback change, fix issue

3. **Measure Progress**
   ```markdown
   ## Step 1: Extract validation ✅
   - Created: validation.py
   - Modified: processor.py
   - Tests: 15/15 passing
   - Time: 5 min
   ```

4. **Repeat for Next Step**
   Continue with next incremental change.

**Example Incremental Refactoring:**

```markdown
## Refactoring Progress

### Step 1: Extract validation ✅
- Created: validation.py
- Modified: processor.py
- Tests: 15/15 passing ✅
- Cohesion: Improved (validation separated)

### Step 2: Separate business logic ✅
- Created: business_logic.py
- Modified: processor.py
- Tests: 15/15 passing ✅
- Cohesion: Improved (clear separation)

### Step 3: Consolidate helpers ✅
- Created: helpers.py
- Merged: utils1.py, utils2.py, utils3.py
- Modified: processor.py, business_logic.py
- Tests: 15/15 passing ✅
- Coherence: Improved (helpers grouped)
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
🔄 Phase 3: Incremental refactoring [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Code review

CURRENT TASK:
Phase 3: Executing incremental refactoring
Status: Step 2 of 3 - Separating business logic
Started: [TIME]

REFACTORING STEPS:
✅ Step 1: Extract validation (tests: 15/15 ✅)
🔄 Step 2: Separate business logic
⏸️ Step 3: Consolidate helpers

CURRENT STEP:
- Creating business_logic.py
- Moving 5 functions
- Tests will run after this step
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Incrementally refactored code

---

### Phase 4: Continuous Testing

**Purpose:** Run tests after each change to verify safety.

**This happens during Phase 3, but here's the summary:**

**After EACH incremental step:**

1. **Run relevant tests**
   ```bash
   # Quick: Run tests for changed module
   pytest tests/test_processor.py -v

   # Thorough: Run full suite
   pytest tests/ -v
   ```

2. **Verify results**
   - All tests pass: ✅ Continue
   - Any test fails: ❌ Stop and investigate

3. **Handle failures**
   If test fails:
   ```markdown
   ## Test Failure

   **Step:** Extract validation
   **Failed test:** test_process_empty_data
   **Error:** ImportError: cannot import validate_data

   **Action:** Fix import issue
   **Resolution:** Added validation.py to imports
   **Re-run:** Tests now pass ✅
   ```

**Safety guarantee:** No step proceeds with failing tests.

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
✅ Phase 3: Incremental refactoring
🔄 Phase 4: Continuous testing [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Code review

CURRENT TASK:
Phase 4: Final test verification
Status: Running full test suite
Started: [TIME]

TEST RESULTS:
✅ Step 1 tests: 15/15 passing
✅ Step 2 tests: 15/15 passing
✅ Step 3 tests: 15/15 passing
✅ Full test suite: 42/42 passing
✅ No regressions detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** All tests passing after refactoring

**Gate:** All tests must pass before proceeding.

---

### Phase 5: Code Review

**Purpose:** Verify improvement without behavior change.

**Review checklist:**

1. **Modularity Improvements**

   **Cohesion:**
   - [ ] Each module has single, clear purpose
   - [ ] Related functionality grouped together
   - [ ] No mixing of concerns

   **Coherence:**
   - [ ] Module boundaries make logical sense
   - [ ] Clear separation of concerns
   - [ ] Easy to understand structure

   **Coupling:**
   - [ ] Dependencies reduced
   - [ ] No circular dependencies
   - [ ] Clean, simple interfaces

2. **Behavior Preservation**
   - [ ] All tests pass
   - [ ] No test modifications (tests define behavior)
   - [ ] Functionality unchanged

3. **Code Quality**
   - [ ] Follows project conventions
   - [ ] Proper naming
   - [ ] Documentation updated if needed

**Measurement:**

```markdown
## Modularity Improvement

**Cohesion:**
- Before: Module handled 3 distinct responsibilities
- After: 3 modules, each with single responsibility
- Improvement: ✅ Yes

**Coherence:**
- Before: Related functions scattered across 3 files
- After: Related functions grouped logically
- Improvement: ✅ Yes

**Coupling:**
- Before: 15 direct imports, circular dependency
- After: 8 imports, no circular dependencies
- Improvement: ✅ Yes

**Overall:** Structure improved significantly
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
✅ Phase 3: Incremental refactoring
✅ Phase 4: Continuous testing
🔄 Phase 5: Code review [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 5: Reviewing modularity improvements
Status: Measuring cohesion/coherence/coupling
Started: [TIME]

MODULARITY REVIEW:
✅ Cohesion: Improved (1 responsibility per module)
✅ Coherence: Improved (logical grouping)
✅ Coupling: Improved (15→8 imports, no circular deps)
✅ Behavior preserved (all tests pass)
🔲 Final documentation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Code review report with modularity assessment

---

## Completion

When all phases complete:

```markdown
## Refactoring Complete ✅

**Target:** [Files refactored]
**Approach:** Incremental refactoring with continuous testing

### Modularity Improvements

**Cohesion:**
- Before: Mixed responsibilities (validation + logic + formatting)
- After: Single responsibility per module
- Assessment: ✅ Significantly improved

**Coherence:**
- Before: Related functions scattered across files
- After: Logical grouping of related functionality
- Assessment: ✅ Significantly improved

**Coupling:**
- Before: 15 imports, circular dependency
- After: 8 imports, clean dependency graph
- Assessment: ✅ Significantly improved

### Safety Verification

- Baseline tests: 42/42 passing
- After refactoring: 42/42 passing
- Tests modified: 0 (behavior preserved)
- Regressions: None

### Files Changed

**Created:**
- validation.py
- business_logic.py
- helpers.py

**Modified:**
- processor.py (refactored)

**Removed:**
- utils1.py (consolidated)
- utils2.py (consolidated)
- utils3.py (consolidated)

### Incremental Steps Taken

1. Extract validation → Tests pass ✅
2. Separate business logic → Tests pass ✅
3. Consolidate helpers → Tests pass ✅

**Refactoring successful. Structure improved, behavior preserved.**
```

---

## Best Practices

### Safety First

**Always preserve tests:**
- Tests define correct behavior
- Never change tests during refactoring
- If tests need changes, you're changing functionality (not refactoring)

**Run tests after each change:**
- Immediate feedback
- Quick rollback if needed
- Prevents cascading failures

**Small incremental steps:**
- One change at a time
- Easy to understand what broke
- Quick to fix issues

### Modularity Focus

**Cohesion (single responsibility):**
- Each module does one thing well
- No mixing of concerns
- Clear, focused purpose

**Coherence (logical boundaries):**
- Related functionality grouped together
- Module boundaries make sense
- Easy to navigate codebase

**Coupling (minimal dependencies):**
- Reduce import count
- Eliminate circular dependencies
- Clean, simple interfaces

### Measurement

**Before refactoring:**
- Count responsibilities per module
- Map dependency graph
- Document coupling issues

**After refactoring:**
- Compare metrics
- Verify improvement
- Document changes

---

## Anti-Patterns to Avoid

1. **Changing tests** - Tests define correct behavior
2. **Big-bang refactoring** - Make small incremental changes
3. **Skipping test runs** - Test after EACH change
4. **Mixing refactoring with features** - Refactor OR add features, not both
5. **Proceeding with failing tests** - Always maintain green tests
6. **Ignoring modularity** - Measure cohesion, coherence, coupling

---

## Notes

- **Test-preserving:** Never change tests during refactoring
- **Incremental:** Small steps, verify after each
- **Safety:** Run tests after each change
- **Modularity:** Measure cohesion, coherence, coupling
- **Rollback:** If tests fail, undo and fix
```

---

## Phase 3: Integration

### Complete Workflow Integration

The refactoring skills form a cohesive workflow with safety guarantees:

```
┌─────────────────────────────────────────────────────────────┐
│                   refactor-workflow                          │
│                   (Orchestrator)                             │
└─────────────────────────────────────────────────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
┌─────────┐         ┌──────────┐         ┌──────────┐
│ Phase 0 │         │ Phase 1  │         │ Phase 2  │
│ Detect  │────────▶│  Plan    │────────▶│ Review   │
│ State   │         │Refactoring│        │  Plan    │
└─────────┘         └──────────┘         └──────────┘
                                              │
                    ┌─────────────────────────┼────────────────┐
                    ▼                         ▼                ▼
            ┌──────────────┐         ┌──────────────┐  ┌──────────┐
            │ Phase 3      │         │ Phase 4      │  │ Phase 5  │
            │ Preserve     │────────▶│ Refactor     │─▶│ Verify   │
            │ Tests        │         │ Structure    │  │ Tests    │
            └──────────────┘         └──────────────┘  └──────────┘
                                            │
                                            ▼
                                   ┌────────────────┐
                                   │ refactor-impl  │
                                   │   (Atomic)     │
                                   └────────────────┘
                                            │
                    ┌───────────────────────┼────────────────┐
                    ▼                       ▼                ▼
            ┌──────────────┐        ┌──────────┐    ┌──────────┐
            │ Phase 6      │        │ Phase 7  │    │Complete  │
            │ Review       │───────▶│ Document │───▶│Improved  │
            │ Modularity   │        │ Changes  │    │Code      │
            └──────────────┘        └──────────┘    └──────────┘
```

### State Detection Flow

The orchestrator detects existing refactoring work and resumes appropriately:

```
User invokes: /refactor-workflow

                    ┌─────────────┐
                    │   Detect    │
                    │   State     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
    ┌─────────┐      ┌─────────┐     ┌─────────┐
    │Plan     │      │Tests    │     │Code     │
    │exists?  │      │baseline?│     │refactored?│
    └────┬────┘      └────┬────┘     └────┬────┘
         │                │               │
    YES  │  NO       YES  │  NO      YES  │  NO
         │                │               │
    ┌────▼───┐       ┌────▼───┐      ┌────▼───┐
    │Skip to │       │Skip to │      │Skip to │
    │Phase 3 │       │Phase 4 │      │Phase 6 │
    │(Tests) │       │(Refactor)│    │(Review)│
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

**Sequential Composition** (workflow calls refactor-impl):
```
refactor-workflow
  └─► Phase 1: Plan refactoring
        └─► Produces: ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md
              └─► Phase 2: Review plan (user gate)
                    └─► Phase 3: Preserve tests
                          └─► Phase 4: refactor-impl
                                └─► Incremental changes + tests
                                      └─► Produces: Improved code
```

**Standalone Usage** (direct refactor-impl):
```
User: "Refactor the validation module to improve cohesion"
  └─► refactor-impl
        └─► Baseline tests
              └─► Identify issues
                    └─► Incremental refactoring
                          └─► Continuous testing
                                └─► Produces: Improved code + tests
```

### Data Flow

**Artifacts produced at each stage:**

| Phase | Skill | Input | Output |
|-------|-------|-------|--------|
| 1 | refactor-workflow | Refactoring target | `*-refactor-plan.md` |
| 2 | User review | `*-refactor-plan.md` | Approval |
| 3 | refactor-workflow | Plan | Test baseline |
| 4 | refactor-impl | `*-refactor-plan.md` | Refactored code |
| 5 | refactor-workflow | Refactored code | Test verification |
| 6 | refactor-workflow | Refactored code | Modularity assessment |
| 7 | refactor-workflow | Complete refactoring | Updated docs |

**File naming conventions:**
- Plans: `${SPEC_DIR}/YYYY-MM-DD-refactor-name-plan.md`
- Reviews: Console output or comments in plan file

---

## Phase 4: Testing & Validation

### Validation Strategy

Test each skill individually, then test the complete workflow.

### Test 1: refactor-impl Simple

**Scenario:** Refactor a simple module to improve cohesion

**Steps:**
1. Create code with cohesion issues (e.g., multiple responsibilities in one file)
2. Write tests for current behavior
3. Invoke: `/refactor-impl "Separate validation from business logic"`
4. Verify baseline tests run
5. Check incremental refactoring (small steps)
6. Verify tests run after each step
7. Check modularity improvement

**Expected Output:**
- Baseline tests: All passing
- Incremental steps documented
- Tests run after each step
- All steps had passing tests
- Final tests: All passing
- Modularity improved (cohesion, coherence, coupling)

### Test 2: refactor-workflow Complete

**Scenario:** End-to-end refactoring with planning and modularity review

**Steps:**
1. Identify code needing refactoring
2. Invoke: `/refactor-workflow`
3. Review refactoring plan at gate
4. Verify test baseline established
5. Monitor incremental refactoring
6. Review modularity improvements
7. Check documentation updates

**Expected Output:**
- Refactoring plan created
- User review gate honored
- Test baseline established
- Incremental refactoring executed
- All tests passing throughout
- Modularity assessment provided
- Documentation updated

### Test 3: Test Preservation

**Scenario:** Verify tests are never modified during refactoring

**Steps:**
1. Create code with tests
2. Run refactor-impl
3. Monitor test files
4. Verify no test modifications

**Expected Output:**
- Test files not modified
- All original tests still pass
- New tests NOT added (refactoring, not feature work)
- Behavior preserved (verified by unchanged passing tests)

### Test 4: Incremental Safety

**Scenario:** Verify tests run after each incremental step

**Steps:**
1. Create multi-step refactoring
2. Run refactor-impl
3. Monitor test execution
4. Verify tests run between steps

**Expected Output:**
- Tests run after step 1 ✅
- Tests run after step 2 ✅
- Tests run after step 3 ✅
- No step proceeded with failing tests
- Evidence of incremental progress

### Test 5: Modularity Measurement

**Scenario:** Verify before/after modularity comparison

**Steps:**
1. Refactor code with coupling issues
2. Run refactor-workflow
3. Review Phase 6 modularity assessment

**Expected Output:**
- Before metrics documented:
  - Cohesion: Multiple responsibilities
  - Coherence: Scattered functionality
  - Coupling: High import count
- After metrics documented:
  - Cohesion: Single responsibility per module
  - Coherence: Logical grouping
  - Coupling: Reduced imports
- Improvement verified

---

## Phase 5: Best Practices

### Progress Tracking

**Why it matters:**
- Makes AI report what it's doing (less likely to skip steps)
- Provides audit trail of refactoring process
- User can see safety measures in action
- Can resume from any phase

**How to use:**
- ALL workflow and implementation skills include progress tracking
- Progress updates after each phase completion
- Shows current refactoring step
- Displays test results after each step

**Example from refactor-impl:**
```
PHASES:
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
🔄 Phase 3: Incremental refactoring [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Code review

CURRENT TASK:
Phase 3: Executing incremental refactoring
Status: Step 2 of 3 - Separating business logic
Started: [TIME]

REFACTORING STEPS:
✅ Step 1: Extract validation (tests: 15/15 ✅)
🔄 Step 2: Separate business logic
⏸️ Step 3: Consolidate helpers
```

### Safety First

**Why it matters:**
- Prevents breaking existing functionality
- Provides confidence to refactor
- Quick feedback on issues
- Easy rollback if needed

**How to ensure safety:**
- Baseline tests before starting
- Tests run after EACH incremental change
- Never modify tests (they define correct behavior)
- Rollback if any test fails

**Anti-pattern:**
```
❌ "Let's refactor everything, then run tests at the end"
✅ Refactor step 1 → test → step 2 → test → step 3 → test
```

### Incremental Approach

**Why it matters:**
- Easier to understand what changed
- Quick to identify what broke
- Reduces cognitive load
- Allows measuring progress

**How to use:**
- Break refactoring into small steps (5-10 minutes each)
- One change per step
- Test after each step
- Document each step's success

**Examples of good incremental steps:**
```
✅ GOOD: Extract validation to separate file
✅ GOOD: Move business logic to new module
✅ GOOD: Consolidate scattered helpers

❌ BAD: Reorganize entire codebase structure
❌ BAD: Refactor 5 modules simultaneously
❌ BAD: Extract + rename + reorganize in one step
```

### Test Preservation

**Why it matters:**
- Tests define correct behavior
- Changing tests during refactoring means changing functionality
- Tests are the safety net

**How to ensure:**
- Never modify test files during refactoring
- If tests fail, fix the refactored code (not the tests)
- Tests failing = refactoring broke something
- Only add tests when adding features (not during refactoring)

**Critical principle:**
```
Refactoring = Changing structure while preserving behavior
Tests = Definition of correct behavior
Therefore: Tests must not change during refactoring
```

### Modularity Measurement

**Why it matters:**
- Quantifies improvement
- Validates refactoring was worthwhile
- Identifies remaining issues
- Guides future refactoring

**How to measure:**

**Cohesion (each module does one thing):**
- Before: Count responsibilities per module
- After: Verify single responsibility
- Improvement: Multiple → Single responsibility

**Coherence (boundaries make sense):**
- Before: Map scattered functionality
- After: Verify logical grouping
- Improvement: Scattered → Grouped

**Coupling (minimal dependencies):**
- Before: Count imports, find circular deps
- After: Count imports, verify no circular deps
- Improvement: High → Low coupling

---

## Summary

### Skills Created

After following this guide, you will have:

- `${TOOL_CONFIG}/skills/refactor-workflow/SKILL.md` - Orchestrator
- `${TOOL_CONFIG}/skills/refactor-impl/SKILL.md` - Implementation

### Key Capabilities

**Safe Refactoring:**
- Test baseline before starting
- Incremental changes with continuous testing
- Test preservation (never change tests)
- Rollback on test failures

**Modularity Focus:**
- Cohesion: Single responsibility per module
- Coherence: Logical boundaries and grouping
- Coupling: Minimal dependencies
- Before/after measurement

**Systematic Approach:**
- Planning with modularity focus
- Review gates for user validation
- Incremental implementation
- Continuous test verification
- Modularity assessment

**Orchestration:**
- State detection (resume from any point)
- Review gates (user validation)
- Progress tracking throughout
- Complete workflow management

### Workflow Patterns

**Complete Refactoring Workflow:**
```
Refactoring Target
  → Plan refactoring (identify cohesion/coherence/coupling issues)
    → Review gate
      → Preserve tests (establish baseline)
        → Incremental refactoring (step → test → step → test)
          → Verify tests (all must pass)
            → Review modularity (measure improvement)
              → Document changes
```

**Safety Pattern:**
```
For each incremental step:
1. Make small change
2. Run tests
3. If pass: Continue
4. If fail: Rollback and fix
```

**State-Based Resume:**
```
Plan exists → Resume from test preservation
Tests baseline exists → Resume from refactoring
Code refactored → Resume from modularity review
```

### Key Principles

**Safety:**
- Preserve existing tests (never change during refactoring)
- Run tests after each change
- Rollback if tests fail
- Maintain green tests throughout

**Incremental:**
- Small steps (5-10 min each)
- Measure improvement continuously
- One change at a time
- Test after each step

**Test-Preserving:**
- Tests define correct behavior
- If tests need changes, it's not refactoring
- Tests are the safety net
- Behavior must remain unchanged

**Modularity Measurement:**
- **Cohesion:** Each module does one thing well
- **Coherence:** Module boundaries make logical sense
- **Coupling:** Dependencies are minimal
- Measure before and after

### Next Steps

1. **Create skills** using templates in this guide
2. **Test individually** using validation scenarios
3. **Test complete workflow** end-to-end
4. **Customize** for your project conventions
5. **Document** any project-specific adaptations

### Related Guides

- **Feature Development:** For building new features with TDD
- **Bugfixing:** For fixing bugs systematically
- **Code Review:** For automated review feedback

---

## Appendix: Quick Reference

### Invocation Commands

```bash
# Orchestrator (detects state, runs complete workflow)
/refactor-workflow

# Direct implementation (when you have a plan)
/refactor-impl "Refactor description"
/refactor-impl ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md
```

### File Locations

```
${SPEC_DIR}/
├── YYYY-MM-DD-refactor-name-plan.md     # Refactoring plans
└── ...

${TOOL_CONFIG}/skills/
├── refactor-workflow/SKILL.md
└── refactor-impl/SKILL.md
```

### Decision Tree

```
Do you have code to refactor?
├─ YES → Start with /refactor-workflow or /refactor-impl
└─ NO → Build features or fix bugs first

Do you have a refactoring plan?
├─ YES → Use /refactor-impl directly
└─ NO → Use /refactor-workflow to create plan

Are all tests passing?
├─ YES → Safe to refactor
└─ NO → Fix tests first, then refactor

Are you changing tests?
├─ YES → Stop! You're changing functionality, not refactoring
└─ NO → Good, tests define correct behavior

Are you making small incremental changes?
├─ YES → Good, test after each step
└─ NO → Break into smaller steps

Did a test fail?
├─ YES → Rollback and fix the refactored code
└─ NO → Continue to next step
```

### Modularity Checklist

```
Good refactoring improves:
✅ Cohesion (single responsibility per module)
✅ Coherence (logical boundaries)
✅ Coupling (minimal dependencies)
✅ All while preserving tests (green → green)

Bad refactoring:
❌ Changes test behavior (tests must not change)
❌ Proceeds with failing tests (always maintain green)
❌ Makes big-bang changes (use incremental steps)
❌ Ignores modularity (measure cohesion/coherence/coupling)
```

### Safety Checklist

```
Before refactoring:
✅ All tests passing (baseline)
✅ Tests cover target code
✅ Incremental plan defined

During refactoring:
✅ One change at a time
✅ Tests run after each change
✅ No test modifications
✅ Rollback if tests fail

After refactoring:
✅ All tests still passing
✅ Modularity improved
✅ Documentation updated
✅ Functionality unchanged
```
