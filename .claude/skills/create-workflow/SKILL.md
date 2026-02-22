---
name: create-workflow
description: Create a new workflow (multi-step process composing primitives) for the skills framework
argument-hint: "[workflow name and purpose]"
---

# Create Workflow

Create a reliable, recoverable workflow that orchestrates primitives effectively.

## When to Use

Use this skill when:
- Task requires multiple primitives in sequence
- Order/dependencies matter
- Recovery from interruption is valuable
- Task is common enough to codify

Do NOT use when:
- Single primitive suffices
- Sequence is ad-hoc and won't recur
- Steps have no dependencies

## Process

Follow the workflow defined in `workflows/create_workflow_workflow.md`.

**Summary:**

1. **Understand existing workflows** — Check workflows/ for overlap
2. **Define the workflow** — Name, goal, intent, scope, type, style
3. **Design the workflow** — Identify primitives, sequence, gates, iteration
4. **Write the workflow** — Create file in workflows/ with required sections
5. **Validate** — Ensure composes primitives, has required protocols, recoverable

## Key Principles

- **Higher-order**: Workflows compose primitives, don't repeat them
- **Prefer declarative**: Define goals and constraints, not rigid steps
- **Required protocols**: Every workflow needs tracking, recovery, checklist_management
- **Validation gates**: After steps that produce artifacts
- **Explicit iteration**: Update checklist on validation failure, don't silently loop

## Workflow Styles

- **Declarative (preferred)**: Goal + key results + available primitives + constraints
- **Imperative**: Step-by-step sequence (only when order is strictly required)
- **Hybrid**: Declarative goals with imperative validation gates

## Existing Workflows

| Workflow | Purpose |
|----------|---------|
| feature_workflow | End-to-end feature development |
| bugfix_workflow | Hypothesis-driven bug fixing |
| refactor_workflow | Safe behavior-preserving refactoring |
| code_review_workflow | Systematic code review |
| test_strategy_workflow | Test design and planning |
| worktree_orchestrate_workflow | Parallel task coordination |
| worktree_task_workflow | Single task in worktree |

## Output

New workflow document in `workflows/[name]_workflow.md`
