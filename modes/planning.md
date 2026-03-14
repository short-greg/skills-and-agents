---
name: planning
description: >
  Sequencing actions to achieve a goal. Produces a sequence of actions to accomplish a goal before starting work.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "plan this", "how should we approach this", "break this down",
  "what should we do first", "create a plan", "what are the steps".
  keywords: sequencing, ordering, decomposing, scheduling, prioritizing
---

# Planning

**Goal:** Produce a clear sequence of actions that will accomplish the goal, in the right order, with dependencies understood.

**Intent:** Prevent wasted effort from doing things in the wrong order or missing dependencies. A good plan makes execution straightforward.

**Scope:** Deciding what to do and in what order. Answers "what steps and in what order?" not "how do we implement each step?" (implementing) or "where are we now?" (orienting).

---

## Key Results - KR

1. Plan is complete and correctly ordered — no missing steps, dependencies explicit, 100% of work captured
2. User has confirmed plan is sound

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

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Decompose Work | KR1 | To break goal into deliverables and milestones | Use when goal is clear and work needs to be enumerated | Read and apply appropriate techniques from `protocols/goal_setting.md` and `protocols/discipline.md` to break goal into sub-goals, enumerate ALL actions (100% rule), and set key milestones/checkpoints | In: Goal or task (required), Requirements (optional) | Out: Action list with milestones |
| Perform Risk Analysis | KR1 | To evaluate scope, complexity, risks, and uncertainty | Use when work decomposed and risks not yet assessed | Read and apply appropriate techniques from `protocols/risk_management.md` and `protocols/thinking.md` to assess scope, estimate complexity, identify risks and uncertainties, and plan contingencies for high-risk items | In: Action list (required), Known constraints (optional) | Out: Risk assessment with contingencies |
| Map Dependencies | KR1 | To identify what must be done before what | Use when action list exists and ordering needed | Read and apply appropriate techniques from `protocols/discipline.md` to check ALL action pairs and map prerequisites systematically | In: Action list (required) | Out: Dependency map |
| Sequence and Prioritize | KR1 | To determine optimal order and priorities | Use when dependencies mapped and plan needs ordering | Read and apply appropriate techniques from `protocols/thinking.md` and `protocols/instruction_giving.md` to determine order, identify critical path, prioritize by impact, and apply fail-fast ordering for uncertain items | In: Action list (required), Dependencies (required), Risk assessment (optional) | Out: Sequenced prioritized plan |
| Allocate Resources | KR1 | To assign work to appropriate execution methods | Use when plan sequenced and parallelization opportunities exist | Read and apply appropriate techniques from `protocols/thinking.md` to identify parallel vs sequential work, assign to sub-agents or worktrees where appropriate | In: Sequenced plan (required), Available resources (optional) | Out: Resource allocation with parallel/sequential assignments |
| Confirm Plan | KR2 | To verify plan acceptability before execution | Use when plan complete and ready for review | Read and apply appropriate techniques from `protocols/pragmatics.md` to present plan with rationale, verify completeness, and get user buy-in | In: Complete plan (required) | Out: Confirmation request with plan summary |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure Principles](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [PMC - Hierarchical Planning](https://pmc.ncbi.nlm.nih.gov/articles/PMC7162548/)
- [Cognitive Science - Resource-rational Task Decomposition](https://cognitivesciencesociety.org/cogsci20/papers/0747/0747.pdf)
