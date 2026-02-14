# Plan Development Setup

Setup instructions for `feature-plan` and `feature-plan-review` skills - transforming PRDs into development plans.

---

## Overview

The **Feature Planning** skills transform Product Requirements Documents into comprehensive development plans with task decomposition, technical designs, and system-level OKRs.

**Skills in this guide:**
- `feature-plan` - Transform PRD into development plan
- `feature-plan-review` - Review and validate development plans

**What they produce:**
- Development plan with design options, task breakdown, dependencies, and system OKRs

**When to use:**
- After PRD is approved
- Before implementation begins
- Need to break work into parallel tasks
- Creating technical design from requirements

**When NOT to use:**
- Development plan already exists (workflow skills detect and skip)
- Implementing a simple change that doesn't need planning
- Fixing a small bug (use bugfix-workflow instead)

---

## Phase 0: Repository Detection

Follow the standard repository detection process from [../shared/repo-detection.md](../shared/repo-detection.md).

**Suite-specific checks:**
- Verify PRDs exist (inputs for planning)
- Find where plans are stored
- Review existing plan formats
- Understand codebase structure for technical design

---

## Phase 1: Validate Prerequisites

Follow the standard prerequisites validation from [../shared/prerequisites.md](../shared/prerequisites.md).

**Suite-specific prerequisites:**
- PRDs available for planning
- Access to codebase for design decisions

**Skill directories to create:**
- `${TOOL_CONFIG}/skills/feature-plan/`
- `${TOOL_CONFIG}/skills/feature-plan-review/`

---

## Phase 2: Define Skills

### Skill 1: `feature-plan`

**Purpose:** Transform PRD into comprehensive development plan with task decomposition and system-level OKRs.

**Phases:**
0. Intake and Context - Read PRD and gather project context
1. Question Resolution - Clarify ambiguities and uncertainties
2. Risk and Complexity Analysis - Identify technical risks
3. Structural Design Options - Propose multiple design approaches
4. Task Decomposition - Break into parallelizable tasks with dependencies
5. System-Level OKRs - Define technical success criteria
6. Detailed Development Plan - Elaborate implementation details
7. Write Plan Document - Save to spec directory

**Output:** `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`

### Skill 2: `feature-plan-review`

**Purpose:** Review and validate development plan for completeness and feasibility.

**Phases:**
1. Read Plan
2. Validate Task Decomposition - Check dependencies and parallelization
3. Validate Design Options - Assess feasibility and trade-offs
4. Validate System OKRs - Verify measurability
5. Identify Gaps - Missing details or concerns
6. Provide Feedback - Actionable suggestions

**Output:** Review report with approval status or required changes

---

## Phase 3: SKILL.md Templates

### Template: feature-plan

**File:** `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`

```markdown
---
name: feature-plan
description: Transform PRD into comprehensive development plan with task decomposition and system OKRs
disable-model-invocation: true
argument-hint: "[path-to-prd or PRD description]"
---

# Feature Plan

Transform a Product Requirements Document into an actionable development plan.

## When to Use

Use when:
- Have an approved PRD
- Need to create technical design
- Breaking work into parallel tasks
- Before implementation begins

Do NOT use when:
- Creating the PRD (use feature-define)
- Already have a development plan
- Implementing (use feature-impl)

## Input

- PRD file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`)
- OR PRD description text

## Output

Development plan at `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md` containing:
- Clarified requirements
- Risk assessment
- Design options with recommendation
- Task breakdown with dependencies
- System-level OKRs
- Implementation roadmap

## Process

### Phase 0: Intake and Context

**Purpose:** Understand the PRD and gather project context.

1. **Read the PRD**
   - If argument is file path, read it
   - If argument is text, capture it
   - If no argument, ask user to provide PRD

2. **Gather Project Context**
   - Read project conventions (CLAUDE.md, README.md)
   - Review similar features in codebase
   - Check framework documentation
   - Identify relevant patterns

3. **Initial Understanding Check**
   - Summarize PRD in 2-3 sentences
   - List key requirements
   - Note immediate questions

**Gate:** User confirms understanding is correct.

---

### Phase 1: Question Resolution

**Purpose:** Identify and resolve ambiguities in requirements.

1. **Identify Questions**
   Review PRD for ambiguities in:
   - Scope (in vs. out?)
   - Requirements (must-have vs. nice-to-have?)
   - Behavior (what happens when X?)
   - Integration (how interact with existing?)
   - Constraints (performance? compatibility?)

2. **Check for Better Designs**
   Even if requirements seem clear:
   - Is there a simpler approach?
   - Are there existing patterns to follow?
   - Would a different architecture be better?

3. **Present Questions**
   Group by priority:
   - **Critical** (blocking design)
   - **Important** (affects design options)
   - **Nice to clarify** (minor impact)

4. **Document Answers**
   Update understanding with clarified requirements.

**Gate:** All critical questions answered.

---

### Phase 2: Risk and Complexity Analysis

**Purpose:** Identify technical risks and complexity factors.

1. **Identify Risks**
   - Technical uncertainty (unfamiliar tech?)
   - Integration risk (multiple systems?)
   - Performance risk (scalability?)
   - Complexity risk (complex algorithms?)
   - Dependency risk (third-party?)

2. **Assess Complexity**
   Rate each component:
   - **Simple:** Straightforward, well-understood
   - **Moderate:** Some complexity, patterns exist
   - **Complex:** Novel approach, unknowns

3. **Research Mitigation**
   For each risk:
   - Research solutions
   - Consider prototypes/spikes
   - Identify fallback approaches

**Output:**
```markdown
## Risk Assessment

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| [Risk 1] | High | Medium | [Strategy] |

## Complexity
- Component A: Simple
- Component B: Complex - requires new approach
```

**Gate:** User reviews risk assessment.

---

### Phase 3: Structural Design Options

**Purpose:** Propose multiple design options with trade-offs.

1. **Design File Hierarchy**
   For each option, show:
   ```
   Option A: [Name]
   path/to/module.py
     class MainClass
       method_1()
     class HelperClass
   ```

2. **Create Dependency Graphs**
   Show how modules relate:
   ```
   MainClass
     └─> HelperClass
     └─> ExistingClass
   ```

3. **Propose 2-3 Options**
   For each:
   - Approach description
   - File structure
   - Dependencies
   - Trade-offs (pros/cons)

4. **Recommend an Option**
   Clear recommendation with rationale.

**Gate:** User selects or requests refinement.

---

### Phase 4: Task Decomposition

**Purpose:** Break PRD into parallelizable tasks with dependencies.

1. **Identify Task Boundaries**
   Look for tasks that:
   - Can be implemented independently
   - Have clear interfaces
   - Can be tested in isolation

2. **Define Tasks**
   For each task:
   ```markdown
   ### Task: task-name
   **Goal:** [One sentence]
   **Scope:** [What's included]
   **Complexity:** Simple | Moderate | Complex
   ```

3. **Analyze Dependencies**
   For each task:
   - **Depends On:** Which tasks must complete first
   - **Blocks:** Which tasks wait for this one

4. **Create Dependency Graph**
   ```
   Wave 1 (No dependencies):
   ├─ task-data-models
   └─ task-api-spec

   Wave 2 (Depends on Wave 1):
   ├─ task-business-logic
   │  └─ Depends on: task-data-models
   └─ task-ui-views
      └─ Depends on: task-api-spec

   Wave 3:
   └─ task-integration
      └─ Depends on: Wave 2
   ```

5. **Simplify Dependencies**
   - Break long chains
   - Define interfaces to decouple
   - Create foundation tasks

**Gate:** User approves task breakdown.

---

### Phase 5: System-Level OKRs

**Purpose:** Define technical success criteria (complement PRD's product OKRs).

1. **Define Technical Objective**
   One clear objective:
   "Deliver a scalable batch moderation system"

2. **Write System-Level Key Results**
   3-5 technical outcomes:
   - KR1: System processes 1000+ ops/sec with <100ms latency
   - KR2: All components have >80% test coverage
   - KR3: API handles 10x traffic spike
   - KR4: Code follows project conventions

   **Characteristics:**
   - System capabilities, not implementation
   - Performance/quality targets
   - Operational characteristics

3. **Map Tasks to OKRs**
   Show which tasks validate which KRs.

**Gate:** User approves system OKRs.

---

### Phase 6: Detailed Development Plan

**Purpose:** Create implementation roadmap for each task.

1. **Elaborate Each Task**
   ```markdown
   ### Task: task-name

   **Implementation Steps:**
   1. Define base classes
   2. Add validation
   3. Write tests

   **Acceptance Criteria:**
   - All tests pass
   - 80%+ coverage
   - Follows conventions

   **Effort:** Simple | Moderate | Complex
   ```

2. **Define Interfaces**
   Key function signatures and contracts.

3. **Identify Dependencies**
   External libraries, APIs, services needed.

4. **Create Test Strategy**
   - Unit tests
   - Integration tests
   - Manual testing

**Gate:** User reviews plan.

---

### Phase 7: Write Plan Document

**Purpose:** Save formal document.

Create: `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`

```markdown
# [Feature Name] Development Plan

**Date:** YYYY-MM-DD
**Status:** Approved
**PRD:** [link to PRD]

## Overview
[Summary]

## Requirements
[Clarified from Phase 1]

## System-Level OKRs
[From Phase 5]

## Risk Assessment
[From Phase 2]

## Design
[Selected option from Phase 3]

## Task Decomposition
[From Phase 4]

## Implementation Details
[From Phase 6]

## Test Strategy
[Testing approach]

## Acceptance Criteria
[Success criteria]
```

---

## Best Practices

### Question Resolution
- Ask early, don't discover during implementation
- Be specific
- Prioritize blocking vs. nice-to-clarify

### Design Options
- Show trade-offs for each
- Use existing patterns
- Consider maintainability

### Task Decomposition
- Maximize parallel work
- Keep dependencies simple
- Use interfaces to decouple
- Right-size tasks

### System OKRs
- Outcome-oriented, not implementation
- Measurable and verifiable
- Complement PRD's product OKRs
```

---

### Template: feature-plan-review

**File:** `${TOOL_CONFIG}/skills/feature-plan-review/SKILL.md`

```markdown
---
name: feature-plan-review
description: Review and validate development plan for completeness and feasibility
disable-model-invocation: true
argument-hint: "[path-to-plan-file]"
---

# Feature Plan Review

Review a development plan for completeness, feasibility, and quality.

## Input

Path to plan file (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`)

## Output

Review report with:
- Validation results
- Identified gaps
- Actionable feedback
- Approval status

## Process

### Phase 1: Read Plan

Read the plan and extract all sections.

### Phase 2: Validate Task Decomposition

Check:
- [ ] Tasks are atomic and testable
- [ ] Dependencies clearly specified
- [ ] Dependency graph shows waves
- [ ] Independent tasks can parallelize
- [ ] No circular dependencies
- [ ] Each task has clear deliverables

### Phase 3: Validate Design Options

Check:
- [ ] At least 2 options presented
- [ ] Trade-offs documented
- [ ] Recommendation has rationale
- [ ] Aligns with codebase patterns

### Phase 4: Validate System OKRs

Check:
- [ ] Outcome-oriented (not implementation)
- [ ] Measurable
- [ ] All tasks map to OKRs
- [ ] Complements PRD OKRs

### Phase 5: Identify Gaps

Check for:
- Unresolved questions
- Missing risk mitigation
- Undefined interfaces
- Missing acceptance criteria

### Phase 6: Provide Feedback

```markdown
# Development Plan Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** Approved / Needs Changes

## Task Decomposition
- [x] Tasks are atomic
- [ ] Issue: Task 3 has circular dependency

## Design Validation
- [x] Multiple options
- [x] Trade-offs clear

## System OKRs
- [x] Outcome-oriented
- [ ] Issue: KR2 is implementation detail

## Approval Decision
- [ ] Approved
- [x] Needs Changes
```
```

---

## Phase 4: Integration

### How These Skills Work Together

```
feature-define (PRD)
     │
     └──► feature-plan
              │
              └──► feature-plan-review
                        │
                        └──► Approved Plan
                                  │
                                  ├──► feature-impl (sequential)
                                  └──► parallel-orchestrate (parallel)
```

---

## Summary

**Skills:** `feature-plan`, `feature-plan-review`

**Purpose:** Transform PRDs into actionable development plans

**Key features:**
- Question resolution
- Risk assessment
- Multiple design options
- Task decomposition with dependencies
- System-level OKRs
- Review validation

**Locations:**
- `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`
- `${TOOL_CONFIG}/skills/feature-plan-review/SKILL.md`
