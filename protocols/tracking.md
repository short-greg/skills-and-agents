# Tracking Protocol

Maintain execution trace throughout workflow runs to enable progress visibility and recovery.

---

## Goal

Enable workflows to maintain a persistent record of progress that survives context loss (compaction, session timeout, errors) and supports visibility and recovery.

---

## Intent

Without tracking, interrupted workflows lose all context about completed work. Silent failures go undetected. After compaction, new context has no memory of prior work. Tracking solves this by maintaining an external trace file recording status, artifacts, decisions, and current focus.

---

## Scope

**Addresses:** What to record and when (trace file structure, step recording, status management, artifact tracking, decision documentation)

**Does not address:** How to resume from interruption (see `recovery` protocol)

---

## Core Concepts

**Trace components - evaluation criteria:**

| Component | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Pre-recording** | Step added with `in-progress` before work starts | Post-hoc recording (interrupted steps invisible) |
| **Status tracking** | Step and workflow status current (`in-progress`/`completed`/`failed`) | Status not updated, no failure detection |
| **Artifacts** | Files created/modified listed per step | No artifact tracking (can't verify state) |
| **Key decisions** | Choices and rationale recorded | Silent design drift, missing "why" |
| **Current focus** | In-progress steps show specific sub-task being worked on | Vague ("implementing" vs "implementing token validation") |
| **Challenges** | Obstacles and failures documented | Silent failures, no diagnosis info |

**Trace file:** `${TASK_DIR}/trace.md` (survives compaction), markdown format, created at start, updated after every action, never deleted. Current step = last entry. Both needed: TodoWrite (real-time) + trace file (persistence).

---

## Techniques

**Trace format:**
```markdown
# Trace: [Workflow Name]
**Task:** [brief description] | **Task spec:** [path] | **Started:** [timestamp] | **Status:** in-progress | completed | failed

## Step N: [Action Name]
**Status:** in-progress | completed | failed | **Summary:** [1-3 sentences]
**Artifacts:** [files] | **Key decisions:** [choices with rationale] | **Current focus:** [in-progress only]
**Challenges:** [obstacles or "none"]
```

**Recording process:**

**Start:** Pre-record with `in-progress` + initial `Current focus` before work begins

**During:** Update `Current focus`, `Artifacts`, `Key decisions` as work progresses (compaction can happen anytime)

**Complete:** Status → `completed`, write summary (1-3 sentences), finalize artifacts/decisions, remove focus, record challenges

**Fail:** Status → `failed`, record what/why, keep focus (shows failure point), record challenges, **stop workflow**

**Evaluation criteria:**
- Step pre-recorded with `in-progress` before work starts?
- Trace updated during execution (artifacts, decisions, focus)?
- Summary concise (1-3 sentences)?
- Current focus specific for in-progress steps?
- Workflow stops on failure (doesn't continue)?

---

## Use Cases and Triggers

Apply when:
- All multi-step workflows (must maintain trace)
- Recovery needed after interruption (trace enables resumption)

Skip when:
- Single-step operations or quick tasks
- Primitives invoked within workflows (workflow trace covers them)

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pre-record Before Work:** Add step entry before starting. Ensures interrupted steps visible.

**Update During Execution:** Artifacts, decisions, focus updated as you work. Compaction can happen anytime.

**Concise Summaries:** 1-3 sentences max. Focus on outcomes not process. Details belong in artifacts.

**Explicit Design Changes:** Record every deviation from plan with rationale.

### Anti-Patterns (❌)

**Post-hoc Recording:** Recording only after completion. **Fix:** Pre-record with `in-progress`.

**Verbose Summaries:** Paragraphs of detail. **Fix:** 1-3 sentences; details in artifacts.

**Silent Design Drift:** Changing approach without recording. **Fix:** Always record changes with rationale.

**Continuing After Failure:** Proceeding to next step after failure. **Fix:** Stop and surface failure to user.

---

## Examples

**In-progress:** Status: in-progress, Summary: Implementing presigned URL endpoint, Artifacts: Created src/api/avatar.py, Key decisions: Using boto3 with 5min expiry, Current focus: Token validation. Completed: route setup, S3 client. Remaining: validation, tests.

**Failed:** Status: failed, Summary: Tests failed — uploads >2MB timeout, Current focus: Running large file test, Challenges: S3 presigned URL 30s default timeout insufficient.

---

## References

- `protocols/recovery.md` - How to resume from trace after interruption
