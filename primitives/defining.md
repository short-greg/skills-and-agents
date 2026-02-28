---
name: defining
description: >
  Specifying the problem, goal, and constraints of a project or task. Establishes requirements, success criteria, and definition of done.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "define this", "what are the requirements", "scope this out",
  "clarify requirements", "what should we build", "create a PRD".
argument-hint: "[task or feature to define]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Defining

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring task is well-understood before solution is attempted. Bad definitions lead to building the wrong thing.

**Scope:** Establishing what to build and defining done. Answers "what are we building and how will we know it's done?" not "how will we build it?"

---

## Key Results - KR

1. Requirements complete — goal clear, acceptance criteria testable, out-of-scope stated
2. User has confirmed definition is accurate

## Requirements and Constraints - REQ

1. Acceptance criteria must be testable (pass/fail, no interpretation)
2. Interview user to clarify intent if ambiguous
3. Define key terms precisely enough to use as evaluation gates

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **goals_and_objectives.md** - Must use for writing outcome-oriented success criteria
- **instructions.md** - Must use for writing clear instructions for implementers
- **tracking.md** - Must use to ensure completeness
- **reasoning.md** - Use when analyzing requirements for completeness and consistency
- **tracking.md** - Use when tracking requirements captured vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Task or feature to define (can be vague initially)

**Elicit if not provided:**
- User intent (interview to understand what they actually want)
- Existing context (codebase, docs, constraints)

**Optional:** Prior definitions, technical constraints

## Postconditions

**Success:** Requirements documented with testable acceptance criteria, confirmed by user

**Failure:** Requirements could not be determined, unresolvable conflicts

---

## Actions

Select based on context. Each action shows which KR it serves.

### Elicit Requirements (→ KR1)
User intent must be understood. Interview with targeted questions: What problem are you solving? Who is the user? What does success look like? What are the constraints?

### Analyze Existing Context (→ KR1)
Current state must be known. Review existing code, docs, and specs to understand what exists and what constraints apply.

### Write User Stories (→ KR1)
User experience must be captured. Use template: "As a [user] I want to [action], so that [benefit]." Include edge cases.

### Specify Acceptance Criteria (→ KR1)
Done must be testable. For each requirement, write pass/fail criteria. Apply `goals_and_objectives.md` — outcomes not implementation steps.

### Analyze Requirements (→ KR1)
Requirements must be validated before confirmation. Check for:
- **Completeness:** All necessary requirements captured?
- **Consistency:** No conflicts between requirements?
- **Feasibility:** Can be achieved within constraints?
- **Priority:** Most important requirements identified?

### State Out-of-Scope (→ KR1)
Boundaries must be explicit. List what is NOT being addressed to prevent scope creep.

### Confirm with User (→ KR2)
Definition must be agreed. Present draft, verify accuracy, resolve ambiguities before finalizing.

---

## Additional Notes and Terms

**Requirements Engineering phases:** Elicitation → Analysis → Documentation → Validation → Management (IEEE 29148)

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
- [Acceptance Criteria Best Practices](https://www.productplan.com/glossary/acceptance-criteria/)
- [User Stories - Mountain Goat Software](https://www.mountaingoatsoftware.com/articles/advantages-of-user-stories-for-requirements)
- [Anthropic - Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
