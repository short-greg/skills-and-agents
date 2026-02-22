---
name: create-primitive
description: Create a new primitive (atomic cognitive action) for the skills framework
argument-hint: "[primitive name and purpose]"
---

# Create Primitive

Create a well-scoped, composable primitive that does one thing well.

## When to Use

Use this skill when:
- A recurring cognitive action appears across workflows
- The action has clear preconditions/postconditions
- The action is atomic (can't be meaningfully decomposed)
- The action is distinct from existing primitives

Do NOT use when:
- It's a variation of an existing primitive (extend instead)
- It requires multiple cognitive actions (that's a workflow)
- It's tool-specific (belongs in a skill)

## Process

Follow the workflow defined in `workflows/create_primitive_workflow.md`.

**Summary:**

1. **Understand existing primitives** — Check primitives/ for overlap
2. **Define the primitive** — Name, goal, intent, scope, category
3. **Write the primitive** — Create file in primitives/ with required sections
4. **Validate** — Ensure distinct purpose, atomic, self-contained scope

## Key Principles

- **Atomic**: One cognitive action, not decomposable further
- **Composable**: Can be sequenced into workflows
- **Self-contained**: Scope doesn't reference other primitives
- **Goal-oriented**: Defined by outcomes, not steps
- **Declarative**: Define goals and key results, not required steps

## Existing Primitives

Before creating, check for overlap:

| Category | Primitives |
|----------|------------|
| Understanding | orient, define, investigate |
| Planning | plan, design, brainstorm |
| Execution | implement |
| Verification | validate, critique |
| Maintenance | bookkeep, upkeep |

## Output

New primitive document in `primitives/[name].md`
