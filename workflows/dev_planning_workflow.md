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

1. Progress tracked per `protocols/tracking_and_recovery.md` — file-based progress tracking
2. Recoverable from interruption — check for existing progress on startup
3. Validate context before defining requirements
4. Validate requirements before planning tasks
5. Surface risks early — unknowns identified before task sequencing
6. Iterate up to 3 times if validation fails before escalating

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

Use `orienting` mode to understand project context and current state before planning begins.

**Inputs:**
- Subject to orient on: The project/feature being planned
- Current state: Codebase, documentation, existing plans (if available)
- Target state: What the plan needs to achieve (optional)

**Output:** Orientation report documenting:
- Project structure, conventions, and patterns
- Existing implementations related to planned work
- Gaps between current state and planning needs
- Key constraints, dependencies, and technical debt

**Fallback:** If scope too large, use `interviewing` mode to narrow scope with user.

---

### Elicit Requirements (→ KR2)

Use `defining` mode to establish clear requirements with acceptance criteria.

**Inputs:**
- Task or goal: The project/feature to define
- Context: Orientation report from previous step
- Existing requirements or specifications (if available)

**Output:** Requirements document with:
- Success criteria (what "done" looks like)
- Acceptance criteria (testable conditions)
- Constraints (time, resources, dependencies)
- Scope boundaries (what's in vs out)

**Fallback:** If requirements cannot be established, use `interviewing` mode to clarify with user.

---

### Research Unknowns (→ KR3)

Use `investigating` mode when significant technical unknowns exist that affect planning.

**Inputs:**
- Question or topic: Technical unknowns identified in requirements or orientation
- Context: Orientation report and requirements document
- Investigation scope: Specific aspects needing research

**Output:** Investigation summary with:
- Findings with sources cited
- Confidence levels (known vs suspected vs unknown)
- Viable approaches with tradeoffs
- Recommendations for plan

**Fallback:** If inconclusive, document uncertainty in plan with contingency tasks.

---

### Create Task Plan (→ KR3)

Use `planning` mode to sequence work into actionable tasks with dependencies.

**Inputs:**
- Goal or task: The project/feature to plan (from requirements)
- Existing definition: Requirements document from previous step
- Codebase context: Orientation report
- Known risks: Investigation findings (if available)

**Output:** Development plan with:
- Complete list of tasks (1-4 hour granularity)
- Task dependencies (what must be done before what)
- Sequenced plan with milestones
- Risks and unknowns flagged
- User confirmation that plan is acceptable

**Fallback:** If planning blocked by unclear requirements, return to Elicit Requirements task.

---

## Available Modes

- `orienting` — Understand project context and current state
- `defining` — Establish requirements and acceptance criteria
- `planning` — Sequence tasks with dependencies
- `investigating` — Research unknowns and reduce uncertainty

---

## Additional Notes

**Dev Planning vs Feature Workflow:** Dev Planning produces a plan and stops. Feature Workflow includes implementation and validation. Use Dev Planning when you need plan approval before committing to implementation.

**Plan granularity:** Tasks should be 1-4 hours of work. Larger tasks need decomposition. Smaller tasks can be grouped.

**Risk identification and user confirmation:** These capabilities are built into the modes above:
- `planning` mode includes "Surface Unknowns" action (identifies risks) and "Confirm with User" action (gets approval)
- Use these mode actions as part of the Create Task Plan task

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [PMI - Work Breakdown Structure](https://www.pmi.org/learning/library/work-breakdown-structure-basic-principles-4883)
- [Atlassian - Project Planning](https://www.atlassian.com/work-management/project-management/project-planning)
