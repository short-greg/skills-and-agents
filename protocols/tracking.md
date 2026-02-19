> **NOTICE:** This protocol is a first draft and needs refinement.

# Tracking Protocol

Workflows must maintain an execution trace file throughout their run. The trace serves two purposes: it makes progress visible in real time, and it provides enough context to recover from compaction or interruption.

---

## When to Use

All multi-step workflows must follow this protocol. Primitives invoked within a workflow do not maintain their own trace — the workflow trace covers them.

---

## Trace File

**Location:** `${TASK_DIR}/trace.md` — in the task's working directory, alongside specs and outputs.

**Lifecycle:**
- Created when the workflow starts (or resumed if one already exists — see recovery protocol)
- Updated after every action completes or fails
- Never deleted during execution — it is the authoritative record of what happened

---

## Trace Format

```markdown
# Trace: [Workflow Name]

**Task:** [brief task description]
**Started:** [timestamp or session ID]
**Status:** in-progress | completed | failed

---

## [Step N]: [Action Name]

**Status:** in-progress | completed | failed
**Summary:** [1-3 sentences: what was done and what the outcome was]
**Challenges:** [any obstacles encountered, or "none"]
**Design changes:** [any decisions that deviated from the plan, or "none"]

---

## [Step N+1]: [Action Name]

...
```

---

## Rules

**Starting a step:**
- Add the step entry with `Status: in-progress` before beginning work
- This ensures an interrupted step is visible as incomplete, not missing

**Completing a step:**
- Update status to `completed`
- Fill in summary, challenges, design changes
- Keep summaries concise — the trace is a record, not a report

**Failing a step:**
- Update status to `failed`
- Record what was attempted and why it failed in the summary
- Record what was tried in challenges
- Stop and surface to user — do not continue to the next step

**Design changes:**
- Any deviation from the original plan or design must be recorded here
- This is the most important field for fault detection and recovery — silent design drift is how traces become misleading

**Current step:**
- The current step is always the last entry in the trace
- A step with `Status: in-progress` that is not the last entry indicates an interrupted workflow

---

## Example

```markdown
# Trace: feature-workflow

**Task:** Add user avatar upload to profile page
**Started:** 2025-01-15T14:32
**Status:** in-progress

---

## Step 1: define

**Status:** completed
**Summary:** Defined requirements for avatar upload: max 5MB, JPEG/PNG only, stored in S3, displayed at 48px in nav. User confirmed.
**Challenges:** none
**Design changes:** none

---

## Step 2: design

**Status:** completed
**Summary:** Designed upload flow: presigned S3 URL from API, client uploads directly, API stores key in user record. Chose presigned URL over server-side proxy to avoid memory pressure.
**Challenges:** Considered three storage approaches — settled on presigned URL after ruling out base64 (size) and server proxy (memory).
**Design changes:** Original plan assumed server-side upload; switched to presigned URL pattern after reviewing existing S3 usage in codebase.

---

## Step 3: implement

**Status:** in-progress
**Summary:** Implementing API endpoint for presigned URL generation.
**Challenges:** none yet
**Design changes:** none yet
```
