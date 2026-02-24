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

**Addresses:** Reasoning patterns for primitives and workflows (pre-action reasoning, post-action verification, pattern selection based on complexity)

**Does not address:** Domain-specific reasoning techniques, debugging strategies, code review patterns

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

## Examples

### Example 1: Complex Task - Designing Authentication System

**Context:** User asks to add authentication to an existing web application.

**Application:** Use Expert Reasoning (complex, multiple valid approaches, high stakes)

**Reasoning:**
```
Before implementing authentication, an expert would approach this by:

Strategy:
- Assess existing architecture (sessions? stateless?)
- Choose auth method (JWT, OAuth, session-based)
- Plan integration points (middleware, protected routes)
- Consider security (password hashing, token storage, HTTPS)

Alternatives:
- Session-based (simpler, requires server state)
- JWT tokens (stateless, more scalable)
- OAuth delegation (external provider)

Fallbacks:
- If JWT proves complex, fall back to session-based
- If OAuth integration fails, implement basic JWT

Common mistakes:
- Storing passwords in plaintext (use bcrypt)
- Weak JWT secrets (use cryptographically secure random)
- No HTTPS enforcement (sensitive data exposure)

High-quality output:
- Secure password hashing
- Proper token expiration
- Protected routes with middleware
- Clear error messages for auth failures
```

**Result:** Expert reasoning revealed multiple security considerations that might have been missed with immediate implementation. Approach selected before writing code.

### Example 2: Moderate Task - Refactoring Function

**Context:** User asks to refactor a 50-line function to improve readability.

**Application:** Use Quick Assessment (moderate complexity, familiar domain)

**Reasoning:**
```
Key considerations:
- Identify separate concerns within function
- Extract helper functions for each concern
- Preserve existing behavior (tests must pass)

Primary approach:
- Extract logging logic to separate function
- Extract validation logic to separate function
- Main function orchestrates extracted functions

Alternative:
- Split into multiple smaller functions (might be overkill for 50 lines)

Main risk:
- Breaking existing behavior during refactoring (run tests frequently)
```

**Result:** Quick assessment provided clear direction without over-analyzing. Refactoring proceeded smoothly.

### Example 3: Simple Task - Adding Console Log

**Context:** User asks to add debug logging to a specific function.

**Application:** Skip pre-action reasoning (simple, obvious approach)

**Implementation:** Directly add `console.log(...)` at relevant point.

**Verification:** Use Confirm pattern to verify
```
Before declaring done, verify:
- [✓] Log statement added at correct location
- [✓] Log includes relevant context (function name, key variables)
- [✓] Log doesn't expose sensitive data
```

**Result:** Task completed efficiently without unnecessary reasoning overhead.

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

## When to Apply

Use this protocol when:

- **Starting a primitive or workflow** - Apply pre-action reasoning appropriate to task complexity
- **Before declaring completion** - Always use Confirm pattern to verify key results
- **Task approach is unclear** - Upgrade from Quick Assessment to Expert Reasoning
- **Previous approach failed** - Use Expert Reasoning to consider alternatives
- **Reviewing workflow design** - Ensure reasoning patterns are specified for each step
- **Teaching or documenting skills** - Include reasoning patterns in skill instructions

---

## Key Takeaways

- Pre-action reasoning prevents poor approaches (scale to complexity)
- Post-action verification prevents premature completion (always required)
- Default to Quick Assessment + Confirm
- Upgrade to Expert Reasoning when task is complex or unfamiliar
