# Feature Workflow Setup

Setup instructions for the `feature-workflow` skill - an end-to-end feature development orchestrator.

---

## Overview

The **feature-workflow** skill orchestrates complete feature development from requirements to implementation. It detects current state, composes lower-level skills, and adds review gates between phases.

**Skill name:** `feature-workflow`

**What it does:**
1. Detects what already exists (PRD, plan, code)
2. Creates PRD if needed (calls `feature-define`)
3. Creates plan if needed (calls `feature-plan`)
4. Decides parallel vs sequential execution
5. Implements via `feature-impl` or `parallel-orchestrate`
6. Reviews modularity after implementation

**Prerequisites:**
- `feature-define` and `feature-define-review` skills set up
- `feature-plan` and `feature-plan-review` skills set up
- `feature-impl` skill set up
- Spec directory configured

---

## Phase 0: Repository Detection

Follow the standard repository detection process from [../shared/repo-detection.md](../shared/repo-detection.md).

**Workflow-specific checks:**
- Verify prerequisite skills exist
- Check for existing PRDs and plans
- Understand current project state

---

## Phase 1: Validate Prerequisites

Follow the standard prerequisites validation from [../shared/prerequisites.md](../shared/prerequisites.md).

**Workflow-specific prerequisites:**

```bash
# Check prerequisite skills exist
ls ${TOOL_CONFIG}/skills/feature-define/SKILL.md
ls ${TOOL_CONFIG}/skills/feature-plan/SKILL.md
ls ${TOOL_CONFIG}/skills/feature-impl/SKILL.md
```

If missing, set up those skills first using:
- [define-requirements-setup.md](define-requirements-setup.md)
- [plan-development-setup.md](plan-development-setup.md)
- [implement-feature-setup.md](implement-feature-setup.md)

**Skill directory to create:**
- `${TOOL_CONFIG}/skills/feature-workflow/`

---

## Phase 2: Define Skill

### Skill Specification

**Name:** `feature-workflow`

**Purpose:** End-to-end feature development orchestrator with state detection and review gates.

**Phases:**
1. Detect State - Check what already exists
2. Define Requirements - Create PRD if needed
3. Review Requirements - User approval gate
4. Plan Implementation - Create plan if needed
5. Review Plan - User approval gate
6. Decide Execution - Parallel or sequential?
7. Propose Implementation - Design approach
8. Review Proposal - User approval gate
9. Implement - Execute via feature-impl
10. Review Modularity - Verify quality

**Output:** Complete feature with passing tests and modularity verification

---

## Phase 3: SKILL.md Template

**File:** `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md`

```markdown
---
name: feature-workflow
description: End-to-end feature development workflow with state detection and review gates
disable-model-invocation: true
---

# Feature Workflow

Complete feature development orchestrator from requirements to implementation.

## Phase 0: Detect Current State

**Check what already exists:**

### Look for PRD
Search for existing requirements documents:
- Check ${SPEC_DIR} for *-prd.md files
- If found: Read it and skip to Phase 3 (Plan Implementation)

### Look for Plan
Search for existing development plans:
- Check ${SPEC_DIR} for *-plan.md files
- If found: Read it and skip to Phase 5 (Decide Execution)

### Look for Partial Implementation
Check if code mentioned in plan exists:
- If partial: Resume from where work stopped

**Output:** Starting phase for this workflow

---

## Phase 1: Define Requirements

**Only execute if no PRD exists.**

Create the Product Requirements Document:
```
/feature-define
```

**Output:** PRD file in ${SPEC_DIR}/

---

## Phase 2: Review Requirements

**User Review Gate**

Present the PRD to the user:
1. Read the generated PRD
2. Summarize key requirements
3. Ask: "Does this PRD capture the requirements correctly?"

**If changes requested:** Update and return to this phase
**If approved:** Proceed to Phase 3

---

## Phase 3: Plan Implementation

**Only execute if no plan exists.**

Create the development plan from PRD:
```
/feature-plan <prd-file>
```

**Output:** Plan file in ${SPEC_DIR}/

---

## Phase 4: Review Plan

**User Review Gate**

Present the plan:
1. Summarize task breakdown
2. Show dependencies
3. Explain test strategy
4. Ask: "Does this plan look good?"

**If changes requested:** Update and return to this phase
**If approved:** Proceed to Phase 5

---

## Phase 5: Decide Execution Strategy

**Analyze the plan to determine execution approach.**

Check for parallelization opportunities:
- Can tasks be done independently?
- Are there multiple tasks with no dependencies?
- Are tasks cleanly separated?

**If yes → Parallel execution:**
```
/parallel-orchestrate plan <plan-file>
```
Stop here - parallel execution happens in separate sessions.

**If no → Sequential execution:**
Proceed to Phase 6

---

## Phase 6: Propose Implementation

**Design the implementation approach WITHOUT writing code.**

Present to user:
1. **File structure** - What files will be created/modified
2. **Module dependencies** - Import structure
3. **Key functions** - Function signatures (no implementation)
4. **Test strategy** - What tests will verify behavior

**Output:** Implementation proposal for user review

---

## Phase 7: Review Proposal

**User Review Gate**

Ask:
- "Does this implementation approach make sense?"
- "Any concerns about modularity or architecture?"
- "Should I proceed with implementation?"

**If changes requested:** Revise and return to Phase 6
**If approved:** Proceed to Phase 8

---

## Phase 8: Implement

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

## Phase 9: Review Modularity

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

**Present findings:**
```
Modularity Assessment:
✓/✗ Cohesion: [explanation]
✓/✗ Coherence: [explanation]
✓/✗ Loose Coupling: [explanation]

Recommendations:
- [any refactoring suggestions]
```

**If modularity issues found:** Ask if user wants to refactor

**Output:** Feature complete with modularity verified

---

## Notes

- This skill orchestrates other skills - it does NOT implement functionality directly
- State detection allows resuming from any phase
- Review gates ensure user alignment before proceeding
- Can switch to parallel execution when appropriate
```

---

## Phase 4: Integration

### How This Skill Composes Others

```
feature-workflow
├── Calls: feature-define (Phase 1)
├── Calls: feature-define-review (Phase 2, if changes needed)
├── Calls: feature-plan (Phase 3)
├── Calls: feature-plan-review (Phase 4, if changes needed)
├── Calls: parallel-orchestrate (Phase 5, if parallel)
├── Calls: feature-impl (Phase 8, if sequential)
└── May call: refactor-workflow (Phase 9, if modularity issues)
```

### State Detection Flow

```
Start
  │
  ├─► PRD exists? ──────────────────────► Skip to Phase 3
  │         │
  │         └─► Plan exists? ──────────► Skip to Phase 5
  │                   │
  │                   └─► Code exists? ─► Skip to Phase 8
  │
  └─► Nothing exists ──────────────────► Start at Phase 1
```

---

## Phase 5: Testing & Validation

### Test 1: Fresh Start

Start feature-workflow with no existing artifacts.

**Expected:**
- Detects no PRD/plan exist
- Starts from Phase 1 (Define Requirements)
- Goes through all phases with review gates

### Test 2: Resume from PRD

Start feature-workflow when PRD already exists.

**Expected:**
- Detects PRD exists
- Skips to Phase 3 (Plan Implementation)
- Continues from there

### Test 3: Resume from Plan

Start feature-workflow when plan already exists.

**Expected:**
- Detects plan exists
- Skips to Phase 5 (Decide Execution)
- Continues from there

---

## Customization

### Adapting to Your Project

1. **Review gate frequency:** Add or remove review gates based on team needs
2. **Parallel threshold:** Adjust when to suggest parallel execution
3. **Modularity criteria:** Customize quality checks for your codebase
4. **Skill composition:** Modify which skills are called in each phase

---

## Summary

**Skill:** `feature-workflow`

**Purpose:** End-to-end feature development orchestrator

**Key features:**
- State detection (resume from any point)
- Review gates (user approval between phases)
- Skill composition (calls feature-define, feature-plan, feature-impl)
- Parallel execution support
- Modularity evaluation

**Location:** `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md`
