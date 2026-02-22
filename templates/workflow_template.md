---
name: workflow-name
description: >
  [What this workflow does. Include trigger keywords and phrases users naturally say.]
argument-hint: "[optional: e.g. [feature] or [bug description]]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, TodoWrite
---

# Workflow Name

**Goal:** [What this workflow achieves - one sentence.]

**Intent:** [Why this workflow exists - what problem it solves, what it prevents.]

**Scope:** [What this workflow covers. Be specific about the types of activities and artifacts.]

---

## Key Results

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress through workflow execution
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Build checklist from Key Results. Update dynamically.
- `reasoning.md` — Reason about approach before starting
- `[other].md` — [when/why this protocol applies]

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — [when to use in this workflow]
- `define` — [when to use in this workflow]
- `design` — [when to use in this workflow]
- `implement` — [when to use in this workflow]
- `validate` — [when to use in this workflow]

---

## Constraints

- [constraint 1]
- [constraint 2]
- [constraint 3]

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### [Task Name] (→ KR#)

[What this task does. Reference primitives or protocols as needed.]

### [Task Name] (→ KR#, KR#)

[What this task does.]

### [Final Task] (→ KR#)

[What this task does.]

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Build checklist from Key Results before starting
- Mark items complete immediately after finishing each task
- Add sub-items dynamically when scope expands
- Report progress after each completed item

---

## Preconditions

**Must be provided:** [what user must provide]

**Self-satisfiable:** [what workflow will gather]

---

## Postconditions

**Success:** [state when workflow completes successfully]

**Failure:** [state when workflow fails]

---

## Recovery

Per `recovery.md` — check for existing trace on startup, resume from last completed task.

---

## Iteration

Per `checklists.md`:
- Max N iterations before escalating
- Each iteration must show progress
- If same failure recurs, escalate immediately

---

## Customization Points

**Prototype:** [how to adapt for prototypes]

**Production:** [how to adapt for production]

**Library:** [how to adapt for libraries]
