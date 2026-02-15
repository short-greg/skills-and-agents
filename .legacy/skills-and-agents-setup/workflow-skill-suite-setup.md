# Workflow Skill Suite Setup

Setup instructions for high-level workflow orchestrator skills that compose other skills into end-to-end development workflows.

---

## Overview

The **Workflow Suite** provides user-facing orchestrators that manage complete development workflows from requirements to implementation. These skills detect current state, compose lower-level skills, and add review gates between phases.

**This suite includes:**
- `feature-workflow` - End-to-end feature development (define → plan → implement)
- `bugfix-workflow` - Hypothesis-based debugging workflow
- `refactor-workflow` - Safe refactoring with modularity evaluation

**When to use:**
- Starting a new feature from scratch
- Debugging an issue systematically
- Refactoring existing code safely
- When you want guided, gated workflow with reviews between phases

**When NOT to use:**
- When you only need one phase (just use feature-define, feature-impl, etc. directly)
- For parallel task execution (use parallel-orchestrate instead)
- When you want full manual control (use individual skills directly)

**Prerequisites:**
- Feature Definition Suite set up (`feature-define`, `feature-define-review`)
- Feature Planning Suite set up (`feature-plan`, `feature-plan-review`)
- Feature Implementation Suite set up (`feature-impl`, `bugfix-impl`, `refactor-impl`)
- Spec directory configured (e.g., `specs/`, `dev-docs/`)

---

## Phase 0: Repository Detection

**Before creating skills, detect project-specific details.**

### Detect Spec Directory

```bash
# Common locations
ls -la specs/
ls -la dev-docs/
ls -la docs/requirements/
```

**Action:** Replace `{spec-dir}` throughout templates with actual directory.

### Detect Tool Configuration

Determine where skills will be installed:
- **Claude Code**: `~/.claude/skills/`
- **Other tools**: Check tool documentation

**Action:** Replace `{tool-config}` with actual path.

**Gate:** Do not proceed until spec directory is confirmed.

---

## Phase 1: Validate Prerequisites

Ensure prerequisite suites are set up.

### Check Feature Definition Suite

```bash
# Check if skills exist
ls {tool-config}/skills/feature-define/SKILL.md
ls {tool-config}/skills/feature-define-review/SKILL.md
```

### Check Feature Planning Suite

```bash
ls {tool-config}/skills/feature-plan/SKILL.md
ls {tool-config}/skills/feature-plan-review/SKILL.md
```

### Check Feature Implementation Suite

```bash
ls {tool-config}/skills/feature-impl/SKILL.md
ls {tool-config}/skills/bugfix-impl/SKILL.md
ls {tool-config}/skills/refactor-impl/SKILL.md
```

**Gate:** Do not proceed until all prerequisite skills exist.

---

## Phase 2: Define Skills in Suite

This suite includes three workflow orchestrator skills.

### Skill 1: `feature-workflow`

**Name:** `feature-workflow` (follows `{task}-{action}` pattern)

**Purpose:** End-to-end feature development orchestrator with state detection and review gates.

**Phases:**
1. **Detect State** - Check what already exists (PRD, plan, code)
2. **Define Requirements** - Create PRD if needed (calls `feature-define`)
3. **Review Requirements** - User approval gate
4. **Plan Implementation** - Create plan if needed (calls `feature-plan`)
5. **Review Plan** - User approval gate
6. **Decide Execution** - Parallel or sequential?
7. **Propose Implementation** - Design approach without code
8. **Review Proposal** - User approval gate
9. **Implement** - Execute via `feature-impl` or `parallel-orchestrate`
10. **Review Modularity** - Verify cohesion, coherence, loose coupling

**Output:** Complete feature with passing tests and modularity verification

### Skill 2: `bugfix-workflow`

**Name:** `bugfix-workflow` (follows `{task}-{action}` pattern)

**Purpose:** Systematic debugging workflow using hypothesis-based approach.

**Phases:**
1. **Detect State** - Check if hypotheses/debugging already started
2. **Reproduce Issue** - Confirm bug exists
3. **Generate Hypotheses** - Create 3+ potential root causes
4. **Review Hypotheses** - User approval gate
5. **Test Hypotheses** - Debug instrumentation, assertions, minimal reproducers
6. **Identify Root Cause** - Present findings
7. **Review Root Cause** - User approval gate
8. **Implement Fix** - Call `bugfix-impl` with minimal fix
9. **Verify Fix** - Run tests, confirm issue resolved

**Output:** Fixed bug with root cause documented and tests passing

### Skill 3: `refactor-workflow`

**Name:** `refactor-workflow` (follows `{task}-{action}` pattern)

**Purpose:** Safe refactoring workflow with modularity focus.

**Phases:**
1. **Detect State** - Check if refactoring plan exists
2. **Plan Refactoring** - Document scope and goals
3. **Review Plan** - User approval gate
4. **Preserve Tests** - Ensure test coverage before changes
5. **Refactor Structure** - Call `refactor-impl`
6. **Verify Tests** - Confirm tests still pass
7. **Review Modularity** - Measure improvement in cohesion/coupling
8. **Document Changes** - Update architecture docs if needed

**Output:** Refactored code with improved modularity and passing tests

---

## Phase 3: Provide SKILL.md Templates

Complete templates for each workflow skill.

### Template 1: feature-workflow

**File:** `{tool-config}/skills/feature-workflow/SKILL.md`

```markdown
---
name: feature-workflow
description: End-to-end feature development workflow with state detection and review gates
disable-model-invocation: true
---

# Feature Workflow

Complete feature development orchestrator from requirements to implementation.

## Workflow State Management

**CRITICAL: This workflow uses two components to track progress:**

1. **WORKFLOW STATE** - Current position and context
2. **STATUS LOG** - Complete history of what happened

**RULES:**
- Display WORKFLOW STATE at the start of EVERY response
- Append to STATUS LOG when phases START, COMPLETE, FAIL, or SKIP
- Update both STATE and LOG at every phase transition
- Never skip logging - it enforces workflow completion

---

## Phase 0: Initialize Workflow

**FIRST ACTION: Create state variable and status log**

```markdown
**WORKFLOW STATE:**
- Current Phase: Phase 0 - Initialize Workflow
- Status: STARTED
- Next Phase: Phase 1 - Detect State
- Blocking Issues: None
- Execution Strategy: null
- Artifacts: {}
- Approval Gates Passed: []

**STATUS LOG:**
[2026-02-14 10:00] Workflow initialized
[2026-02-14 10:00] Phase 0 - Initialize Workflow [STARTED]
```

**Initialize workflow:**
- Confirm spec directory location: `{spec-dir}`
- Confirm prerequisites installed (feature-define, feature-plan, feature-impl)

**UPDATE STATE AND LOG:**
```markdown
**WORKFLOW STATE:**
- Current Phase: Phase 1 - Detect State
- Status: STARTED
- Next Phase: TBD (depends on detection)
- Blocking Issues: None

**STATUS LOG:**
[10:00] Phase 0 - Initialize Workflow [COMPLETED] → Prerequisites confirmed
[10:00] Phase 1 - Detect State [STARTED]
```

---

## Phase 1: Detect Current State

**DISPLAY STATE:**
```markdown
**WORKFLOW STATE:**
- Current Phase: Phase 1 - Detect State
- Status: IN_PROGRESS
```

**LOG:**
```
[10:01] Phase 1 - Detect State [IN_PROGRESS] → Checking for existing artifacts
```

**Check what already exists:**

### Look for PRD
Search for existing requirements documents:
```bash
find {spec-dir} -name "*-prd.md" -o -name "*-requirements.md"
```

### Look for Plan
Search for existing development plans:
```bash
find {spec-dir} -name "*-plan.md" -o -name "*-dev-plan.md"
```

### Look for Partial Implementation
Check if code mentioned in plan exists:
- Read plan to identify target files
- Check if files exist
- Check if tests exist

**Determine Starting Phase:**

**If PRD exists:**
- Skip Phase 2 (Define Requirements)
- Start at Phase 3 (Review Requirements)

**If Plan exists:**
- Skip Phases 2-4
- Start at Phase 5 (Decide Execution)

**If Partial Implementation exists:**
- Skip Phases 2-8
- Start at Phase 9 (Implement) - resume mode

**UPDATE STATE AND LOG:**

**Example: PRD exists, no plan**
```markdown
**WORKFLOW STATE:**
- Current Phase: Phase 3 - Review Requirements
- Status: STARTED
- Next Phase: Phase 4 - Plan Implementation
- Blocking Issues: None
- Artifacts: {"prd": "specs/2026-02-14-feature-prd.md"}

**STATUS LOG:**
[10:01] Phase 1 - Detect State [COMPLETED] → Found PRD, no plan
[10:01] Phase 2 - Define Requirements [SKIPPED] → PRD exists at specs/2026-02-14-feature-prd.md
[10:01] Phase 3 - Review Requirements [STARTED]
```

**TRANSITION:** Proceed to detected starting phase

---

## Phase 1: Define Requirements

**Only execute if no PRD exists.**

Create the Product Requirements Document:
```
/feature-define
```

**Output:** PRD file in {spec-dir}/

---

## Phase 2: Review Requirements

**User Review Gate**

Present the PRD to the user:
1. Read the generated PRD
2. Summarize key requirements
3. Ask user: "Does this PRD capture the requirements correctly?"

**If user requests changes:**
- Invoke `/feature-define-review` with their feedback
- Return to this phase

**If user approves:**
- Proceed to Phase 3

---

## Phase 3: Plan Implementation

**Only execute if no plan exists.**

Create the development plan from PRD:
```
/feature-plan <prd-file>
```

**Output:** Plan file in {spec-dir}/

---

## Phase 4: Review Plan

**User Review Gate**

Present the plan to the user:
1. Read the generated plan
2. Summarize:
   - Task breakdown
   - Dependencies
   - Test strategy
3. Ask user: "Does this plan look good?"

**If user requests changes:**
- Invoke `/feature-plan-review` with their feedback
- Return to this phase

**If user approves:**
- Proceed to Phase 5

---

## Phase 5: Decide Execution Strategy

**Analyze the plan to determine execution approach.**

### Check for Parallelization Opportunities

Read the plan and identify:
- Can tasks be done independently?
- Are there multiple developers available?
- Are tasks cleanly separated?

**If yes → Parallel execution:**
- Proceed to Phase 6 (Parallel Path)

**If no → Sequential execution:**
- Proceed to Phase 7 (Propose Implementation)

---

## Phase 6: Parallel Execution Path

**Execute via parallel orchestration:**

```
/parallel-orchestrate plan <plan-file>
```

Inform user:
```
This feature can be parallelized. I've created a parallel plan.
Run `/parallel-orchestrate setup` to create worktrees.
Then start worker sessions with `/parallel-task-workflow` in each worktree.
```

**Stop here** - parallel execution happens in separate sessions.

---

## Phase 7: Propose Implementation

**Design the implementation approach WITHOUT writing code.**

### Analyze Requirements
- Read PRD and plan
- Identify affected files
- Determine integration points

### Design Approach
Present to user:
1. **File structure** - What files will be created/modified
2. **Module dependencies** - Import structure
3. **Key functions** - Function signatures (no implementation)
4. **Test strategy** - What tests will verify behavior

**Output:** Implementation proposal for user review

---

## Phase 8: Review Proposal

**User Review Gate**

Ask user:
- "Does this implementation approach make sense?"
- "Any concerns about modularity or architecture?"
- "Should I proceed with implementation?"

**If user requests changes:**
- Revise proposal
- Return to Phase 7

**If user approves:**
- Proceed to Phase 9

---

## Phase 9: Implement

**Execute the implementation:**

```
/feature-impl <plan-file>
```

Follow TDD implementation process:
1. Research existing code
2. Design structure
3. Write tests first
4. Implement feature
5. Run tests
6. Document

**Output:** Implemented feature with passing tests

---

## Phase 10: Review Modularity

**Evaluate code quality against modularity criteria.**

### Cohesion Check
- Does each module have a single, clear purpose?
- Is related functionality grouped together?

### Coherence Check
- Do module boundaries make sense?
- Is the purpose of each file/class clear?

### Coupling Check
- Are dependencies minimal?
- Can modules be tested independently?
- Are interfaces clean and simple?

**Present findings to user:**
```
Modularity Assessment:
✓/✗ Cohesion: [explanation]
✓/✗ Coherence: [explanation]
✓/✗ Loose Coupling: [explanation]

Recommendations:
- [any refactoring suggestions]
```

**If modularity issues found:**
- Ask user if they want to refactor
- If yes: invoke `/refactor-workflow`

**Output:** Feature complete with modularity verified

---

## Notes

- This skill orchestrates other skills - it does NOT implement functionality directly
- State detection allows resuming from any phase
- Review gates ensure user alignment before proceeding
- Can switch to parallel execution when appropriate
- Modularity evaluation happens after implementation
```

### Template 2: bugfix-workflow

**File:** `{tool-config}/skills/bugfix-workflow/SKILL.md`

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
```bash
find {spec-dir} -name "*-hypotheses.md" -o -name "*-debug.md"
```

If exists:
- Read to see what's been tested
- Skip to appropriate phase

### Look for Bug Report
```bash
find {spec-dir} -name "*-bug.md" -o -name "*-issue.md"
```

If exists but no hypotheses:
- Start from Phase 1 (Reproduce)

**Output:** Starting phase

---

## Phase 1: Reproduce Issue

**Confirm the bug exists and is reproducible.**

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

**Output:** Confirmed reproduction steps

---

## Phase 2: Generate Hypotheses

**Create 3+ potential root causes.**

Based on the bug symptoms, generate hypotheses:

### Example Format:
```markdown
## Hypothesis 1: [Name]
**Theory:** [What might be causing this]
**Test:** [How to verify this hypothesis]
**Evidence Needed:** [What would confirm/refute this]

## Hypothesis 2: [Name]
...

## Hypothesis 3: [Name]
...
```

**Requirements:**
- At least 3 distinct hypotheses
- Each with a testable prediction
- Cover different potential root causes

**Output:** Hypotheses document in {spec-dir}/

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

---

## Phase 4: Test Hypotheses

**Systematically test each hypothesis.**

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
```

**Output:** Test results for each hypothesis

---

## Phase 5: Identify Root Cause

**Synthesize findings to identify the actual cause.**

Based on test results:
1. Which hypothesis was confirmed?
2. What is the underlying issue?
3. Why did this happen?
4. What is the minimal fix?

**Present findings:**
```markdown
## Root Cause Analysis

**Confirmed Hypothesis:** [Which one]

**Root Cause:** [The actual problem]

**Why It Happened:** [Context, explanation]

**Proposed Fix:** [Minimal change to fix]

**Test Strategy:** [How to verify fix works]
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

---

## Phase 7: Implement Fix

**Execute the minimal fix:**

```
/bugfix-impl <root-cause-analysis-file>
```

Follow hypothesis-based implementation:
1. Implement minimal fix
2. Add regression test
3. Verify fix works
4. Ensure no side effects

**Output:** Bug fixed with passing tests

---

## Phase 8: Verify Fix

**Confirm the issue is resolved.**

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

**Output:** Verified bug fix

---

## Notes

- Always generate at least 3 hypotheses
- Test systematically - don't skip to a "likely" cause
- Document all findings for future reference
- Minimal fix is preferred over large refactors
- Root cause document serves as historical record
```

### Template 3: refactor-workflow

**File:** `{tool-config}/skills/refactor-workflow/SKILL.md`

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
find {spec-dir} -name "*-refactor.md" -o -name "*-refactor-plan.md"
```

If plan exists:
- Read to understand refactoring scope
- Skip to Phase 3 (Preserve Tests)

**Output:** Starting phase

---

## Phase 1: Plan Refactoring

**Document refactoring scope and goals.**

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
- [Modularity issue 1]
- [Modularity issue 2]
- [Coupling problem]

**Goals:**
- [Improved structure]
- [Better separation of concerns]
- [Reduced coupling]

**Approach:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Success Criteria:**
- [How to measure improvement]
- Tests still pass
- Functionality unchanged
```

**Output:** Refactoring plan in {spec-dir}/

---

## Phase 2: Review Plan

**User Review Gate**

Present the refactoring plan:
1. Explain current problems
2. Describe proposed changes
3. Show step-by-step approach
4. Ask: "Does this refactoring plan make sense?"

**If user requests changes:**
- Update plan
- Return to this phase

**If user approves:**
- Proceed to Phase 3

---

## Phase 3: Preserve Tests

**Ensure test coverage before making changes.**

### Check Test Coverage
- Identify tests for target code
- Verify tests pass currently
- Note any missing coverage

### Add Tests if Needed
If coverage is insufficient:
- Write additional tests
- Focus on behavior preservation
- Ensure all tests pass

**Output:** Full test suite passing

---

## Phase 4: Refactor Structure

**Execute the refactoring:**

```
/refactor-impl <refactor-plan-file>
```

Follow safe refactoring process:
1. Make small, incremental changes
2. Run tests after each change
3. Preserve functionality
4. Improve structure

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
- Fix refactoring issues
- Re-run tests

**If tests pass:**
- Proceed to Phase 6

**Output:** All tests passing

---

## Phase 6: Review Modularity

**Measure modularity improvement.**

### Before vs After Comparison

**Cohesion:**
- Before: [description]
- After: [description]
- ✓/✗ Improved?

**Coherence:**
- Before: [description]
- After: [description]
- ✓/✗ Improved?

**Coupling:**
- Before: [description]
- After: [description]
- ✓/✗ Improved?

### Present Assessment
```
Modularity Improvement Assessment:
✓/✗ Cohesion improved
✓/✗ Coherence improved
✓/✗ Coupling reduced

Overall: SUCCESS / NEEDS MORE WORK
```

**If needs more work:**
- Ask user if they want another iteration
- If yes: Return to Phase 1

**If successful:**
- Proceed to Phase 7

---

## Phase 7: Document Changes

**Update architecture documentation if needed.**

### Check for Architecture Docs
```bash
find . -name "ARCHITECTURE.md" -o -name "README.md"
```

### Update if Needed
If structure changed significantly:
- Update architecture diagrams
- Revise module descriptions
- Update import examples

**Output:** Documentation updated

---

## Notes

- Always preserve tests before refactoring
- Make incremental changes
- Run tests frequently
- Measure modularity improvement
- Functionality must remain unchanged
```

---

## Phase 4: Integration Instructions

How workflow skills compose other skills.

### Composition Pattern

Workflow skills use **direct delegation** to other skills:

```markdown
Phase N: [Phase Name]
Execute lower-level skill:
```
/skill-name <args>
```

Wait for completion, then proceed to next phase.
```

### State Detection Pattern

All workflows start with Phase 0 to detect state:

```markdown
## Phase 0: Detect Current State

Look for existing artifacts:
- PRDs, plans, code, etc.

If found:
- Skip completed phases
- Resume from appropriate point

Output: Starting phase
```

### Review Gate Pattern

Workflows include review gates between major phases:

```markdown
## Phase N: Review [Artifact]

**User Review Gate**

Present [artifact] to user:
1. Summarize key points
2. Ask for feedback
3. Wait for approval

If changes needed:
- Update artifact
- Return to this phase

If approved:
- Proceed to next phase
```

### Execution Flow

```
feature-workflow
├─> Phase 0: Detect state
│   └─> Found PRD? Skip to Phase 3
├─> Phase 1: /feature-define
├─> Phase 2: Review with user
├─> Phase 3: /feature-plan
├─> Phase 4: Review with user
├─> Phase 5: Decide parallel vs sequential
├─> Phase 6: /parallel-orchestrate OR continue
├─> Phase 7: Propose (no code)
├─> Phase 8: Review with user
├─> Phase 9: /feature-impl
└─> Phase 10: Review modularity
```

---

## Phase 5: Testing & Validation

### Test 1: Feature Workflow - Full Path

Test complete feature development from scratch:

```bash
# Start with no artifacts
feature-workflow

# Should execute:
# - State detection (nothing found)
# - feature-define
# - Review gate
# - feature-plan
# - Review gate
# - Propose implementation
# - Review gate
# - feature-impl
# - Modularity review
```

**Success criteria:**
- All phases execute in order
- Review gates wait for user input
- Final code is modular and tested

### Test 2: Feature Workflow - Resume from Plan

Test state detection and resumption:

```bash
# Create a PRD manually
# Then run:
feature-workflow

# Should:
# - Detect existing PRD
# - Skip to planning phase
# - Continue from there
```

**Success criteria:**
- PRD detected correctly
- Definition phase skipped
- Workflow resumes at correct phase

### Test 3: Bugfix Workflow - Hypothesis Testing

Test hypothesis-based debugging:

```bash
bugfix-workflow

# Should:
# - Reproduce issue
# - Generate 3+ hypotheses
# - Test each systematically
# - Identify root cause
# - Implement minimal fix
```

**Success criteria:**
- At least 3 hypotheses generated
- Each hypothesis tested
- Root cause identified
- Fix implemented and verified

### Test 4: Refactor Workflow

Test safe refactoring:

```bash
refactor-workflow

# Should:
# - Create refactoring plan
# - Preserve tests
# - Refactor incrementally
# - Verify tests still pass
# - Measure modularity improvement
```

**Success criteria:**
- Tests pass before and after
- Modularity improved
- Functionality preserved

---

## Troubleshooting

### Issue: Workflow skipped phases incorrectly

**Check:**
- State detection logic
- Artifact naming patterns
- Search paths for existing files

### Issue: Review gates not waiting

**Check:**
- User approval prompt clear?
- Workflow proceeding without input?
- Review gate implementation

### Issue: Lower-level skill not found

**Check:**
- Prerequisite suites installed?
- Skill names match exactly?
- Path to skills correct?

---

## Customization

### Adapting to Your Project

1. **Spec directory**: Replace `{spec-dir}` throughout
2. **Review gates**: Adjust approval questions to your team's needs
3. **Modularity criteria**: Customize cohesion/coupling standards
4. **State detection**: Add project-specific artifact patterns

### Adding Custom Phases

To add a phase to a workflow:
1. Insert between existing phases
2. Add corresponding review gate if needed
3. Update phase numbering
4. Test the modified workflow

---

## Summary

**This suite provides:**
- ✅ End-to-end workflow orchestration
- ✅ State detection and resumption
- ✅ User review gates between phases
- ✅ Composition of lower-level skills
- ✅ Modularity evaluation
- ✅ Hypothesis-based debugging

**Key skills:**
- `feature-workflow` - Complete feature development
- `bugfix-workflow` - Systematic debugging
- `refactor-workflow` - Safe refactoring

**Prerequisites met:**
- Feature definition, planning, and implementation suites installed
- Spec directory configured
- Understanding of skill composition

**Next steps:**
1. Test each workflow with real features/bugs
2. Customize review gates and modularity criteria
3. Train team on workflow usage
4. Collect feedback and iterate
