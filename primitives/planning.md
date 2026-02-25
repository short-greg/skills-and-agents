---
name: planning
description: >
  Produces a sequence of actions to accomplish a goal before starting work.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "plan this", "how should we approach this", "break this down",
  "what should we do first", "create a plan", "what are the steps".
argument-hint: "[task or goal to plan]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Planning

**Goal:** Produce a clear sequence of actions that will accomplish the goal, in the right order, with dependencies understood.

**Intent:** Prevent wasted effort from doing things in the wrong order or missing dependencies. A good plan makes execution straightforward.

**Scope:** Deciding what to do and in what order. Answers "what steps and in what order?" not "how do we implement each step?"

---

## Key Results - KR

1. Plan is complete and correctly ordered — no missing steps, no action requires something not yet done, dependencies explicit
2. User has confirmed plan is sound

## Requirements and Constraints - REQ

1. Apply 100% rule — plan must capture all work needed, no gaps
2. Focus on deliverables — what needs to be done, not implementation details
3. Right level of abstraction — actionable but not over-specified

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to reason about sequencing and verify completeness
- **goals_and_objectives.md** - Must use to ensure plan achieves success criteria
- **checklists.md** - Must use to ensure all aspects covered
- **risk_management.md** - Use when identifying unknowns that affect sequencing
- **tracking.md** - Use when tracking planning progress
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Goal or task — what needs to be accomplished

**Elicit if not provided:**
- Existing definition or requirements (read if exists)
- Codebase context (understand current state and constraints)

**Optional:** Prior plan, design decisions

## Postconditions

**Success:** Sequence of actions established, dependencies identified, right level of detail, user confirmed

**Failure:** Goal too ambiguous, conflicting constraints

---

## Actions

Select based on context. Each action shows which KR it serves.

### Identify Actions (→ KR1)
All required work must be enumerated. List high-level actions needed to accomplish goal. Apply 100% rule — no gaps.

### Identify Dependencies (→ KR1)
Prerequisites must be explicit. Determine what must be done before what.

### Sequence Actions (→ KR1)
Order must be correct. Sequence based on dependencies and logical flow. Identify parallelizable work.

### Surface Unknowns (→ KR1)
Risks must be visible. Identify actions where approach is unclear and investigation may be needed first.

### Confirm with User (→ KR2)
Plan must be agreed. Present plan, verify completeness and ordering before proceeding.

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure Principles](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [PMC - Hierarchical Planning](https://pmc.ncbi.nlm.nih.gov/articles/PMC7162548/)
- [Cognitive Science - Resource-rational Task Decomposition](https://cognitivesciencesociety.org/cogsci20/papers/0747/0747.pdf)
