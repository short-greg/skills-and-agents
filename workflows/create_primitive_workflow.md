**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions.**

---
name: create-primitive
description: >
  Use when creating a new primitive (atomic cognitive action).
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Primitive need justified and distinct — reasoned about necessity, answers unique question no existing primitive answers, atomic (single cognitive action)
2. Primitive document created — in primitives/, follows template, validated against criteria
3. Primitive is well-scoped — self-contained (no references to other primitives), composable, outcome-oriented

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for partial primitive document, resume from last section
3. Check existing primitives before creating — if >50% overlap, extend that one instead
4. When defining scope, use positive framing (state what IS covered) not negative framing (what ISN'T covered)
5. Iterate up to 3 times if validation fails, revise primitive, re-validate

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Primitive name and purpose

**Elicit if not provided:**
- Existing primitives (read from primitives/ to check for duplicates)
- Primitive category (Understanding | Planning | Execution | Verification | Maintenance)

**Optional:** Specific use cases, example workflows that would use this primitive

## Postconditions

The resulting state after the skill is finished.

**Success:** New primitive document created in primitives/, validated against all criteria

**Failure:** Primitive duplicates existing one, is not atomic, or user aborts

## Steps

Complete the Tasks in this order.

1. Reason About Need
2. Understand Existing Primitives
3. Define the Primitive
4. Write the Primitive
5. Validate Primitive

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

### Understand Existing Primitives (→ KR1)

Use `orient` primitive. Read existing primitives to understand the landscape and verify uniqueness.

### Define the Primitive (→ KR1, KR3)

Use `define` primitive. Establish the primitive's identity:
1. Name (action verb: orient, define, validate)
2. Goal (one sentence outcome)
3. Intent (why it exists, what problem it prevents)
4. Scope (what's covered — positive, self-contained, precise)
5. Category (Understanding | Planning | Execution | Verification | Maintenance)

Ask user to confirm the scope is distinct from existing primitives.

### Write the Primitive (→ KR2)

Create the primitive document in primitives/ following templates/primitive_template.md.

Required sections per template:
- Frontmatter with MUST satisfy instruction
- Goal, Intent, Scope
- Key Results - KR (2, with imperative)
- Requirements and Constraints - REQ (with imperative)
- Preconditions (Required, Elicit, Optional)
- Postconditions (Success, Failure)
- Actions (with → KR# mapping)
- Validation Criteria
- Additional Notes and Terms

### Validate Primitive (→ KR1, KR2, KR3)

Use `validate` primitive. Verify the created primitive against all Validation Criteria from templates/primitive_template.md:

1. **Structure:** All sections present with one-line imperatives
2. **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW)
3. **Traceability:** Actions show (→ KR#), all KRs served by at least one action
4. **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
5. **Coherent:** Actions flow logically, no contradictions
6. **Concise:** As few words as possible, no duplication
7. **Complete:** All necessary information provided, all KRs achievable from Actions
8. **Precise:** Specific, unambiguous language

Additionally verify primitive-specific criteria:
9. **Distinct purpose:** Answers a question no existing primitive answers
10. **Atomic:** Single cognitive action, not decomposable into multiple actions
11. **Self-contained scope:** No references to other primitives in scope definition

Check each criterion. Report which pass and which fail.

On failure: Revise the primitive to address failures, then re-validate (up to 3 iterations per REQ5).

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand existing primitives and the landscape
- `define` — Establish the new primitive's identity and scope
- `validate` — Verify the primitive meets all criteria
- `critique` — Review the primitive for issues
- `brainstorm` — Generate options for scope and naming

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

**Scope Pattern:**
```markdown
**Scope:** [Core activity]. Includes: [activity 1], [activity 2], [activity 3].
[Primitive name] answers "[question it answers]" not "[different question]".
```

**Anti-Patterns to Avoid:**
- Scope references other primitives ("Unlike validate..." — describe what this primitive does, not what others do)
- Not atomic (if it requires multiple cognitive actions, create a workflow instead)
- Overlapping (if >50% overlap, extend the existing primitive)
- Step-oriented (primitives define goals and possible actions, not required steps)
