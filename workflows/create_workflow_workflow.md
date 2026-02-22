---
name: create-workflow
description: >
  Use when creating a new workflow (multi-step process composing primitives).
  Triggers on: "create a workflow", "define a new workflow", "add a workflow".
argument-hint: "[workflow name and purpose]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
protocols:
  - tracking
  - recovery
  - checklist_management
  - reasoning_patterns
  - goals_and_objectives
---

# Create Workflow Workflow

**Goal:** Create a reliable, recoverable workflow that orchestrates primitives effectively.

**Intent:** Primitives are atomic cognitive actions. Many tasks require multiple primitives in sequence with progress tracking and recovery. Workflows provide this orchestration layer—they compose primitives while maintaining state.

**Scope:** Workflow creation: determining if a workflow is needed, composing primitives, defining validation gates, and ensuring recoverability.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (declarative goals with validation gates)

---

## Key Results

1. **Workflow is distinct** — serves a purpose no existing workflow serves
2. **Workflow is composable** — orchestrates 2+ primitives effectively
3. **Workflow is recoverable** — can resume from any interruption point
4. **Workflow is trackable** — progress is visible throughout execution
5. **Workflow Key Results are outcome-oriented** — measure what's produced, not steps taken

---

## Available Primitives

`orient`, `define`, `design`, `validate`, `critique`, `brainstorm`

---

## Constraints

- Every workflow MUST reference: tracking, recovery, checklist_management protocols
- Compose primitives, don't repeat their content
- Prefer declarative style (goals + constraints) over imperative (rigid steps)
- Validation gates after steps that produce artifacts

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Is a workflow needed?** — Does this require multiple primitives? Would a single primitive suffice?
2. **What primitives are involved?** — Which from: orient, define, design, implement, validate, investigate, brainstorm, critique?
3. **What are the dependencies?** — Must any step complete before another?
4. **What validation gates?** — Where should progress be verified?
5. **Style?** — Deterministic (fixed path) or Adaptive (context-dependent)?

Output reasoning before proceeding.

---

## Execution

### Phase 1: Understand Existing Workflows

**Primitive:** `orient`

Read existing workflows to understand patterns:

```
workflows/
├── feature_workflow.md          # End-to-end feature development
├── bugfix_workflow.md           # Hypothesis-driven bug fixing
├── refactor_workflow.md         # Safe behavior-preserving refactoring
├── code_review_workflow.md      # Systematic code review
├── test_strategy_workflow.md    # Test design and planning
├── worktree_orchestrate_workflow.md  # Parallel task coordination
└── worktree_task_workflow.md    # Single task in worktree
```

**Key question:** Does this workflow serve a distinct purpose from existing ones?

### Phase 2: Define the Workflow

**Primitive:** `define`

Establish the workflow's identity:

1. **Name** — Descriptive name (e.g., feature-workflow, bugfix-workflow)
2. **Goal** — What the workflow achieves
3. **Intent** — Why it exists, what it prevents
4. **Scope** — What's covered
5. **Type** — Deterministic (fixed path) or Adaptive (context-dependent)
6. **Style** — Imperative, Declarative, or Hybrid

**Workflow Styles:**

- **Declarative (preferred):** Define goal, key results, available primitives, constraints. Execution determines path.
- **Imperative:** Explicit step-by-step sequence. Use when order is strictly required.
- **Hybrid:** Declarative goals with imperative gates (validation checkpoints).

**Gate:** User confirms the workflow purpose is distinct.

### Phase 3: Design the Workflow

**Primitive:** `design`

Plan the workflow structure:

1. **Identify primitives needed** — Which cognitive actions?
2. **Determine sequence/dependencies** — What order? What depends on what?
3. **Add validation gates** — Where to verify before proceeding?
4. **Define iteration behavior** — What happens on validation failure?
5. **Specify customization points** — How to adapt to different contexts?

**Declarative structure (preferred):**
```markdown
## Available Primitives
[list]

## Constraints
[ordering requirements, validation gates]

## Execution
[guidance, not rigid steps]
```

**Gate:** User reviews workflow design.

### Phase 4: Write the Workflow

Create the workflow document in `workflows/`:

**Required sections:**

```markdown
---
name: workflow-name
description: When to use this workflow
argument-hint: "[expected input]"
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Workflow Name

**Goal:** [Outcome]
**Intent:** [Why it exists]
**Scope:** [What's covered]

---

## Workflow Type

**Type:** Deterministic | Adaptive
**Style:** Imperative | Declarative | Hybrid

---

## Key Results

[Binary, verifiable outcomes]

---

## Available Primitives

[List of primitives workflow may use]

---

## Constraints

[Ordering, dependencies, validation requirements]

---

## Expert Reasoning (Required First)

[What to reason about before starting]

---

## Execution

[Phases/steps with primitive references and gates]

---

## Preconditions

**Must be provided:** [Required inputs]
**Self-satisfiable:** [Context workflow gathers]

---

## Postconditions

**Success:** [End state]
**Failure:** [Failure conditions]

---

## Recovery

[How to resume from interruption]

---

## Iteration

[How validation failures are handled]

---

## Customization Points

[How to adapt to different contexts]
```

### Phase 5: Validate

**Primitive:** `validate`

Verify the workflow:

1. **Composes primitives** — References primitives, doesn't repeat their content?
2. **Required protocols** — tracking, recovery, checklist_management referenced?
3. **Validation gates** — Present after artifact-producing steps?
4. **Iteration handling** — Clear what happens on failure?
5. **Recoverable** — Can resume from interruption?
6. **Follows template** — All required sections present?

**On failure:** Revise and re-validate.

---

## Preconditions

**Must be provided:** Workflow name and purpose (ask if unclear)

**Self-satisfiable:** Existing workflows (read from `workflows/`)

---

## Postconditions

**Success:** New workflow document created in `workflows/`, validated.

**Failure:** Workflow duplicates existing one, doesn't need multiple primitives, or user aborts.

---

## Recovery

Per `recovery` protocol — check for partial workflow document, resume from last completed section.

---

## Anti-Patterns

**Repeating primitive content:** Reference the primitive, add workflow-specific context only.

**Silent iteration:** When validation fails, explicitly update checklist—don't silently loop.

**Over-specification:** Let primitives do their job; don't dictate their internal actions.

**Single primitive:** If only one primitive is needed, use that primitive directly.

**Missing protocols:** Every workflow needs tracking, recovery, checklist_management.
