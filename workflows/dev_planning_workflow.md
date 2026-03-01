---
name: dev-planning
description: >
  Create a validated development plan before implementation begins.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "plan this project", "create a dev plan", "what's the plan", "how should we approach this",
  "break down this project", "plan before we start".
  keywords: planning, sequencing, scoping, decomposing
argument-hint: "[project or feature to plan]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, WebSearch, WebFetch, Task, TodoWrite
---

# Dev Planning Workflow

**Goal:** Produce a validated development plan that enables confident implementation.

**Intent:** Prevent wasted implementation effort by ensuring context is understood, requirements are clear, and tasks are properly sequenced before coding begins.

**Scope:** From initial project/feature idea to approved plan. Covers understanding context, defining requirements, identifying risks, sequencing tasks, and getting user confirmation. Does NOT include implementation.

---

## Key Results - KR

1. Context understood — project state, constraints, conventions mapped
2. Requirements defined — outcomes clear, acceptance criteria established
3. Plan created — tasks sequenced, dependencies identified, risks surfaced
4. User confirmed — plan approved before implementation begins

## Requirements and Constraints - REQ

1. Progress tracked per `tracking_and_recovery.md` — file-based progress tracking
2. Recoverable from interruption — check for existing progress on startup
3. Validate context before defining requirements
4. Validate requirements before planning tasks
5. Surface risks early — unknowns identified before task sequencing
6. Iterate up to 3 times if validation fails before escalating

## Protocols

- **tracking_and_recovery.md** — Must use for progress tracking and recovery
- **discipline.md** — Must use for systematic enumeration of requirements, tasks, risks
- **risk_management.md** — Must use for risk identification and mitigation
- **pragmatics.md** — Must use for user confirmation

---

## Steps

MUST read and follow steps in `dynamic_workflow_base.md`

---

## Preconditions

**Required:** Project or feature to plan

**Elicit if not provided:**
- Project context (codebase, docs, conventions)
- Scope boundaries (what's in vs out)
- Known constraints (time, resources, dependencies)

**Optional:** Existing requirements, prior plans, stakeholder input

## Postconditions

**Success:** Approved development plan with clear tasks, dependencies, and risks documented

**Failure:** Scope too ambiguous, requirements cannot be established, user rejects plan

---

## Deliverables

| Deliverable | Format | Location | Validation |
|-------------|--------|----------|------------|
| Development Plan | Markdown | Project-specific (e.g., `docs/plans/` or inline) | All requirements addressed, tasks sequenced, dependencies explicit, risks documented, user approved |

---

## Possible Tasks

Select and execute tasks to achieve Key Results. Each task shows which KR it serves.

### Orient on Project (→ KR1)

Execute project orientation using `orienting` primitive when beginning planning or context is unclear. Understand project structure, existing code, conventions, documentation, and current state. Map what exists and identify gaps.

**Fallback:** If orienting fails (too large, cannot access), narrow scope with user or gather context through interview.

### Elicit Requirements (→ KR2)

Execute requirements elicitation using `defining` primitive when requirements are unclear or incomplete. Establish what success looks like, acceptance criteria, and constraints. Document requirements with testable criteria.

**Fallback:** If requirements cannot be established, surface specific blockers to user for clarification.

### Research Unknowns (→ KR3)

Execute research using `investigating` primitive when significant unknowns exist that affect planning. Research technical approaches, dependencies, or risks before committing to plan. Reduce uncertainty before task sequencing.

**Fallback:** If research inconclusive, document uncertainty in plan with contingency tasks.

### Create Task Plan (→ KR3)

Execute task planning using `planning` primitive when requirements are clear and context is understood. Break work into tasks, identify dependencies, sequence appropriately, estimate complexity. Surface risks and unknowns.

**Fallback:** If planning blocked by unclear requirements, return to Elicit Requirements.

### Identify and Mitigate Risks (→ KR3)

Execute risk assessment using `risk_management.md` when plan exists but risks not fully addressed. Enumerate risks, assess likelihood and impact, determine mitigations. Add contingency tasks to plan if needed.

**Fallback:** If risks are too high, present options to user (reduce scope, add spikes, accept risk).

### Confirm with User (→ KR4)

Execute confirmation using `pragmatics.md` (Recommended Option, Option Presentation) when plan is complete. Present plan summary, highlight key decisions and risks, request approval before implementation begins.

**Fallback:** If user rejects, capture feedback and return to relevant task (requirements, planning, or risks).

---

## Available Primitives

- `orienting` — Understand project context and current state
- `defining` — Establish requirements and acceptance criteria
- `planning` — Sequence tasks with dependencies
- `investigating` — Research unknowns and reduce uncertainty

---

## Additional Notes

**Dev Planning vs Feature Workflow:** Dev Planning produces a plan and stops. Feature Workflow includes implementation and validation. Use Dev Planning when you need plan approval before committing to implementation.

**Plan granularity:** Tasks should be 1-4 hours of work. Larger tasks need decomposition. Smaller tasks can be grouped.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [Atlassian - Project Planning](https://www.atlassian.com/work-management/project-management/project-planning)
