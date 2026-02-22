# Recovery Protocol

Resume workflows from interruption by reading and interpreting the execution trace.

---

## Goal

Enable workflows to resume intelligently after interruption (especially compaction), preserving completed work, reconstructing necessary context, and avoiding re-execution.

---

## Intent

Workflows are interrupted by compaction, session timeout, user abort, or errors. After compaction, the new context has no memory of prior work — it must reconstruct everything from external state. Recovery reads the trace file to determine what was completed, verifies artifacts match expectations, reconstructs context, and resumes from the appropriate point.

---

## Scope

This protocol addresses **how to resume**: detecting traces, interpreting state, verifying artifacts, reconstructing context, determining resume point, and handling interruption scenarios. It does NOT address what to record (see `tracking` protocol).

---

## Key Results

- Completed work is preserved (not re-executed)
- Context is reconstructed (task spec, key decisions re-read)
- Artifacts are verified (files match trace expectations)
- Interrupted steps are handled appropriately (user decides)
- Resume point is clearly communicated to user
- Session boundaries are recorded in trace

---

## Essential Concepts

### 1. Recovery Check on Startup

**Every workflow must check for an existing trace before doing anything else.**

```
1. Check for existing `${TASK_DIR}/trace.md`
2. If none exists → proceed normally, create new trace
3. If one exists → read fully, apply recovery rules
```

**Why mandatory:** An unread trace is a silent failure. The workflow might re-do completed work or skip important context.

### 2. Trace States

The trace tells you where the workflow was when it stopped:

| Workflow Status | Last Step Status | Meaning |
|-----------------|------------------|---------|
| `completed` | `completed` | Workflow finished successfully |
| `in-progress` | `completed` | Interrupted between steps |
| `in-progress` | `in-progress` | Interrupted mid-step |
| `in-progress` | `failed` | Stopped due to error |

### 3. Recovery Rules

**Workflow completed (`completed` + `completed`):**
- Workflow finished in a prior session
- Inform user and stop — do not re-run
- User can explicitly request re-execution if needed

**Interrupted between steps (`in-progress` + `completed`):**
- Last step finished, but workflow was interrupted before next step began
- Summarize completed steps to user
- Resume from next step in sequence

**Interrupted mid-step (`in-progress` + `in-progress`):**
- Step was in progress when interruption occurred
- Work may be partially complete or entirely incomplete
- Summarize situation: what step, what summary says so far
- **Ask user** whether to re-run step or investigate partial state
- Do not silently skip or silently re-run

**Stopped after failure (`in-progress` + `failed`):**
- Prior session encountered error and stopped
- Surface failure: what failed, challenges recorded
- **Ask user** how to proceed
- Do not auto-recover from failures

### 4. Context Reconstruction

**What it is:** After compaction, re-read external sources to rebuild understanding.

**Why it matters:** The new context has no memory. Trace tells you *where* you are, but you need to re-read *why* and *what* to continue intelligently.

**Process:**
1. Read task spec (path in trace header)
2. Read key decisions from completed steps
3. Review artifacts to understand current state
4. Read current focus for in-progress steps

### 5. Artifact Verification

**What it is:** Check that files listed in trace actually exist and match expectations.

**Why it matters:** Trace says "Created src/auth/login.py" — verify it exists. If not, something went wrong.

**Process:**
1. For each artifact in trace, verify file exists
2. If missing: flag discrepancy to user
3. If present: optionally verify content matches summary

### 6. Resume Entry

When resuming, add a resume entry to the trace:

```markdown
---

## Resume

**Resumed:** [timestamp or session ID]
**Resuming from:** Step N — [action name]
**Prior steps completed:** [N-1]
**Context reconstructed from:** [task spec path, key decisions reviewed]
```

This makes session boundaries visible in the audit trail.

---

## Processes

### On Workflow Startup

1. Check for `${TASK_DIR}/trace.md`
2. If not found: create new trace, proceed normally
3. If found: read entire trace
4. **Reconstruct context:**
   - Read task spec (path from trace header)
   - Review key decisions from completed steps
   - Understand current state from artifacts
5. **Verify artifacts:** Check files exist as expected
6. Determine state (see trace states table)
7. Apply appropriate recovery rule
8. Add resume entry to trace
9. Continue workflow from resume point

### Handling Mid-Step Interruption

1. Read trace for interrupted step
2. Read `Current focus` to understand what was in progress
3. Verify artifacts created so far
4. Present to user: "Step N was in progress. Focus: [...]. Artifacts created: [...]"
5. Ask: "Continue from current focus, re-run step, or investigate?"
6. If continue: pick up from current focus
7. If re-run: mark step as `in-progress`, start fresh
8. If investigate: user examines artifacts, decides next action

### Handling Failure

1. Read trace for failed step
2. Read `Current focus` to see where failure occurred
3. Present to user: "Step N failed at [...]. Challenges: [...]"
4. Ask: "How would you like to proceed?"
5. Options: retry step, skip step, abort workflow, investigate
6. Do not auto-retry — failures need human judgment

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pattern 1: Always Check First**
- Check for trace before any workflow action
- Even if you "know" it's a fresh start
- Example: First line of workflow execution checks `${TASK_DIR}/trace.md`

**Pattern 2: Summarize Before Resuming**
- Tell user what was completed before continuing
- Provides context for where we are
- Example: "Steps 1-3 completed. Resuming from Step 4: implement"

**Pattern 3: User Decision for Ambiguity**
- Mid-step interruption is ambiguous (how much completed?)
- Ask user rather than guessing
- Example: "Step was in progress. Re-run or investigate?"

### Anti-Patterns (❌)

**Anti-Pattern 1: Skipping Recovery Check**
- Assuming fresh start without checking
- Causes re-execution of completed work
- **Instead:** Always check for existing trace

**Anti-Pattern 2: Silent Re-run**
- Automatically re-running interrupted steps without asking
- User might lose partial work
- **Instead:** Ask user for mid-step interruptions

**Anti-Pattern 3: Auto-Recovering from Failure**
- Automatically retrying failed steps
- Failures often need investigation, not retry
- **Instead:** Surface failure, ask user how to proceed

**Anti-Pattern 4: Ignoring Session Boundaries**
- Not recording when resumption occurred
- Makes trace confusing (which session did what?)
- **Instead:** Add resume entry to trace

---

## Examples

### Example 1: Resume Between Steps

**Trace shows:**
```markdown
**Status:** in-progress

## Step 2: design
**Status:** completed
**Summary:** Designed API with three endpoints...
```

**Recovery action:**
```
"Steps 1-2 completed (orient, design). Resuming from Step 3: implement."
[Add resume entry, continue with Step 3]
```

### Example 2: Mid-Step Interruption (Compaction)

**Trace shows:**
```markdown
**Task spec:** tasks/auth_feature/tracking.md
**Status:** in-progress

## Step 3: implement
**Status:** in-progress
**Summary:** Implementing auth endpoint.
**Artifacts:** Created src/auth/login.py, modified src/api/routes.py
**Key decisions:** Using JWT with 24h expiry, refresh tokens stored in DB
**Current focus:** Token validation logic. Completed: user model, routes. Remaining: validation, tests.
```

**Recovery action:**
```
1. Re-read {TASK_DIR}/auth-feature/tracking.md to understand progress and {TASK_DIR}/auth-feature/requirements.md requirements. Note the requirements file may be different.
2. Note key decisions: JWT with 24h expiry, refresh tokens in DB
3. Verify artifacts exist: src/auth/login.py ✓, src/api/routes.py ✓
4. Present to user:

"Step 3 (implement) was interrupted.
Current focus: Token validation logic.
Completed: user model, routes.
Remaining: validation, tests.
Artifacts verified: src/auth/login.py, src/api/routes.py

How would you like to proceed?
1. Continue from token validation
2. Re-run Step 3 from beginning
3. Investigate current state first"
```

### Example 3: Failed Step

**Trace shows:**
```markdown
## Step 4: validate
**Status:** failed
**Summary:** Tests failed — 3 of 12 tests failing.
**Challenges:** Auth endpoint returns 401 for valid tokens.
```

**Recovery action:**
```
"Step 4 (validate) failed in prior session.
Summary: Tests failed — 3 of 12 tests failing.
Challenges: Auth endpoint returns 401 for valid tokens.

How would you like to proceed?
1. Investigate the failure
2. Retry Step 4
3. Go back to Step 3 (implement) to fix
4. Abort workflow"
[Wait for user decision]
```

---

## Key Takeaways

- **Always check for existing trace** — recovery check is mandatory on startup
- **Reconstruct context first** — re-read task spec, key decisions, artifacts
- **Verify artifacts exist** — trace says file was created, confirm it exists
- **Use current focus** — know exactly where in-progress step was interrupted
- **Don't re-execute completed work** — read trace to know what's done
- **Ask user for ambiguous situations** — mid-step interruption, failures
- **Record session boundaries** — add resume entry when resuming

---

## When to Apply This Protocol

- All workflows that use the tracking protocol must use recovery
- Recovery check happens at workflow startup, before any other action
- Primitives don't have their own recovery — workflow trace covers them

---

## Integration with Tracking Protocol

Recovery depends on tracking:

| Tracking (what to record) | Recovery (how to resume) |
|---------------------------|--------------------------|
| Records status, artifacts, decisions | Reads status to determine resume point |
| Records current focus | Uses focus to understand mid-step state |
| Records task spec path | Re-reads spec to reconstruct context |
| Records artifacts | Verifies artifacts exist |

The trace format from `protocols/tracking.md` provides the structure recovery interprets.
