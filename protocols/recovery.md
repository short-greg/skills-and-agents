> **NOTICE:** This protocol is a first draft and needs refinement.

# Recovery Protocol

When a workflow starts, it must check whether an existing trace exists. If one does, recovery takes precedence over starting fresh.

---

## When to Use

All workflows that follow the tracking protocol must also follow the recovery protocol. Recovery is not optional — an unread trace is a silent failure.

---

## Recovery Check (REQUIRED ON STARTUP)

Before doing anything else, a workflow must:

1. Check for an existing `${TASK_DIR}/trace.md`
2. If none exists: proceed normally, create a new trace
3. If one exists: read it fully, then apply the recovery rules below

---

## Recovery Rules

**If the last step status is `completed` and workflow status is `completed`:**
- The workflow finished successfully in a prior session
- Inform the user and stop — do not re-run

**If the last step status is `completed` and workflow status is `in-progress`:**
- The workflow was interrupted between steps (e.g. compaction after a step completed but before the next began)
- Summarize completed steps to the user
- Resume from the next step in the workflow's sequence

**If the last step status is `in-progress`:**
- The workflow was interrupted mid-step
- The step's work may be partially complete or entirely incomplete
- Summarize the situation to the user: what step was in progress, what the summary says so far
- Ask the user whether to re-run the step from the beginning or investigate the partial state first
- Do not silently skip or silently re-run

**If the last step status is `failed`:**
- A prior session encountered an error and stopped
- Surface the failure to the user: what failed, what was recorded in challenges
- Ask the user how to proceed before doing anything
- Do not attempt to auto-recover from failures

---

## Resuming

When resuming, prepend a resume entry to the trace before continuing:

```markdown
---

## Resume

**Resumed:** [timestamp or session ID]
**Resuming from:** Step N — [action name]
**Prior steps completed:** [N-1]
```

This makes it clear in the trace that a session boundary occurred.

---

## Future Work

Fault detection — identifying steps that look complete but produced incorrect outputs — is deferred. The current protocol handles interruption and explicit failure, not silent correctness failures. This will be addressed when assessment criteria and validation are integrated into the tracking format.
