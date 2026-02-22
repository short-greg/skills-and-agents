---
name: create-primitive
description: >
  Use when creating a new primitive (atomic cognitive action).
  Triggers on: "create a primitive", "define a new primitive", "add a primitive".
argument-hint: "[primitive name and purpose]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, TodoWrite
---

# Create Primitive Workflow

**Goal:** Create a well-scoped, composable primitive that does one thing well.

**Intent:** Primitives are building blocks of workflows. Poorly defined primitives cause confusion, scope overlap, and brittle compositions. Clear primitives with distinct purposes make workflow creation straightforward.

**Scope:** Primitive creation: determining if a primitive is needed, defining scope, writing the primitive document, and validating against existing primitives.

---

## Key Results

1. Need for primitive is reasoned about before creating
2. Primitive is distinct — answers a question no existing primitive answers
3. Primitive is atomic — single cognitive action, not decomposable
4. Primitive is self-contained — scope doesn't reference other primitives
5. Primitive is composable — can be sequenced into workflows
6. Primitive Key Results are outcome-oriented — measure what's produced, not steps taken

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which sections of the primitive are complete.
- `recovery.md` — On startup, check for partial primitive document. Resume from last completed section.
- `checklists.md` — Create a checklist based on required sections. Update dynamically.
- `reasoning.md` — Reason about whether primitive is needed before creating.
- `goals_and_objectives.md` — Ensure primitive key results are outcome-oriented.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to create the new primitive. If you do not understand a primitive, read it before using it.

- `orient` — Understand existing primitives and the landscape.
- `define` — Establish the new primitive's identity and scope.
- `validate` — Verify the primitive meets all criteria.
- `critique` — Review the primitive for issues.
- `brainstorm` — Generate options for scope and naming.

---

## Constraints

- Check existing primitives before creating
- Scope must be positive (what IS covered) not negative
- Must be atomic (single cognitive action)
- If >50% overlap with existing primitive, extend that one instead

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Need (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Is this primitive needed?** — Could an existing primitive handle this?
2. **Is it atomic?** — Can it be decomposed? If yes, it's a workflow.
3. **What question does it answer?** — One sentence that no other primitive answers.
4. **Where does it fit?** — Understanding, Planning, Execution, Verification, Maintenance?

Output your reasoning.

### Understand Existing Primitives (→ KR2)

Use `orient` primitive. Read existing primitives to understand the landscape:

- orient.md, define.md, design.md, implement.md, validate.md
- investigate.md, brainstorm.md, critique.md
- plan.md, bookkeep.md, upkeep.md

**Key question:** What question does the proposed primitive answer that none of these answer?

### Define the Primitive (→ KR2, KR3, KR4)

Use `define` primitive. Establish the primitive's identity:

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

**Gate:** Ask user to confirm the scope is distinct from existing primitives.

### Write the Primitive (→ KR5, KR6)

Create the primitive document in `primitives/` following the template in `templates/primitive_template.md`.

**Required sections:**
- Goal, Intent, Scope
- Key Results
- Protocols
- Preconditions, Postconditions
- Possible Actions
- Confirm
- Use Cases
- Tools, Hooks

### Validate Primitive (→ KR2, KR3, KR4, KR5)

Use `validate` primitive. Verify the primitive:

1. **Distinct purpose** — Different question than existing primitives?
2. **Atomic** — Single cognitive action, not decomposable?
3. **Self-contained scope** — No references to other primitives?
4. **Follows template** — All required sections present?
5. **Declarative** — Defines goals, not steps?

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

**Must be provided:** Primitive name and purpose (ask if unclear)

**Self-satisfiable:** Existing primitives (read from `primitives/`)

---

## Postconditions

**Success:** New primitive document created in `primitives/`, validated against existing primitives.

**Failure:** Primitive duplicates existing one, is not atomic, or user aborts.

---

## Recovery

Per `recovery.md` — check for partial primitive document, resume from last completed section.

---

## Anti-Patterns

**Scope references other primitives:** "Unlike validate..." — describe what this primitive does, not what others do.

**Not atomic:** If it requires multiple cognitive actions, create a workflow instead.

**Overlapping:** If >50% overlap, extend the existing primitive.

**Step-oriented:** Primitives define goals and possible actions, not required steps.
