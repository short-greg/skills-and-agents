---
name: planning
description: >
  Sequencing actions to achieve a goal. Produces a sequence of actions to accomplish a goal before starting work.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "plan this", "how should we approach this", "break this down",
  "what should we do first", "create a plan", "what are the steps".
  keywords: sequencing, ordering, decomposing, scheduling, prioritizing
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

- **goal_setting.md** — Must use for Identify Actions
- **discipline.md** — Must use for Identify Actions, Identify Dependencies, Surface Unknowns
- **instruction_giving.md** — Must use for Identify Actions, Sequence Actions
- **thinking.md** — Must use for Sequence Actions
- **risk_management.md** — Must use for Surface Unknowns
- **interviewing.md** — Use for Elicit Context (when info incomplete)
- **pragmatics.md** — Use for Elicit Context, Confirm with User
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

Inherits from `base.md` — output lightweight checklist, resolve preconditions, plan actions, execute, report result.

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

### Elicit Context (→ KR1)

Execute context elicitation using `interviewing.md` (Open Questions, Constraint Discovery) and `pragmatics.md` (Directness Calibration) when definition, requirements, or codebase context not provided. Gather what's needed to plan accurately before proceeding.

### Identify Actions (→ KR1)

Execute action identification using `goal_setting.md` (Goal Decomposition), `discipline.md` (MECE Enumeration), and `instruction_giving.md` (Action Verbs, Explicit Language) when enumerating required work. Break goal into independent sub-goals, enumerate ALL actions systematically, write each as clear instruction. Apply 100% rule — no gaps.

### Identify Dependencies (→ KR1)

Execute dependency identification using `discipline.md` (Coverage Tracking) when determining prerequisites. Check ALL action pairs systematically, map what must be done before what.

### Sequence Actions (→ KR1)

Execute action sequencing using `thinking.md` (Strategic) and `instruction_giving.md` (Cognitive Chunking, Step Sequencing) when ordering work. Determine optimal order, group into 5-9 step chunks, identify parallelizable work, plan milestones.

### Surface Unknowns (→ KR1)

Execute unknown surfacing using `risk_management.md` (Uncertainty Indicators, Fail-Fast Ordering) and `discipline.md` (Coverage Tracking) when identifying risks. Check ALL actions for vague requirements, untested approaches, missing criteria. Order uncertain steps early.

### Confirm with User (→ KR2)

Execute plan confirmation using `pragmatics.md` (Recommended Option, Option Presentation) when presenting plan for review. Frame as recommended approach with reasoning, verify completeness and ordering before proceeding.

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure Principles](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [PMC - Hierarchical Planning](https://pmc.ncbi.nlm.nih.gov/articles/PMC7162548/)
- [Cognitive Science - Resource-rational Task Decomposition](https://cognitivesciencesociety.org/cogsci20/papers/0747/0747.pdf)
