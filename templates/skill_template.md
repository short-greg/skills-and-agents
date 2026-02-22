# Skill Templates

Skills come in two types. Use the appropriate template:

## Workflows

**Use for:** Multi-step processes that compose primitives.

**Template:** [workflow_template.md](workflow_template.md)

**Key characteristics:**
- Orchestrates 2+ primitives
- Has Tasks with `(→ KR#)` annotations showing which Key Result each task serves
- Includes Progress Tracking, Recovery, Iteration sections
- References tracking, recovery, checklists protocols

**Examples:** feature_workflow, bugfix_workflow, refactor_workflow

---

## Primitives

**Use for:** Atomic cognitive actions that do one thing well.

**Template:** [primitive_template.md](primitive_template.md)

**Key characteristics:**
- Single cognitive action, not decomposable
- Has Possible Actions (options, not required steps) without KR annotations
- Has Confirm section to verify Key Results at the end
- Answers one question that no other primitive answers

**Examples:** orient, define, design, implement, validate

---

## When to Use Which

| Situation | Use |
|-----------|-----|
| Requires multiple cognitive actions in sequence | Workflow |
| Single cognitive action, context determines specifics | Primitive |
| Needs validation gates between steps | Workflow |
| Can be completed in one "thinking session" | Primitive |
| Composes existing primitives | Workflow |
| Answers a unique question no other primitive answers | Primitive |
