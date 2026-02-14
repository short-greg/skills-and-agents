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

## Critical Reference

**skill-guidelines/modularity.md** - Complete modularity assessment framework

Refactoring is fundamentally about improving modularity. You MUST:
1. Assess all 8 modularity criteria BEFORE refactoring (Phase 0.5)
2. Use objective tools (radon, pylint, pydeps) not subjective judgment
3. Re-assess all 8 criteria AFTER refactoring (Phase 5)
4. Provide concrete evidence of improvement (before/after metrics)

The 8 criteria from modularity.md:
1. Cohesion - Single responsibility per module
2. Coupling - Minimal dependencies
3. Coherence - Logical boundaries
4. Abstraction - Appropriate layers
5. Encapsulation - Hidden implementation
6. Testability - Isolated testing
7. Reusability - Generic interfaces
8. Complexity - Low cyclomatic complexity

See modularity.md for complete measurement tools and thresholds.

## Input

- Refactoring plan file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md`)
- OR refactoring description text

## Output

- Refactored code with improved structure
- Same functionality (verified by tests)
- Improved modularity metrics
- All tests passing

## Process

## Goal

Improve code structure through measurable modularity improvements while maintaining 100% correctness (all tests pass before and after).

## OKRs

**Objective:** Deliver structurally improved code with verified modularity gains and zero behavior changes

**Key Results:**
- KR1: All original tests preserved and passing (100% test preservation, no test modifications)
- KR2: Modularity improved with concrete evidence (measurable via modularity.md 8 criteria)
- KR3: Incremental changes applied safely (tests pass after each step, no big-bang refactoring)
- KR4: No behavior changes introduced (verified by unchanged, passing test suite)
- KR5: Before/after improvement documented (modularity metrics captured and compared)

## Evaluation Criteria

This skill is **complete** when ALL of the following are verified:

**Safety:**
- [ ] Test baseline established BEFORE refactoring: `pytest tests/ -v` (capture count)
- [ ] All tests still pass AFTER refactoring: `pytest tests/ -v` (same count)
- [ ] No tests modified during refactoring: `git diff tests/` shows no changes
- [ ] No regressions introduced: Test count before = Test count after

**Modularity Improvement (Evidence Required):**
- [ ] Cohesion improved: Before/after assessment using modularity.md criteria #1
- [ ] Coupling reduced: Before/after dependency count (imports, circular deps eliminated)
- [ ] Coherence improved: Module boundaries more logical (measured via modularity.md criteria #3)
- [ ] Complexity reduced: `radon cc` shows lower average complexity after refactoring
- [ ] All 8 modularity criteria assessed: See modularity.md for complete framework

**Incremental Process:**
- [ ] Changes made in small steps: Each step documented in scratchpad
- [ ] Tests run after EACH step: Evidence of incremental testing captured
- [ ] No big-bang refactoring: Multiple small commits, not one large change

**Convention Compliance:**
- [ ] Follows CLAUDE.md patterns: List specific patterns used
- [ ] No code duplication: `jscpd src/` shows <3% duplication
- [ ] Naming conventions preserved: Verified in code review

**⚠️ Common Over-Evaluation Traps:**

**Trap: "Code looks better" ≠ Measurable modularity improvement**
- ❌ "The code is more organized now" - Subjective assessment without metrics
- ✅ Before: 15 imports, complexity 18. After: 8 imports, complexity 9. Evidence: `radon cc` output

**Trap: "Refactored successfully" ≠ Tests prove no behavior change**
- ❌ "Refactoring complete, everything works" - Did ALL original tests stay passing?
- ✅ Baseline: 42/42 tests passing. After: 42/42 tests passing. No tests modified.

**Trap: "More organized" ≠ Complexity metrics decreased**
- ❌ "Files are better organized" - Did you measure cyclomatic complexity?
- ✅ `radon cc` before: Average B (11.2). After: Average A (6.8). Improvement verified.

**Trap: "Tests still pass" ≠ ALL tests pass (no skipped/disabled)**
- ❌ "Tests pass" - Did you skip/disable any tests during refactoring?
- ✅ All 42 original tests still enabled and passing. Zero tests skipped or disabled.

**Reference: skill-guidelines/modularity.md**
The 8 modularity criteria MUST be assessed with concrete evidence:
1. Cohesion - Each module does ONE thing
2. Coupling - Minimal dependencies between modules
3. Coherence - Logical module boundaries
4. Abstraction - Appropriate abstraction levels
5. Encapsulation - Implementation details hidden
6. Testability - Can test in isolation
7. Reusability - Can reuse in other contexts
8. Complexity - Low cyclomatic complexity

See modularity.md for measurement tools and thresholds.

---

### Phase 0.5: Refactoring Scratchpad

**Purpose:** Plan refactoring approach and predict modularity improvements BEFORE changing ANY code.

**CRITICAL:** This phase prevents over-refactoring and ensures measurable goals.

**Steps:**

1. **Assess Current State (BEFORE refactoring)**

   Run modularity assessment tools on target code:
   ```bash
   # Measure complexity
   radon cc src/target_module.py -a -nb

   # Count dependencies
   grep -c "^import\|^from" src/target_module.py

   # Check for circular dependencies
   pydeps src/target_module.py --show-deps

   # Count public methods (cohesion indicator)
   grep -c "^    def [^_]" src/target_module.py
   ```

   Document baseline metrics in scratchpad.

2. **Create Refactoring Scratchpad**

   File: `${SPEC_DIR}/YYYY-MM-DD-refactor-name-scratchpad.md`

   Template:
   ```markdown
   # Refactoring Scratchpad: [Module Name]

   **Date:** YYYY-MM-DD
   **Target:** [File/module being refactored]

   ---

   ## Current State Assessment (BEFORE Refactoring)

   ### Modularity Metrics
   - **Complexity:** radon cc average = [Score] (Grade [A-F])
   - **Dependencies:** [Number] imports
   - **Circular Dependencies:** [Yes/No, list if yes]
   - **Cohesion:** [Number] public methods, [Number] responsibilities
   - **Function Length:** Average [Number] lines per function
   - **Nesting Depth:** Max [Number] levels

   ### Modularity Assessment (8 Criteria)
   Reference: skill-guidelines/modularity.md

   1. **Cohesion:** ❌/⚠️/✅ - [Evidence]
   2. **Coupling:** ❌/⚠️/✅ - [Evidence]
   3. **Coherence:** ❌/⚠️/✅ - [Evidence]
   4. **Abstraction:** ❌/⚠️/✅ - [Evidence]
   5. **Encapsulation:** ❌/⚠️/✅ - [Evidence]
   6. **Testability:** ❌/⚠️/✅ - [Evidence]
   7. **Reusability:** ❌/⚠️/✅ - [Evidence]
   8. **Complexity:** ❌/⚠️/✅ - [Evidence]

   **Current Issues:**
   - [List specific modularity problems]

   ---

   ## Proposed Changes

   **What will be refactored:**
   - [Specific structural changes]

   **What will NOT change:**
   - Behavior (verified by tests)
   - Public API (no breaking changes)

   ---

   ## Expected Improvements (AFTER Refactoring)

   ### Predicted Metrics
   - **Complexity:** Target average [Score] (Grade A-B)
   - **Dependencies:** Target [Number] imports (reduction of [X])
   - **Cohesion:** Target [Number] responsibility per module
   - **Circular Dependencies:** None

   ### Expected Modularity Improvements
   1. **Cohesion:** [Specific improvement expected]
   2. **Coupling:** [Specific improvement expected]
   3. **Coherence:** [Specific improvement expected]
   4. **Complexity:** [Specific improvement expected]

   ---

   ## Refactoring Steps (Incremental)

   **Step 1:** [Small change - 5-10 min]
   - Action: [What to do]
   - Test: Run `pytest tests/test_target.py -v`
   - Expected: All tests pass

   **Step 2:** [Small change - 5-10 min]
   - Action: [What to do]
   - Test: Run `pytest tests/test_target.py -v`
   - Expected: All tests pass

   **Step 3:** [Small change - 5-10 min]
   - Action: [What to do]
   - Test: Run `pytest tests/test_target.py -v`
   - Expected: All tests pass

   [Continue for all steps...]

   ---

   ## Risk Assessment

   **What could break:**
   - [List potential issues]

   **Mitigation:**
   - Tests run after each step
   - Rollback if any test fails
   - Small incremental changes

   ---

   ## Self-Review Question

   **Am I changing BEHAVIOR or just STRUCTURE?**

   - [ ] ✅ I am ONLY changing structure (refactoring)
   - [ ] ❌ I am changing behavior (NOT refactoring - should be feature/bugfix)

   **How do I know?**
   - Tests define correct behavior
   - If tests need changes, I'm changing behavior
   - If tests stay unchanged and pass, I'm refactoring correctly

   ---

   ## Sign-Off

   - [ ] Current state measured with tools (not subjective assessment)
   - [ ] Expected improvements are specific and measurable
   - [ ] Refactoring steps are small and incremental
   - [ ] Risk assessment complete
   - [ ] Ready to proceed with Phase 1 (Baseline Tests)
   ```

3. **Validate Scratchpad**

   Self-check:
   - [ ] All BEFORE metrics captured with tool output (not guesses)
   - [ ] Expected AFTER metrics are specific numbers (not "better" or "improved")
   - [ ] Refactoring steps are small (each <10 minutes)
   - [ ] Each step has clear test verification
   - [ ] All 8 modularity criteria assessed for current state

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REFACTOR-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 0.5: Refactoring scratchpad [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 1: Baseline tests
⏸️ Phase 2: Identify refactoring opportunity
⏸️ Phase 3: Incremental refactoring
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Measure improvement
⏸️ Phase 6: Code review

CURRENT TASK:
Phase 0.5: Creating refactoring scratchpad with BEFORE metrics
Status: Running modularity assessment tools
Started: [TIME]

CHECKLIST:
✅ Run complexity analysis (radon cc)
✅ Count dependencies
🔲 Assess all 8 modularity criteria
🔲 Define incremental steps
🔲 Validate scratchpad completeness
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Refactoring scratchpad with BEFORE metrics and incremental plan

**Gate:** Scratchpad complete with measurable baseline before proceeding.

---

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
✅ Phase 0.5: Refactoring scratchpad
🔄 Phase 1: Baseline tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Identify refactoring opportunity
⏸️ Phase 3: Incremental refactoring
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Measure improvement
⏸️ Phase 6: Code review

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
✅ Phase 0.5: Refactoring scratchpad
✅ Phase 1: Baseline tests
🔄 Phase 2: Identify refactoring opportunity [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Incremental refactoring
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Measure improvement
⏸️ Phase 6: Code review

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
✅ Phase 0.5: Refactoring scratchpad
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
🔄 Phase 3: Incremental refactoring [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Continuous testing
⏸️ Phase 5: Measure improvement
⏸️ Phase 6: Code review

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
✅ Phase 0.5: Refactoring scratchpad
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
✅ Phase 3: Incremental refactoring
🔄 Phase 4: Continuous testing [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Measure improvement
⏸️ Phase 6: Code review

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

### Phase 5: Measure Improvement

**Purpose:** Quantify modularity improvements with concrete evidence (AFTER refactoring).

**CRITICAL:** Use the SAME tools and measurements from Phase 0.5 scratchpad to prove improvement.

**Steps:**

1. **Re-run Modularity Assessment Tools (AFTER refactoring)**

   Run the exact same commands as Phase 0.5:
   ```bash
   # Measure complexity AFTER
   radon cc src/target_module.py -a -nb

   # Count dependencies AFTER
   grep -c "^import\|^from" src/target_module.py

   # Check for circular dependencies AFTER
   pydeps src/target_module.py --show-deps

   # Count public methods AFTER (cohesion indicator)
   grep -c "^    def [^_]" src/target_module.py

   # Check code duplication AFTER
   jscpd src/target_module.py
   ```

2. **Complete Modularity Assessment (AFTER)**

   Use modularity.md assessment template for ALL 8 criteria:

   ```markdown
   ## Modularity Assessment (AFTER Refactoring)

   **Reference:** skill-guidelines/modularity.md

   ### 1. Cohesion
   **Tool Check:**
   ```bash
   pylint --disable=all --enable=R0904,R0902 src/target_module.py
   ```
   Result: [Output]

   **Manual Check:**
   - Module purpose: [Describe in <10 words]
   - Responsibilities per module: [Number]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Specific tool output or metric]

   ### 2. Coupling
   **Tool Check:**
   ```bash
   grep -c "^import\|^from" src/target_module.py
   pydeps src/target_module.py --show-deps
   ```
   Result: [Number] imports, [circular: yes/no]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Import count, no circular dependencies]

   ### 3. Coherence
   **Tool Check:**
   ```bash
   tree src/ -L 2
   find src/ -name "utils.py" -o -name "helpers.py"
   ```
   Result: [Module organization assessment]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Logical module boundaries]

   ### 4. Abstraction
   **Manual Check:**
   - Clear abstraction layers: [Yes/No]
   - High-level doesn't know low-level details: [Yes/No]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Description]

   ### 5. Encapsulation
   **Tool Check:**
   ```bash
   pylint --disable=all --enable=W0212 src/target_module.py
   grep -r "\._[a-z]" src/ --include="*.py"
   ```
   Result: [Output]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [No external access to private members]

   ### 6. Testability
   **Tool Check:**
   ```bash
   pytest tests/unit/ --verbose
   ```
   Result: [All pass / Some fail]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Tests run in isolation]

   ### 7. Reusability
   **Manual Check:**
   - Could be used elsewhere: [Yes/No]
   - No hard-coded assumptions: [Yes/No]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Description]

   ### 8. Complexity
   **Tool Check:**
   ```bash
   radon cc src/target_module.py -a -nb
   radon mi src/target_module.py -nb
   ```
   Result: Complexity [Score], MI [Score]

   **Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
   **Evidence:** [Complexity grade A-B, MI > 20]
   ```

3. **Create Before/After Comparison**

   **REQUIRED:** Side-by-side comparison with EVIDENCE:

   ```markdown
   ## Before/After Comparison

   ### Complexity Metrics
   | Metric | Before | After | Improvement |
   |--------|--------|-------|-------------|
   | Cyclomatic Complexity (avg) | [Number] (Grade [X]) | [Number] (Grade [X]) | ✅/❌ |
   | Maintainability Index | [Number] | [Number] | ✅/❌ |
   | Lines of Code per Function | [Number] | [Number] | ✅/❌ |
   | Max Nesting Depth | [Number] | [Number] | ✅/❌ |

   **Tool Evidence:**
   ```bash
   # BEFORE: radon cc output
   [Paste actual output from Phase 0.5]

   # AFTER: radon cc output
   [Paste actual output from Phase 5]
   ```

   ### Coupling Metrics
   | Metric | Before | After | Improvement |
   |--------|--------|-------|-------------|
   | Import Count | [Number] | [Number] | ✅/❌ |
   | Circular Dependencies | [Yes/No] | [Yes/No] | ✅/❌ |
   | Max Dependency Depth | [Number] | [Number] | ✅/❌ |

   **Tool Evidence:**
   ```bash
   # BEFORE: import count
   [Paste actual count from Phase 0.5]

   # AFTER: import count
   [Paste actual count from Phase 5]
   ```

   ### Cohesion Metrics
   | Metric | Before | After | Improvement |
   |--------|--------|-------|-------------|
   | Responsibilities per Module | [Number] | [Number] | ✅/❌ |
   | Public Methods per Class | [Number] | [Number] | ✅/❌ |
   | Module Purpose Clarity | [Vague/Clear] | [Vague/Clear] | ✅/❌ |

   ### Code Duplication
   | Metric | Before | After | Improvement |
   |--------|--------|-------|-------------|
   | Duplication % | [Number]% | [Number]% | ✅/❌ |

   **Tool Evidence:**
   ```bash
   # BEFORE: jscpd output
   [Paste actual output]

   # AFTER: jscpd output
   [Paste actual output]
   ```

   ### Test Verification
   | Metric | Before | After | Status |
   |--------|--------|-------|--------|
   | Tests Passing | [X]/[Y] | [X]/[Y] | ✅ Same |
   | Tests Modified | 0 | 0 | ✅ None |
   | Tests Disabled | 0 | 0 | ✅ None |
   | New Test Failures | N/A | 0 | ✅ None |

   ### Overall Modularity (8 Criteria)
   | Criterion | Before | After | Improvement |
   |-----------|--------|-------|-------------|
   | 1. Cohesion | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 2. Coupling | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 3. Coherence | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 4. Abstraction | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 5. Encapsulation | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 6. Testability | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 7. Reusability | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |
   | 8. Complexity | [✅/⚠️/❌] | [✅/⚠️/❌] | [Yes/No/Same] |

   **Summary:**
   - Criteria improved: [Number]
   - Criteria unchanged: [Number]
   - Criteria degraded: [Number] ← Should be ZERO
   ```

4. **Validate Improvement**

   **PASS Criteria:**
   - [ ] At least 3 of 8 modularity criteria improved
   - [ ] Zero criteria degraded
   - [ ] All tests still passing (same count as baseline)
   - [ ] No tests modified or disabled
   - [ ] Concrete tool evidence for all metrics

   **FAIL Criteria (requires more work):**
   - Any modularity criterion degraded
   - Tests modified during refactoring
   - No measurable improvement in any criterion
   - Missing tool evidence

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0.5: Refactoring scratchpad
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
✅ Phase 3: Incremental refactoring
✅ Phase 4: Continuous testing
🔄 Phase 5: Measure improvement [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Code review

CURRENT TASK:
Phase 5: Measuring modularity improvement with concrete evidence
Status: Running AFTER assessment tools and comparing to BEFORE
Started: [TIME]

ASSESSMENT PROGRESS:
✅ Re-run complexity analysis (radon cc)
✅ Re-count dependencies
✅ Complete 8-criteria modularity assessment
🔲 Create before/after comparison table
🔲 Validate improvement (at least 3 criteria improved)
🔲 Verify no degradation (zero criteria worse)

PRELIMINARY RESULTS:
- Complexity: [Before Grade] → [After Grade]
- Dependencies: [Before Count] → [After Count]
- Cohesion: [Before Rating] → [After Rating]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Before/After comparison with evidence-based modularity improvement

**Gate:** At least 3 of 8 criteria improved, zero degraded, all tests passing.

---

### Phase 6: Code Review

**Purpose:** Final validation that refactoring met all quality criteria with EVIDENCE from Phase 5.

**CRITICAL:** This phase uses the measurements from Phase 5, not new subjective assessment.

**Review checklist:**

1. **Modularity Improvements (Evidence from Phase 5)**

   Reference the Before/After comparison from Phase 5:

   **Cohesion:**
   - [ ] Evidence shows improved cohesion (Phase 5 metrics)
   - [ ] Each module has single, clear purpose (verified via tool output)
   - [ ] Related functionality grouped together
   - [ ] No mixing of concerns

   **Coherence:**
   - [ ] Evidence shows improved coherence (Phase 5 assessment)
   - [ ] Module boundaries make logical sense
   - [ ] Clear separation of concerns
   - [ ] Easy to understand structure

   **Coupling:**
   - [ ] Evidence shows reduced coupling (Phase 5 import count)
   - [ ] Dependencies reduced (Before: [X] → After: [Y])
   - [ ] No circular dependencies (verified by pydeps)
   - [ ] Clean, simple interfaces

   **All 8 Modularity Criteria:**
   - [ ] Reference Phase 5 complete assessment
   - [ ] At least 3 of 8 criteria improved
   - [ ] Zero criteria degraded
   - [ ] All ratings backed by tool evidence

2. **Behavior Preservation**
   - [ ] All tests pass (Phase 5 verification: [X]/[X])
   - [ ] Same test count as baseline (Phase 1: [X], Phase 5: [X])
   - [ ] No test modifications: `git diff tests/` shows no changes
   - [ ] Functionality unchanged (tests prove this)

3. **Code Quality**
   - [ ] Follows project conventions (CLAUDE.md patterns listed)
   - [ ] Proper naming conventions maintained
   - [ ] Code duplication reduced (Phase 5: [Before]% → [After]%)
   - [ ] Complexity reduced (Phase 5: Grade [Before] → Grade [After])
   - [ ] Documentation updated if needed

4. **Incremental Process Verification**
   - [ ] Scratchpad shows incremental steps taken (Phase 0.5)
   - [ ] Tests run after each step (documented in implementation)
   - [ ] No big-bang refactoring (multiple small commits)
   - [ ] Each step had passing tests

**Final Measurement Summary:**

Reference Phase 5's Before/After comparison. Example:

```markdown
## Modularity Improvement (Evidence-Based)

**Source:** Phase 5 Before/After Comparison

**Cohesion:**
- Before: Module handled 3 distinct responsibilities
- After: 3 modules, each with single responsibility
- Evidence: `pylint R0904` violations: 3 → 0
- Improvement: ✅ Yes (Verified by tool)

**Coherence:**
- Before: Related functions scattered across 3 files
- After: Related functions grouped logically in 1 module
- Evidence: `tree` output shows clear module organization
- Improvement: ✅ Yes (Verified by structure)

**Coupling:**
- Before: 15 direct imports, circular dependency
- After: 8 imports, no circular dependencies
- Evidence: `grep -c import`: 15 → 8, `pydeps`: circular detected → none
- Improvement: ✅ Yes (Verified by tools)

**Complexity:**
- Before: Average complexity 14.2 (Grade C)
- After: Average complexity 7.1 (Grade A)
- Evidence: `radon cc` output (see Phase 5)
- Improvement: ✅ Yes (50% reduction)

**Test Preservation:**
- Before: 42/42 tests passing
- After: 42/42 tests passing
- Tests modified: 0
- Evidence: `pytest` output identical, `git diff tests/` empty
- Status: ✅ Behavior preserved

**Overall:** Structure improved significantly with concrete evidence
- 4 of 8 modularity criteria improved
- 0 of 8 criteria degraded
- All improvements verified by tools (not subjective)
```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0.5: Refactoring scratchpad
✅ Phase 1: Baseline tests
✅ Phase 2: Identify refactoring opportunity
✅ Phase 3: Incremental refactoring
✅ Phase 4: Continuous testing
✅ Phase 5: Measure improvement
🔄 Phase 6: Code review [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 6: Final code review and validation
Status: Verifying all quality criteria met
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
**Documentation:**
- Scratchpad: ${SPEC_DIR}/YYYY-MM-DD-refactor-name-scratchpad.md
- Before/After Evidence: See Phase 5 assessment

### Modularity Improvements (Evidence-Based)

**Reference:** Phase 5 Before/After Comparison (skill-guidelines/modularity.md)

**Cohesion:**
- Before: Mixed responsibilities (validation + logic + formatting)
- After: Single responsibility per module
- Evidence: `pylint R0904` violations: 3 → 0
- Tool Output: Module now has 1 clear purpose vs 3 before
- Assessment: ✅ Significantly improved

**Coherence:**
- Before: Related functions scattered across files
- After: Logical grouping of related functionality
- Evidence: Module structure now matches domain concepts
- Tool Output: No "utils.py" files, clear module names
- Assessment: ✅ Significantly improved

**Coupling:**
- Before: 15 imports, circular dependency detected
- After: 8 imports, clean dependency graph
- Evidence: `grep -c import`: 15 → 8, `pydeps`: circular → none
- Tool Output: Max dependency depth reduced from 4 to 2
- Assessment: ✅ Significantly improved (47% reduction)

**Complexity:**
- Before: Average cyclomatic complexity 14.2 (Grade C)
- After: Average cyclomatic complexity 7.1 (Grade A)
- Evidence: `radon cc -a` output (see Phase 5)
- Tool Output: 50% complexity reduction
- Assessment: ✅ Significantly improved

**All 8 Modularity Criteria:**
- Criteria improved: 4 (Cohesion, Coupling, Coherence, Complexity)
- Criteria unchanged: 4 (maintained good ratings)
- Criteria degraded: 0 ← **CRITICAL: Zero regressions**
- Assessment: ✅ Overall modularity significantly improved

### Safety Verification

**Test Preservation:**
- Baseline tests (Phase 1): 42/42 passing
- After refactoring (Phase 5): 42/42 passing
- Tests modified: 0 (behavior preserved)
- Tests disabled: 0 (all original tests still active)
- Regressions: None
- Evidence: `git diff tests/` shows no changes

**Verification Command:**
```bash
pytest tests/ -v
# Output: 42 passed in X.XXs
```

### Files Changed

**Created:**
- validation.py (extracted from processor.py)
- business_logic.py (extracted from processor.py)
- helpers.py (consolidated from utils*.py)

**Modified:**
- processor.py (refactored: 250 lines → 80 lines)

**Removed:**
- utils1.py (consolidated into helpers.py)
- utils2.py (consolidated into helpers.py)
- utils3.py (consolidated into helpers.py)

**Metrics:**
- Total lines of code: 450 → 380 (16% reduction)
- Average function length: 35 lines → 18 lines
- Code duplication: 12% → 2% (verified by jscpd)

### Incremental Steps Taken

**Process:** All steps from Phase 0.5 scratchpad

1. Extract validation → Tests pass ✅ (pytest: 42/42)
2. Separate business logic → Tests pass ✅ (pytest: 42/42)
3. Consolidate helpers → Tests pass ✅ (pytest: 42/42)

**Safety:** Tests run after EVERY step, no failures at any point

### Evidence Summary

**Objective Measurements:**
- Complexity: `radon cc` Grade C → Grade A
- Coupling: 15 imports → 8 imports
- Duplication: 12% → 2%
- Tests: 42/42 → 42/42 (preserved)
- LOC: 450 → 380 (simplified)

**All measurements from tools, not subjective assessment.**

**Refactoring successful. Structure improved with concrete evidence, behavior preserved.**
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

**CRITICAL REFERENCE:** skill-guidelines/modularity.md

Refactoring is ALL ABOUT modularity improvement. Use the complete 8-criteria framework:

**The 8 Modularity Criteria (from modularity.md):**

1. **Cohesion** - Each module does ONE thing well
   - Tool: `pylint --enable=R0904,R0902`
   - Threshold: <5 public functions per module
   - Evidence: Function count, responsibility count

2. **Coupling** - Minimal dependencies between modules
   - Tool: `pydeps`, `grep -c import`
   - Threshold: <10 imports, no circular dependencies
   - Evidence: Import count, dependency graph

3. **Coherence** - Logical module boundaries
   - Tool: `tree`, `find` for utils.py
   - Threshold: No "junk drawer" modules
   - Evidence: Module organization matches domain

4. **Abstraction** - Appropriate abstraction levels
   - Manual: Check layer violations
   - Threshold: High-level doesn't know low-level details
   - Evidence: No database code in API layer, etc.

5. **Encapsulation** - Implementation details hidden
   - Tool: `pylint --enable=W0212`
   - Threshold: No external access to private methods
   - Evidence: Private method usage, public API size

6. **Testability** - Can test in isolation
   - Tool: `pytest tests/unit/`
   - Threshold: All tests pass without external dependencies
   - Evidence: Test setup complexity, mock usage

7. **Reusability** - Code can be reused elsewhere
   - Manual: Check for hard-coded assumptions
   - Threshold: Generic interfaces, no caller-specific code
   - Evidence: Interface design, no tight coupling to use case

8. **Complexity** - Low cyclomatic complexity
   - Tool: `radon cc`, `radon mi`
   - Threshold: Complexity <15, MI >20, functions <50 lines
   - Evidence: Complexity grade, MI score

**See modularity.md for:**
- Complete tool commands for each criterion
- Thresholds for pass/warning/fail
- Example good vs bad code
- Common over-evaluation traps

### Measurement (REQUIRED)

**Before refactoring (Phase 0.5):**
- Run ALL 8 modularity assessments with tools
- Document baseline metrics in scratchpad
- Use objective measurements, not subjective assessment

**After refactoring (Phase 5):**
- Re-run ALL 8 modularity assessments with same tools
- Create side-by-side comparison table
- Verify improvement with concrete evidence
- Require: At least 3 criteria improved, zero degraded

**Evidence Required:**
- Tool output (not "looks better")
- Numeric metrics (not "more modular")
- Before/after comparison (not "improved")
- Pass/fail against thresholds (not subjective rating)

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
