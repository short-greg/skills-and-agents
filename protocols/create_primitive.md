# Create Primitive Protocol

Define atomic cognitive actions that can be composed into workflows.

---

## Goal

Enable creation of well-scoped, composable primitives that do one thing well.

---

## Intent

Primitives are the building blocks of workflows. Poorly defined primitives lead to confusion about which to use, scope overlap, and brittle compositions. Clear primitives with distinct purposes make workflow creation straightforward.

---

## Scope

This protocol addresses primitive creation: when to create a primitive, how to define scope, required structure, and avoiding overlap with existing primitives.

---

## Key Results

- Primitive has a clear, distinct purpose
- Scope is self-contained (no references to other primitives)
- Preconditions and postconditions are defined
- Primitive can be composed into workflows

---

## Essential Concepts

### 1. What is a Primitive

A primitive is an atomic cognitive action—a single unit of reasoning that can be composed into workflows. Primitives are:

- **Atomic**: One cognitive action, not decomposable further
- **Composable**: Can be sequenced into workflows
- **Self-contained**: Scope doesn't reference other primitives
- **Goal-oriented**: Defined by outcomes, not steps

### 2. Declarative Over Imperative

Primitives should be **goal-oriented (declarative)** rather than step-oriented (imperative):

- **Define the goal and key results** — what success looks like
- **Provide possible actions** — not required steps
- **Let reasoning determine the path** — expert reasoning chooses approach
- **Validate against outcomes** — did we achieve the goal, not did we follow steps

**Why declarative?** Different contexts require different approaches. A primitive that prescribes exact steps becomes brittle. A primitive that defines goals and provides action options adapts to context.

**Example:**
```markdown
# Imperative (brittle)
1. Read all files in directory
2. Search for pattern X
3. List matches

# Declarative (flexible)
**Goal:** Find relevant code
**Key Results:** Locations identified, context understood
**Possible Actions:** Grep, Glob, Read, Ask user
```

### 3. When to Create a Primitive

Create when:
- A recurring cognitive action appears across workflows
- The action has clear preconditions/postconditions
- The action is atomic (can't be meaningfully decomposed)
- The action is distinct from existing primitives

Do NOT create when:
- It's a variation of an existing primitive (extend instead)
- It requires multiple cognitive actions (that's a workflow)
- It's tool-specific (belongs in a skill)

### 4. Existing Primitives

Before creating, check for overlap:

| Category | Primitives |
|----------|------------|
| Understanding | orient, define, investigate |
| Planning | plan, design, brainstorm |
| Execution | implement |
| Verification | validate, critique |
| Maintenance | bookkeep, upkeep |

**Test:** Can you explain in one sentence what question your primitive answers that no existing primitive answers?

### 5. Scope Definition

Scope is the most critical section. Requirements:

- **Positive**: State what IS covered, not what is NOT
- **Self-contained**: No references to other primitives by name
- **Precise**: Two people would agree on whether an action falls within scope
- **Concise**: One paragraph

**Pattern:**
```
**Scope:** [Core activity]. Includes: [activity 1], [activity 2], [activity 3].
[Primitive name] answers "[question it answers]" not "[different question]".
```

---

## Required Structure

See `protocols/skill_template.md` for the full template. Key sections:

### Frontmatter
```yaml
---
name: primitive-name
description: >
  Use when [situation]. Triggers on: "[phrase 1]", "[phrase 2]".
argument-hint: "[expected input]"
---
```

### Goal, Intent, Scope
- **Goal**: One sentence—the outcome
- **Intent**: Why it exists—what problem it prevents
- **Scope**: What's covered (positive, self-contained)

### Preconditions
- **Must be provided**: Required inputs (ask if missing)
- **Self-satisfiable**: Context primitive can gather itself
- **Non-essential**: Helpful but not required

### Postconditions
- **Success**: State when primitive succeeds
- **Failure**: Conditions that cause failure

### Key Results
Binary, verifiable outcomes used by Confirm.

### Reasoning Patterns
Apply patterns from `protocols/reasoning_patterns.md`:
- Pre-action: Expert Reasoning or Quick Assessment (based on complexity)
- Post-action: Confirm (always required)

---

## Patterns

**Positive scope**: State what IS covered, not what is NOT.

**Distinct purpose**: Each primitive answers a different question.

**Minimal tools**: Only include tools the primitive genuinely needs.

---

## Anti-Patterns

**Scope references other primitives**: "Unlike validate, this primitive..." — describe what this primitive does, not what others do.

**Overlapping scope**: If >50% overlap with existing primitive, extend that one instead.

**Not atomic**: If it requires multiple cognitive actions, it's a workflow.

---

## Key Takeaways

- Primitives are atomic cognitive actions
- **Define goals and key results, not steps** — declarative over imperative
- **Provide possible actions, not required sequences** — let reasoning choose the path
- Scope must be positive and self-contained
- Check for overlap with existing primitives before creating
- Apply reasoning patterns (Expert Reasoning/Quick Assessment + Confirm)
