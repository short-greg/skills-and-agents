# Tracking Protocol

Maintain an execution trace throughout workflow runs to enable progress visibility and recovery.

---

## Goal

Enable workflows to maintain a persistent record of progress that survives context loss (compaction, session timeout, errors) and supports both visibility and recovery.

---

## Tools

**For real-time display:** Use `TodoWrite` tool to show progress to the user (if available in your AI tool)

**For persistence:** Use `Write`/`Edit` tools to maintain a `trace.md` file that survives compaction

Both are needed: TodoWrite shows live progress, trace.md enables recovery after interruption.

---

## Intent

Without tracking, workflows that are interrupted lose all context about what was completed. Silent failures go undetected. Progress is invisible. After compaction, the new context has no memory of prior work. Tracking solves this by maintaining an external trace file that records status, artifacts, decisions, and current focus — everything needed to resume intelligently.

---

## Scope

This protocol addresses **what to record and when**: trace file structure, step recording, status management, artifact tracking, and decision documentation. It does NOT address how to resume (see `recovery` protocol).

---

## Key Results

- Progress is visible in real time (current step, completed steps)
- Trace survives compaction (external file, not in-context)
- Artifacts are tracked (files created/modified per step)
- Decisions are recorded (key choices made, not just outcomes)
- Current focus is explicit (what specifically is in progress)
- Failed steps are diagnosable (challenges and outcomes recorded)

---

## Essential Concepts

### 1. Trace File

**What it is:** A markdown file that records workflow execution state.

**Location:** `${TASK_DIR}/trace.md` — in the task's working directory, alongside specs and outputs.

**Lifecycle:**
- Created when workflow starts (or resumed if one exists — see recovery protocol)
- Updated after every action completes or fails
- Never deleted during execution — it is the authoritative record

**Why markdown:** Human-readable, diffable, parseable, works with version control.

### 2. Step Recording

**What it is:** Each workflow step is recorded before starting and updated upon completion.

**Why it matters:** If a step is interrupted mid-execution, the trace shows it as `in-progress`. Without pre-recording, interrupted steps would be invisible.

**Recording sequence:**
1. Add step entry with `Status: in-progress` **before** beginning work
2. Execute the step
3. Update to `completed` or `failed` with summary

### 3. Status States

| Status | Meaning |
|--------|---------|
| `in-progress` | Step has started but not completed |
| `completed` | Step finished successfully |
| `failed` | Step encountered an error and stopped |

**Workflow-level status:**
- `in-progress` — Workflow is active
- `completed` — All steps completed successfully
- `failed` — Workflow stopped due to step failure

### 4. Artifacts

**What it is:** Files created or modified during each step.

**Why it matters:** After compaction, you need to know what changed. Without artifact tracking, you'd need to re-discover via `git status` or file inspection.

**Format:**
```markdown
**Artifacts:**
- Created: src/auth/login.py, src/auth/token.py
- Modified: src/api/routes.py
```

### 5. Key Decisions

**What it is:** Important choices made during a step, with rationale.

**Why it matters:** After compaction, the new context has no memory of *why* choices were made. Design changes capture deviations from plan; key decisions capture reasoning within the plan.

**Examples:**
- "Used JWT over session cookies — stateless, works with mobile clients"
- "Chose presigned URLs after ruling out base64 (size) and server proxy (memory)"

### 6. Current Focus

**What it is:** For in-progress steps, what specific sub-task is being worked on.

**Why it matters:** "implement" is too vague. If compaction happens mid-step, the new context needs to know: implementing which part? What approach? What's left?

**Format:**
```markdown
**Current focus:** Implementing token validation in login endpoint. Completed: user model, routes. Remaining: validation logic, tests.
```

### 7. Current Step Detection

**Rule:** The current step is always the last entry in the trace.

**Interrupted detection:** A step with `Status: in-progress` that is not the last entry indicates the workflow was interrupted after that step completed but before the trace was updated.

---

## Trace Format

```markdown
# Trace: [Workflow Name]

**Task:** [brief task description]
**Task spec:** [path to task spec or requirements doc]
**Started:** [timestamp or session ID]
**Status:** in-progress | completed | failed

---

## Step N: [Action Name]

**Status:** in-progress | completed | failed
**Summary:** [1-3 sentences: what was done and outcome]
**Artifacts:** [files created/modified, or "none"]
**Key decisions:** [choices made with rationale, or "none"]
**Current focus:** [for in-progress only: what specifically is being worked on]
**Challenges:** [obstacles encountered, or "none"]

---

## Step N+1: [Action Name]

...
```

**Note:** `Current focus` is only needed for in-progress steps. Completed steps don't need it.

---

## Processes

### Starting a Step

1. Add step entry with `Status: in-progress`
2. Record step name (primitive being invoked)
3. Set initial `Current focus` (what you're about to do)
4. Begin work

### During a Step

1. Update `Current focus` as work progresses
2. Update `Artifacts` as files are created/modified
3. Record `Key decisions` as choices are made

**Why update during?** Compaction can happen anytime. If trace only updates at completion, mid-step compaction loses all context.

### Completing a Step

1. Update status to `completed`
2. Write summary (1-3 sentences, outcomes)
3. Finalize artifacts list
4. Record key decisions (or "none")
5. Remove `Current focus` (no longer relevant)
6. Record challenges (or "none")

### Failing a Step

1. Update status to `failed`
2. Record what was attempted and why it failed
3. Keep `Current focus` (shows where failure occurred)
4. Record challenges
5. **Stop workflow** — do not continue to next step

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pattern 1: Pre-record Before Work**
- Add step entry before starting work
- Ensures interrupted steps are visible
- Example: Write "Step 3: implement - Status: in-progress" before writing any code

**Pattern 2: Concise Summaries**
- Keep summaries to 1-3 sentences
- Focus on outcomes, not process details
- Example: "Implemented auth endpoint with JWT validation. All tests pass."

**Pattern 3: Explicit Design Changes**
- Record every deviation from plan
- Include rationale
- Example: "Switched from cookie auth to header auth per security review"

### Anti-Patterns (❌)

**Anti-Pattern 1: Post-hoc Recording**
- Recording steps only after completion
- Interrupted steps become invisible
- **Instead:** Always pre-record with `in-progress`

**Anti-Pattern 2: Verbose Summaries**
- Writing paragraphs of detail in summaries
- Makes trace hard to scan
- **Instead:** 1-3 sentences max; details belong in artifacts

**Anti-Pattern 3: Silent Design Drift**
- Changing approach without recording
- Trace becomes misleading
- **Instead:** Always record design changes, even small ones

**Anti-Pattern 4: Continuing After Failure**
- Proceeding to next step after a failure
- Masks errors, causes cascading issues
- **Instead:** Stop and surface failure to user

---

## Examples

### Example 1: Feature Implementation (In Progress)

```markdown
# Trace: feature-workflow

**Task:** Add user avatar upload to profile page
**Task spec:** {TASK_DIR}/avatar-upload/specs.md # note the name can be different
**Started:** 2025-01-15T14:32
**Status:** in-progress

---

## Step 1: define

**Status:** completed
**Summary:** Defined requirements: max 5MB, JPEG/PNG only, stored in S3, displayed at 48px in nav.
**Artifacts:** Created tasks/avatar-upload/specs.md
**Key decisions:** none
**Challenges:** none

---

## Step 2: design

**Status:** completed
**Summary:** Designed upload flow: presigned S3 URL from API, client uploads directly.
**Artifacts:** Modified {TASK_DIR}/avatar-upload/specs.md (added design section)
**Key decisions:** Chose presigned URL over base64 (size limits) and server proxy (memory). Stateless approach.
**Challenges:** none

---

## Step 3: implement

**Status:** in-progress
**Summary:** Implementing presigned URL endpoint.
**Artifacts:** Created src/api/avatar.py, modified src/api/routes.py
**Key decisions:** Using boto3 generate_presigned_url with 5min expiry
**Current focus:** Token validation in upload endpoint. Completed: route setup, S3 client. Remaining: validation, tests.
**Challenges:** none yet
```

### Example 2: Failed Step

```markdown
## Step 4: validate

**Status:** failed
**Summary:** Tests failed — uploads over 2MB timeout.
**Artifacts:** Created tests/test_avatar.py
**Key decisions:** none
**Current focus:** Was running large file upload test when failure occurred
**Challenges:** S3 presigned URL has 30s default timeout; large files exceed this.
```

---

## Key Takeaways

- **Pre-record steps** — Write `in-progress` before starting work
- **Update during execution** — Artifacts, decisions, focus as you work (compaction can happen anytime)
- **Track artifacts** — Files created/modified per step
- **Record key decisions** — Choices and rationale, not just outcomes
- **Maintain current focus** — For in-progress steps, what specifically is being worked on
- **Stop on failure** — Do not continue to next step after failure

---

## When to Apply This Protocol

- All multi-step workflows must maintain a trace
- Primitives invoked within workflows do not maintain separate traces — the workflow trace covers them
- Single-step operations or quick tasks may skip tracking
- Recovery protocol depends on tracking — they work together

---

## Integration with Recovery Protocol

The tracking protocol produces the trace file that the recovery protocol consumes:

1. **Tracking** records state during execution
2. **Recovery** reads state to resume after interruption

See `protocols/recovery.md` for how traces are used for resumption.
