# Create Workflow Protocol

Define multi-step processes that compose primitives to accomplish larger goals.

---

## Goal

Enable creation of reliable, recoverable workflows that orchestrate primitives effectively.

---

## Intent

Primitives are atomic cognitive actions. Many tasks require multiple primitives in sequence with progress tracking and recovery from interruption. Workflows provide this orchestration layer—they are higher-order skills that compose primitives while maintaining state.

---

## Scope

This protocol addresses workflow creation: when to create workflows, how to compose primitives, workflow styles (imperative/declarative/hybrid), iteration handling, and required protocol references.

---

## Key Results

- Workflow composes primitives (or custom instructions) effectively
- Progress is tracked and recoverable via protocols
- Iteration is handled through checklist management
- Style matches the task (imperative for strict sequences, declarative for flexibility)

---

## Essential Concepts

### 1. Higher-Order Composition

Workflows compose primitives to accomplish goals that require multiple cognitive actions. A workflow can:
- Invoke primitives by reference
- Provide custom instructions for steps
- Offer alternative primitives for a step (e.g., "validate OR critique")

Workflows should not repeat primitive content—reference the primitive and add workflow-specific context only.

### 2. Workflow Styles

**Declarative (preferred):** Define goal, key results, available primitives, and constraints—execution determines path. The workflow says *what* to achieve, not *how* to achieve it.

**Imperative:** Explicit step-by-step sequence. Use only when order is strictly required (e.g., must validate before implementing).

**Hybrid:** Most workflows combine both—declarative goals with imperative gates (validation checkpoints).

**Prefer declarative.** Declarative workflows:
- Adapt to context (different paths for different situations)
- Are less brittle (don't break when steps need adjustment)
- Enable reasoning (AI chooses approach based on expert reasoning)
- Focus on outcomes (success = key results met, not steps completed)

**Declarative workflow structure:**
```markdown
**Goal:** [What to achieve]
**Key Results:** [How to know it's achieved]
**Available Primitives:** orient, define, design, implement, validate, investigate, brainstorm, critique
**Constraints:** [What must be true, e.g., "validate before implement"]

Execute by: Apply reasoning to select and sequence primitives to achieve goal.
```

### 3. Iteration and Checkpoints

Workflows can iterate. When a validation gate fails:
1. Workflow determines loop-back point
2. Adds new checklist items to return to that state
3. Proceeds forward again

This is managed via the checklist_management protocol. Validation failure doesn't just "go back"—it explicitly updates the task list.

### 4. Uncertainty Resolution

Before creating a workflow, resolve uncertainty about:
- Which primitives are needed
- What order/dependencies exist
- What validation gates are required

See `protocols/manage_complexity_uncertainty_risk.md`.

---

## Required Protocols

Every workflow MUST reference:

1. **tracking** — Maintain progress trace
2. **recovery** — Resume from interruption
3. **checklist_management** — Dynamic task management, iteration handling

---

## Optional Protocols

Reference when applicable:

- **reasoning_patterns** — Pre-action reasoning and post-action verification
- **modularity** — Component boundaries, avoiding unnecessary coupling
- **manage_complexity_uncertainty_risk** — Handling unknowns
- **doc_maintenance** — Updating documentation with changes

---

## Workflow Structure

### Frontmatter

```yaml
---
name: workflow-name
description: When to use this workflow
argument-hint: "[expected input]"
protocols:
  - tracking
  - recovery
  - checklist_management
---
```

### Body

```markdown
# Workflow Name

**Goal:** What this workflow achieves
**Intent:** Why it exists
**Scope:** What it covers

## Workflow Type

**Type:** Deterministic | Adaptive
**Style:** Imperative | Declarative | Hybrid

## Steps

### Step 1: [Name]
**Primitive:** [primitive-name]
**Purpose:** Why this step exists
**Inputs:** What it needs
**Outputs:** What it produces

### Step 2: [Name]
...

## Preconditions

**Must be provided:** [required inputs]
**Self-satisfiable:** [context workflow gathers]

## Postconditions

**Success:** [end state]
**Failure:** [failure conditions]

## Recovery

Step-specific recovery notes.
```

---

## When to Create a Workflow

Create a workflow when:
- Task requires multiple primitives in sequence
- Order/dependencies matter
- Recovery from interruption is valuable
- Task is common enough to codify

Do NOT create a workflow when:
- Single primitive suffices
- Sequence is ad-hoc and won't recur
- Steps have no dependencies

---

## Patterns

**Compose, don't repeat:** Reference primitives and protocols. Add workflow-specific context only.

**Validate after deliverables:** Add validation gates after steps that produce artifacts used by later steps.

**Explicit iteration:** When validation fails, add checklist items—don't silently loop.

---

## Anti-Patterns

**Repeating primitive content:** Workflow duplicates what's in the primitive definition.

**Silent iteration:** Looping back without updating checklist.

**Over-specification:** Dictating primitive actions instead of invoking the primitive.

---

## Key Takeaways

- Workflows are higher-order—they compose primitives
- **Prefer declarative** — define goals and key results, not rigid steps
- **Let reasoning choose the path** — provide primitives and constraints, not prescriptions
- **Validate against outcomes** — success = key results met, not steps completed
- Reference protocols, don't repeat them
- Handle iteration through explicit checklist updates
- Resolve uncertainty before creating the workflow
