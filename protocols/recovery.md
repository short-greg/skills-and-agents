# Recovery Protocol

Resume workflows from interruption by reconstructing context from trace.

---

## Goal

Enable workflows to resume intelligently after interruption, preserving completed work and avoiding re-execution.

---

## Intent

After compaction/interruption, new context has no memory. Recovery reads trace to determine what's completed, verifies artifacts, reconstructs context, and resumes appropriately.

---

## Scope

**Addresses:** How to resume (detect trace, interpret state, verify artifacts, reconstruct context, determine resume point, handle interruptions)

**Does not address:** What to record in trace (see `tracking` protocol)

---

## Core Concepts

**Recovery components - evaluation criteria:**

| Component | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Trace detection** | Check for trace on startup (before any action) | Assuming fresh start without checking |
| **Context reconstruction** | Re-read task spec, key decisions, artifacts | Proceeding without understanding why/what |
| **Artifact verification** | Files listed in trace exist and match expectations | Assuming trace is accurate without verification |
| **Resume decision** | User decides for ambiguous cases (mid-step, failures) | Auto-recovering from failures, silently re-running |
| **Session boundaries** | Resume entry added to trace (timestamp, resume point) | No record of when sessions occurred |

**Trace states:**

| Workflow Status | Last Step Status | Action |
|-----------------|------------------|--------|
| `completed` + `completed` | Workflow finished | Inform user, stop (don't re-run) |
| `in-progress` + `completed` | Between steps | Summarize completed, resume next step |
| `in-progress` + `in-progress` | Mid-step | **Ask user:** continue, re-run, or investigate |
| `in-progress` + `failed` | After failure | **Ask user:** surface failure, how to proceed |

---

## Techniques

**On workflow startup:**

1. Check for `${TASK_DIR}/trace.md` before any action
2. If found: read trace, reconstruct context (read task spec, review key decisions, verify artifacts exist)
3. Determine state from trace (see trace states table), apply recovery rule
4. Add resume entry to trace, continue from resume point

**Handling interruptions:**
- **Mid-step:** Present current focus and artifacts to user, ask: continue, re-run, or investigate
- **Failed step:** Present failure details to user, ask how to proceed (don't auto-retry)

---

## Use Cases and Triggers

Apply at workflow startup (before any action) when workflow uses tracking protocol.

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Always Check First:** Check for trace before any workflow action, even if you "know" it's fresh.

**Summarize Before Resuming:** Tell user what was completed before continuing.

**User Decision for Ambiguity:** Ask user for mid-step interruptions and failures rather than guessing.

### Anti-Patterns (❌)

**Skipping Recovery Check:** Assuming fresh start. **Fix:** Always check for trace.

**Silent Re-run:** Auto-running interrupted steps. **Fix:** Ask user for mid-step interruptions.

**Auto-Recovering from Failure:** Auto-retrying failed steps. **Fix:** Ask user how to proceed.

---

## Examples

**Resume Between Steps:** Trace shows Step 2 completed → "Steps 1-2 completed (orient, design). Resuming from Step 3: implement." [Add resume entry, continue]

**Mid-Step Interruption:** Trace shows Step 3 in-progress, current focus: token validation → Reconstruct context (read task spec, verify artifacts exist), present to user: "Step 3 interrupted. Current focus: token validation. Completed: user model, routes. Continue, re-run, or investigate?"

**Failed Step:** Trace shows Step 4 failed, challenges noted → Present to user: "Step 4 (validate) failed. Challenges: Auth endpoint returns 401 for valid tokens. Investigate, retry, go back to Step 3, or abort?"

---

## References

- `protocols/tracking.md` - Defines trace format that recovery interprets
