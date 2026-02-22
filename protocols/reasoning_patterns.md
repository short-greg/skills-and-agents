# Reasoning Patterns Protocol

Structured reasoning patterns for primitives and workflows.

---

## Goal

Ensure quality through deliberate reasoning before action and verification before completion.

---

## Intent

AI tends to rush into solutions without considering approach, and declare completion without verification. These patterns prevent poor approaches and premature completion by requiring explicit reasoning at key points.

---

## Scope

This protocol addresses reasoning patterns for primitives and workflows: pre-action reasoning (Expert Reasoning and alternatives), post-action verification (Confirm), and guidance on when to use each.

---

## Key Results

- Approach is considered before execution begins
- Completion is verified against success criteria
- Pattern choice matches task complexity

---

## Pre-Action Reasoning

Before starting work, reason about approach. Choose based on task complexity:

### Expert Reasoning (Full)

**When to use:** Complex or unfamiliar tasks, high-stakes decisions, tasks with multiple valid approaches.

**Pattern:**
```
Before doing anything else, describe how an expert in [domain] would approach [task].
Cover:
- Strategy and first steps
- Alternatives to consider
- Fallbacks if primary approach fails
- Common mistakes to avoid
- What high-quality output looks like

This is meta-reasoning about approach, not doing the task.
```

### Quick Assessment

**When to use:** Moderate complexity, familiar domain, time-sensitive tasks.

**Pattern:**
```
Before starting, briefly identify:
- Key considerations for this task
- Primary approach and one alternative
- Main risk to watch for
```

### Skip

**When to use:** Simple/routine tasks, well-defined procedures, approach is obvious.

**Guidance:** Only skip pre-action reasoning when the task is truly straightforward. When in doubt, use Quick Assessment.

---

## Post-Action Verification

Before declaring completion, verify against success criteria.

### Confirm

**When to use:** Always required for primitives and workflows.

**Pattern:**
```
Before declaring done, verify against each key result:
- [Check for key result 1]
- [Check for key result 2]
- ...

Report outcome explicitly: state whether succeeded or failed, and why.
```

**Why always required:** Unlike pre-action reasoning (which can be scaled), verification is binary—either you checked or you didn't. Skipping verification leads to incomplete work being marked complete.

---

## Choosing Patterns

| Task Type | Pre-Action | Post-Action |
|-----------|------------|-------------|
| Complex/unfamiliar | Expert Reasoning | Confirm |
| Moderate complexity | Quick Assessment | Confirm |
| Simple/routine | Skip | Confirm |

**Default:** Quick Assessment + Confirm. Upgrade to Expert Reasoning when uncertain.

---

## Patterns

**Pattern: Scale reasoning to complexity**
- Match pre-action reasoning depth to task difficulty
- Don't over-engineer simple tasks
- Don't under-think complex ones

**Pattern: Always verify**
- Confirm is not optional
- Verification catches issues before they propagate

---

## Anti-Patterns

**Anti-Pattern: Skipping reasoning on complex tasks**
- Leads to poor approaches, wasted effort, rework
- When in doubt, use Quick Assessment at minimum

**Anti-Pattern: Skipping verification**
- Leads to incomplete work marked as complete
- Confirm is required, not optional

**Anti-Pattern: Over-reasoning simple tasks**
- Expert Reasoning on trivial tasks wastes time
- Match pattern to complexity

---

## Key Takeaways

- Pre-action reasoning prevents poor approaches (scale to complexity)
- Post-action verification prevents premature completion (always required)
- Default to Quick Assessment + Confirm
- Upgrade to Expert Reasoning when task is complex or unfamiliar
