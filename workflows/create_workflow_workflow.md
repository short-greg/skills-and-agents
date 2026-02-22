---
name: create-workflow
description: >
  Use when creating a new workflow (multi-step process composing primitives).
  Triggers on: "create a workflow", "define a new workflow", "add a workflow".
argument-hint: "[workflow name and purpose]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, TodoWrite
---

# Create Workflow Workflow

**Goal:** Create a reliable, recoverable workflow that orchestrates primitives effectively.

**Intent:** Primitives are atomic cognitive actions. Many tasks require multiple primitives in sequence with progress tracking and recovery. Workflows provide this orchestration layer—they compose primitives while maintaining state.

**Scope:** Workflow creation: determining if a workflow is needed, composing primitives, defining validation gates, and ensuring recoverability.

---

## Key Results

1. Need for workflow is reasoned about before creating
2. Workflow is distinct — serves a purpose no existing workflow serves
3. Workflow is composable — orchestrates 2+ primitives effectively
4. Workflow is recoverable — can resume from any interruption point
5. Workflow is trackable — progress is visible throughout execution
6. Workflow Key Results are outcome-oriented — measure what's produced, not steps taken

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which sections of the workflow are complete.
- `recovery.md` — On startup, check for partial workflow document. Resume from last completed section.
- `checklists.md` — Create a checklist based on required sections. Update dynamically.
- `reasoning.md` — Reason about whether workflow is needed before creating.
- `goals_and_objectives.md` — Ensure workflow key results are outcome-oriented.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to create the new workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand existing workflows and patterns.
- `define` — Establish the new workflow's identity and purpose.
- `design` — Plan the workflow structure and primitive composition.
- `validate` — Verify the workflow meets all criteria.
- `critique` — Review the workflow for issues.
- `brainstorm` — Generate options for structure and approach.

---

## Constraints

- Every workflow MUST reference: tracking, recovery, checklists protocols
- Compose primitives, don't repeat their content
- Prefer declarative style (goals + constraints) over imperative (rigid steps)
- Validation gates after steps that produce artifacts

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Need (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Is a workflow needed?** — Does this require multiple primitives? Would a single primitive suffice?
2. **What primitives are involved?** — Which from: orient, define, design, implement, validate, investigate, brainstorm, critique?
3. **What are the dependencies?** — Must any step complete before another?
4. **What validation gates?** — Where should progress be verified?
5. **Style?** — Deterministic (fixed path) or Adaptive (context-dependent)?

Output your reasoning.

### Understand Existing Workflows (→ KR2)

Use `orient` primitive. Read existing workflows to understand patterns:

- feature_workflow.md, bugfix_workflow.md, refactor_workflow.md
- code_review_workflow.md, test_strategy_workflow.md
- worktree_orchestrate_workflow.md, worktree_task_workflow.md

**Key question:** Does this workflow serve a distinct purpose from existing ones?

### Define the Workflow (→ KR2)

Use `define` primitive. Establish the workflow's identity:

1. **Name** — Descriptive name (e.g., feature-workflow, bugfix-workflow)
2. **Goal** — What the workflow achieves
3. **Intent** — Why it exists, what it prevents
4. **Scope** — What's covered
5. **Type** — Deterministic (fixed path) or Adaptive (context-dependent)
6. **Style** — Imperative, Declarative, or Hybrid

**Gate:** Ask user to confirm the workflow purpose is distinct.

### Design the Workflow (→ KR3, KR4, KR5)

Use `design` primitive. Plan the workflow structure:

1. **Identify primitives needed** — Which cognitive actions?
2. **Determine sequence/dependencies** — What order? What depends on what?
3. **Add validation gates** — Where to verify before proceeding?
4. **Define iteration behavior** — What happens on validation failure?
5. **Specify customization points** — How to adapt to different contexts?

**Gate:** Ask user to review workflow design.

### Write the Workflow (→ KR3, KR4, KR5, KR6)

Create the workflow document in `workflows/` following the template in `templates/workflow_template.md`.

**Required sections:**
- Goal, Intent, Scope
- Key Results
- Protocols (with descriptions)
- Available Primitives (with descriptions)
- Constraints
- Tasks (not numbered, not rigid phases)
- Progress Tracking
- Preconditions, Postconditions
- Recovery
- Customization Points

### Validate Workflow (→ KR2, KR3, KR4, KR5)

Use `validate` primitive. Verify the workflow:

1. **Composes primitives** — References primitives, doesn't repeat their content?
2. **Required protocols** — tracking, recovery, checklists referenced?
3. **Validation gates** — Present after artifact-producing steps?
4. **Iteration handling** — Clear what happens on failure?
5. **Recoverable** — Can resume from interruption?
6. **Follows template** — All required sections present?

**On failure:** Revise and re-validate.

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist based on required sections
- Mark items complete immediately after finishing each task
- Report progress after each completed item

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

Per `recovery.md` — check for partial workflow document, resume from last completed section.

---

## Anti-Patterns

**Repeating primitive content:** Reference the primitive, add workflow-specific context only.

**Silent iteration:** When validation fails, explicitly update checklist—don't silently loop.

**Over-specification:** Let primitives do their job; don't dictate their internal actions.

**Single primitive:** If only one primitive is needed, use that primitive directly.

**Missing protocols:** Every workflow needs tracking, recovery, checklists.
