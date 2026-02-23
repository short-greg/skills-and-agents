**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions.**

---
name: create-workflow
description: >
  Use when creating a new workflow (multi-step process composing primitives).
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Workflow need justified and distinct — reasoned about necessity, serves unique purpose not covered by existing workflows
2. Workflow document created — in workflows/, follows template, validated against criteria
3. Workflow is effective — orchestrates 2+ primitives, recoverable, trackable

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for partial workflow document, resume from last section
3. Compose primitives, don't repeat their content — reference primitives, add workflow-specific context only
4. Iterate up to 3 times if validation fails, revise workflow, re-validate

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Workflow name and purpose

**Elicit if not provided:**
- Existing workflows (read from workflows/ to check for duplicates)
- Workflow type preference (Deterministic vs Adaptive, Imperative vs Declarative)

**Optional:** Specific primitives to compose, validation gate preferences

## Postconditions

The resulting state after the skill is finished.

**Success:** New workflow document created in workflows/, validated against all criteria

**Failure:** Workflow duplicates existing one, doesn't need multiple primitives, or user aborts

## Steps

Complete the Tasks in this order.

1. Reason About Need
2. Understand Existing Workflows
3. Define the Workflow
4. Design the Workflow
5. Write the Workflow
6. Validate Workflow

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

### Understand Existing Workflows (→ KR1)

Use `orient` primitive. Read existing workflows to understand patterns and verify uniqueness.

### Define the Workflow (→ KR2)

Use `define` primitive. Establish the workflow's identity:
1. Name, Goal, Intent, Scope
2. Type (Deterministic/Adaptive, Imperative/Declarative/Hybrid)
3. Key Results (2-4 measurable outcomes)
4. Requirements (constraints on execution)

Ask user to confirm the workflow purpose is distinct.

### Design the Workflow (→ KR2, KR3)

Use `design` primitive. Plan the workflow structure:
1. Identify primitives needed
2. Determine sequence/dependencies
3. Add validation gates
4. Define iteration behavior
5. Specify Steps and Tasks with KR mapping

Ask user to review workflow design.

### Write the Workflow (→ KR2)

Create the workflow document in workflows/ following templates/workflow_template.md.

Required sections per template:
- Frontmatter with MUST satisfy instruction
- Goal, Intent, Scope
- Key Results - KR (with imperative)
- Requirements and Constraints - REQ (with imperative, include progress tracking, recovery, iteration)
- Preconditions (Required, Elicit, Optional)
- Postconditions (Success, Failure)
- Steps
- Tasks (with → KR# mapping)
- Available Primitives
- Validation Criteria
- Additional Notes and Terms

### Validate Workflow (→ KR2, KR3)

Use `validate` primitive. Verify the created workflow against all Validation Criteria from templates/workflow_template.md:

1. **Structure:** All sections present with one-line imperatives. Frontmatter complete.
2. **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
3. **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
4. **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
5. **No redundancy:** Each piece of information appears exactly once
6. **Recovery:** Created workflow includes progress tracking, recovery behavior, iteration limit in its Requirements section
7. **Coherent:** Steps flow logically, no contradictions between sections
8. **Concise:** As few words as possible, no duplication
9. **Complete:** All necessary information provided, all KRs achievable from Tasks
10. **Precise:** Specific, unambiguous language, clear definitions

Check each criterion. Report which pass and which fail.

On failure: Revise the workflow to address failures, then re-validate (up to 3 iterations per REQ4).

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand existing workflows and patterns
- `define` — Establish the new workflow's identity and purpose
- `design` — Plan the workflow structure and primitive composition
- `validate` — Verify the workflow meets all criteria
- `critique` — Review the workflow for issues
- `brainstorm` — Generate options for structure and approach

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

**Anti-Patterns to Avoid:**
- Repeating primitive content (reference instead)
- Silent iteration (explicitly update checklist on validation failure)
- Over-specification (let primitives do their job)
- Single primitive workflows (use primitive directly)
- Missing protocols (every workflow needs tracking, recovery, checklists in Requirements)
