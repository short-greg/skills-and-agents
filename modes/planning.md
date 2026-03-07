---
name: planning
description: >
  Sequencing actions to achieve a goal. Produces a sequence of actions to accomplish a goal before starting work.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
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

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** Goal or task — what needs to be accomplished

**Optional:**
- Existing definition or requirements (read if exists)
- Codebase context (understand current state and constraints)
- Prior plan, design decisions

## Postconditions

**Success:** Sequence of actions established, dependencies identified, right level of detail, user confirmed

**Failure:** Goal too ambiguous, conflicting constraints. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Elicit Context (→ KR1)

**Goal:** Gather information needed for accurate planning

**When:** Definition, requirements, or codebase context not provided

**Protocols:** `protocols/elicitation.md`, `protocols/pragmatics.md`

**Instructions:** Use interviewing techniques to discover constraints and context. Apply pragmatic communication patterns to gather what's needed to plan accurately before proceeding.

**Inputs:**
- Goal or task (required)
- Known context (optional)

**Default Output:** Context summary with constraints and requirements

---

### Identify Actions (→ KR1)

**Goal:** Enumerate all required work

**When:** Enumerating required work

**Protocols:** `protocols/goal_setting.md`, `protocols/discipline.md`, `protocols/instruction_giving.md`

**Instructions:** Break goal into independent sub-goals, enumerate ALL actions systematically, write each as clear instruction. Apply 100% rule — no gaps. Use goal decomposition, MECE enumeration, and action verbs with explicit language.

**Inputs:**
- Goal or task (required)
- Requirements (optional)

**Default Output:** Complete list of actions

---

### Identify Dependencies (→ KR1)

**Goal:** Map what must be done before what

**When:** Determining prerequisites

**Protocol:** `protocols/discipline.md`

**Instructions:** Check ALL action pairs systematically, map what must be done before what. Use coverage tracking to ensure all dependencies are captured.

**Inputs:**
- Action list (required)

**Default Output:** Dependency map

---

### Sequence Actions (→ KR1)

**Goal:** Determine optimal order of actions

**When:** Ordering work

**Protocols:** `protocols/thinking.md`, `protocols/instruction_giving.md`

**Instructions:** Determine optimal order, group into 5-9 step chunks, identify parallelizable work, plan milestones. Use strategic thinking, cognitive chunking, and step sequencing.

**Inputs:**
- Action list (required)
- Dependencies (required)

**Default Output:** Sequenced plan with milestones

---

### Surface Unknowns (→ KR1)

**Goal:** Identify risks and uncertain steps

**When:** Identifying risks

**Protocols:** `protocols/risk_management.md`, `protocols/discipline.md`

**Instructions:** Check ALL actions for vague requirements, untested approaches, missing criteria. Order uncertain steps early. Use uncertainty indicators, fail-fast ordering, and coverage tracking.

**Inputs:**
- Action list (required)
- Known risks (optional)

**Default Output:** Risk list with uncertain steps flagged

---

### Confirm with User (→ KR2)

**Goal:** Verify plan acceptability before execution

**When:** Presenting plan for review

**Protocol:** `protocols/pragmatics.md`

**Instructions:** Frame as recommended approach with reasoning, verify completeness and ordering before proceeding. Use option presentation techniques.

**Inputs:**
- Complete plan (required)

**Default Output:** Confirmation request with plan summary

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure Principles](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [PMC - Hierarchical Planning](https://pmc.ncbi.nlm.nih.gov/articles/PMC7162548/)
- [Cognitive Science - Resource-rational Task Decomposition](https://cognitivesciencesociety.org/cogsci20/papers/0747/0747.pdf)
