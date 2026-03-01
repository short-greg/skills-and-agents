# Dynamic Workflow Base

Workflows that inherit from this base execute dynamically, evaluating which tasks to run based on context rather than following a fixed sequence.

---

## Steps

1. **Check recovery state** per `tracking_and_recovery.md` — if progress file exists, read state and resume from last completed task
2. **Set up progress tracking** — create/update progress file with KRs, deliverables checklist, and task status
3. **Resolve preconditions** — elicit missing required inputs from user
4. **Evaluate tasks** — output table of available tasks: | Task | Primitive/Workflow | Why Use | Why Not | Fallback |
5. **Output task plan** — using `planning.md`, sequence selected tasks with dependencies, identify parallel opportunities
6. [PLACEHOLDER: **Execute tasks** — workflow-specific tasks per Possible Tasks section. On failure, execute fallback or diagnose.]
7. **Validate deliverables** — verify each deliverable meets its validation criteria per Deliverables section
8. **Confirm and report result** — output success or failure with evidence for each KR

---

## On Task Failure

When a task fails:
1. Check fallback from evaluation table
2. If fallback exists: execute fallback, update progress, re-evaluate remaining tasks
3. If no fallback: diagnose failure using `investigating` primitive
4. If diagnosis suggests remediation: add remediation tasks, continue
5. If max iterations reached (default 3) or unrecoverable: report failure with diagnosis

---

## Output Requirements

**Must report result** — Every workflow ends with explicit success or failure. Never complete silently.

**File-based tracking** — Progress tracked in file (location specified by workflow or project conventions). Enables recovery from interruption.

**Deliverables validated** — Each deliverable checked against its validation criteria before reporting success.

---

## Progress File Format

Create at workflow start, update as tasks complete:

```markdown
## Workflow: [workflow-name]
Started: [timestamp]
Status: in_progress | completed | failed

## Key Results
- [ ] KR1: [description]
- [ ] KR2: [description]

## Deliverables
- [ ] [deliverable]: [validation status]

## Tasks
- [x] [completed task] — [outcome]
- [ ] [pending task]

## Current State
Last completed: [task name]
Next: [task name or "validate deliverables"]
Failures: [count], Last failure: [task and reason]
```

---

## Completion Output

Always end with explicit result:

**Success:**
```
## Result: Success
- KR1: [evidence]
- KR2: [evidence]
- Deliverables: [list with locations]
```

**Failure:**
```
## Result: Failure
- KR1: [status]
- KR2: [status]
- Reason: [why workflow could not complete]
- Diagnosis: [root cause if determined]
- Partial deliverables: [what was produced, if any]
```
