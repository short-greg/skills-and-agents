---
name: create-primitive
description: >
  Use when creating a new primitive (atomic cognitive action).
  Triggers on: "create a primitive", "define a new primitive", "add a primitive".
argument-hint: "[primitive name and purpose]"
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

# Create Primitive Workflow

**Goal:** Create a well-scoped, composable primitive that does one thing well.

**Intent:** Primitives are building blocks of workflows. Poorly defined primitives cause confusion, scope overlap, and brittle compositions. Clear primitives with distinct purposes make workflow creation straightforward.

**Scope:** Primitive creation: determining if a primitive is needed, defining scope, writing the primitive document, and validating against existing primitives.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (declarative goals with validation gates)

---

## Key Results

1. **Primitive is distinct** — answers a question no existing primitive answers
2. **Primitive is atomic** — single cognitive action, not decomposable
3. **Primitive is self-contained** — scope doesn't reference other primitives
4. **Primitive is composable** — can be sequenced into workflows
5. **Primitive Key Results are outcome-oriented** — measure what's produced, not steps taken

---

## Available Primitives

`orient`, `define`, `validate`, `critique`, `brainstorm`

---

## Constraints

- Check existing primitives before creating
- Scope must be positive (what IS covered) not negative
- Must be atomic (single cognitive action)
- If >50% overlap with existing primitive, extend that one instead

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Is this primitive needed?** — Could an existing primitive handle this?
2. **Is it atomic?** — Can it be decomposed? If yes, it's a workflow.
3. **What question does it answer?** — One sentence that no other primitive answers.
4. **Where does it fit?** — Understanding, Planning, Execution, Verification, Maintenance?

Output reasoning before proceeding.

---

## Execution

### Phase 1: Understand Existing Primitives

**Primitive:** `orient`

Read existing primitives to understand the landscape:

```
primitives/
├── orient.md        # Understanding context and constraints
├── define.md        # Establishing requirements and scope
├── design.md        # Planning technical approach
├── implement.md     # Executing plans into artifacts
├── validate.md      # Verifying against criteria
├── investigate.md   # Diagnosing root causes
├── brainstorm.md    # Generating options
├── critique.md      # Evaluating quality
├── plan.md          # Creating action plans
├── bookkeep.md      # Recording state changes
└── upkeep.md        # Maintaining existing systems
```

**Key question:** What question does the proposed primitive answer that none of these answer?

### Phase 2: Define the Primitive

**Primitive:** `define`

Establish the primitive's identity:

1. **Name** — Action verb (e.g., orient, define, validate)
2. **Goal** — One sentence outcome
3. **Intent** — Why it exists, what problem it prevents
4. **Scope** — What's covered (positive, self-contained, precise)
5. **Category** — Understanding | Planning | Execution | Verification | Maintenance

**Scope Pattern:**
```markdown
**Scope:** [Core activity]. Includes: [activity 1], [activity 2], [activity 3].
[Primitive name] answers "[question it answers]" not "[different question]".
```

**Gate:** User confirms the scope is distinct from existing primitives.

### Phase 3: Write the Primitive

Create the primitive document in `primitives/`:

**Required sections:**

```markdown
# [Primitive Name]

[One sentence description]

---

## Goal

[Outcome this primitive achieves]

---

## Intent

[Why it exists — what problem it prevents]

---

## Scope

[Positive, self-contained description of what's covered]

---

## Key Results

[Binary, verifiable outcomes]

---

## Preconditions

**Must be provided:** [Required inputs]
**Self-satisfiable:** [Context primitive can gather]
**Non-essential:** [Helpful but not required]

---

## Postconditions

**Success:** [State when primitive succeeds]
**Failure:** [Conditions that cause failure]

---

## Possible Actions

[Actions the primitive might take — not required steps]

---

## Patterns

[Recommended approaches]

---

## Anti-Patterns

[What to avoid]
```

### Phase 4: Validate

**Primitive:** `validate`

Verify the primitive:

1. **Distinct purpose** — Different question than existing primitives?
2. **Atomic** — Single cognitive action, not decomposable?
3. **Self-contained scope** — No references to other primitives?
4. **Follows template** — All required sections present?
5. **Declarative** — Defines goals, not steps?

**On failure:** Revise and re-validate.

---

## Preconditions

**Must be provided:** Primitive name and purpose (ask if unclear)

**Self-satisfiable:** Existing primitives (read from `primitives/`)

---

## Postconditions

**Success:** New primitive document created in `primitives/`, validated against existing primitives.

**Failure:** Primitive duplicates existing one, is not atomic, or user aborts.

---

## Recovery

Per `recovery` protocol — check for partial primitive document, resume from last completed section.

---

## Anti-Patterns

**Scope references other primitives:** "Unlike validate..." — describe what this primitive does, not what others do.

**Not atomic:** If it requires multiple cognitive actions, create a workflow instead.

**Overlapping:** If >50% overlap, extend the existing primitive.

**Step-oriented:** Primitives define goals and possible actions, not required steps.
