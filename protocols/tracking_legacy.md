# Tracking Protocol

**Type:** Protocol

Maintain progress records during workflow execution to enable visibility, recovery, and adaptation.

---

## Goal

Enable workflows to maintain a persistent record of progress that survives context loss (compaction, session timeout, errors) and supports visibility, recovery, and dynamic adaptation.

---

## Intent

Without tracking, interrupted workflows lose all context about completed work. Silent failures go undetected. Rigid checklists can't adapt when reality differs from plans. Tracking solves this by maintaining both real-time visibility (TodoWrite) and persistent state (trace file), while allowing dynamic modification as needs become clear.

---

## Scope

**Addresses:** Progress tracking mechanisms (TodoWrite, trace files), checklist structure and format, dynamic adaptation (add/remove items), what to record (artifacts, decisions, challenges), KR→Task mapping

**Does not address:** How to resume from interruption (see `recovery.md`), workflow design (see workflow creation guideline)

---

## Core Concepts

### Dual Tracking System

**TodoWrite** - Real-time visibility, shown to user immediately, lost on compaction
**Trace file** - Persistent record, survives compaction/errors, enables recovery

Both needed: TodoWrite for UX, trace file for durability.

### Checklist Structure

**Format:** `<Skill> - KR<num> - <task>` provides full traceability from task to objective

**Key principles:**
- Build from Key Results, not Tasks (ensures all success criteria covered)
- One item `in_progress` at a time
- Dynamic adaptation (add/remove as needs become clear)
- Sequential numbering with letter suffixes (2a, 2b) for sub-items

### Trace File Format

**Location:** `${TASK_DIR}/trace.md` (never deleted)

**Structure:**
```markdown
# Trace: [Workflow Name]
**Task:** [brief description] | **Task spec:** [path] | **Started:** [timestamp] | **Status:** in-progress | completed | failed

## Step N: [Action Name]
**Status:** in-progress | completed | failed | **Summary:** [1-3 sentences]
**Artifacts:** [files] | **Key decisions:** [choices with rationale] | **Current focus:** [in-progress only]
**Challenges:** [obstacles or "none"]
```

### What to Record

| Component | Purpose | When to Record |
|-----------|---------|----------------|
| **Pre-recording** | Step added with `in-progress` before work starts | Start of each step |
| **Status** | `in-progress` / `completed` / `failed` | Throughout execution |
| **Artifacts** | Files created/modified | As work progresses |
| **Key decisions** | Choices and rationale | When decisions made |
| **Current focus** | Specific sub-task being worked on | During in-progress steps |
| **Challenges** | Obstacles and failures | When encountered |
| **Summary** | 1-3 sentence outcome | Step completion |

---

## Techniques

### Building Checklists

1. **List all Key Results** from workflow/skill (before starting work)
2. **Create preliminary items** - one per KR, task TBD if unclear: `- [ ] skill - KR1 - (clarify requirements)`
3. **Interview/investigate** to understand what's needed
4. **Add specific tasks** for each KR: `- [ ] skill - KR2 - Analyze repository structure`
5. **Verify coverage** - every Key Result has at least one task

**Evaluation criteria:**
- Every Key Result mapped to at least one task?
- Format includes skill name and KR number for traceability?

### Recording Process

**Start:** Pre-record step with `in-progress` + initial `Current focus` before work begins

**During:** Update `Current focus`, `Artifacts`, `Key decisions` as work progresses (compaction can happen anytime)

**Complete:** Status → `completed`, write summary (1-3 sentences), finalize artifacts/decisions, remove focus, record challenges

**Fail:** Status → `failed`, record what/why, keep focus (shows failure point), record challenges, **stop workflow**

**Evaluation criteria:**
- Step pre-recorded with `in-progress` before work starts?
- Trace updated during execution (artifacts, decisions, focus)?
- Summary concise (1-3 sentences)?
- Current focus specific for in-progress steps?
- Workflow stops on failure (doesn't continue)?

### Dynamic Adaptation

**Add items:** Use letter suffixes to insert between numbers: `2a, 2b, 2c`. Mark `(NEW)` on first appearance.

**Remove items:** Strike through with reason: `~~[ ] item~~ (REMOVED: out of scope)`

**Expand items:** Add sub-items when scope becomes clear, parent remains as anchor

**Conditionals:** `(If condition) Action` - execute if condition met, skip otherwise

**Loop-backs:** `(If coverage <85%) Add tests and return to step 1` - prevents duplication

**Evaluation criteria:**
- Items added/removed during execution with reasons documented?
- Loop-backs used instead of duplicating "retry" items?

### Progress Visibility

- Mark `[x]` in TodoWrite when complete
- Keep exactly ONE item `in_progress` at a time
- Update both TodoWrite AND trace file when status changes
- Record modifications with timestamp and reason

---

## Use Cases and Triggers

Apply when:
- All multi-step workflows (must maintain trace)
- Starting tasks - create preliminary checklist
- Discovering new requirements - add items dynamically
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

**Key Results → Tasks Mapping:** Build checklist from KRs, not tasks. Prevents cherry-picking and orphaned objectives.

**Dynamic Expansion:** Start with high-level items, expand when scope becomes clear. Example: `2. Implement features (details TBD)` → `2a. Add authentication, 2b. Create dashboard, 2c. API endpoints`

**Loop-Back Pattern:** `- [ ] 1. Run tests` → `- [ ] 2. (If tests fail) Fix and return to step 1` instead of duplicating test steps.

**Audit Trail:** `~~[ ] Add caching layer~~ (REMOVED: out of scope per user)` preserves context.

**Explicit Design Changes:** Record every deviation from plan with rationale.

### Anti-Patterns (❌)

**Post-hoc Recording:** Recording only after completion. **Fix:** Pre-record with `in-progress`.

**Static Checklists:** Requirements change but checklist doesn't adapt. **Fix:** Add/remove items as needs become clear, document why.

**Verbose Summaries:** Paragraphs of detail. **Fix:** 1-3 sentences; details in artifacts.

**Task-First Build:** Building from tasks alone misses Key Results. **Fix:** Start with all KRs, then map tasks to each.

**Duplicating Retry Items:** "Run tests, Run tests again, Run tests one more time". **Fix:** Use loop-back pattern.

**Silent Design Drift:** Changing approach without recording. **Fix:** Always record changes with rationale.

**Silent Deletion:** Removing items without explanation. **Fix:** Strike through with reason, preserve audit trail.

**Continuing After Failure:** Proceeding to next step after failure. **Fix:** Stop and surface failure to user.

---

## Examples

### Checklist Format

`- [ ] setup-skill-env - KR2 - Analyze repository structure`

### Dynamic Expansion

```markdown
Before: - [ ] 2. Implement features (details TBD)
After:  - [ ] 2. Implement features
          - [ ] 2a. Add authentication (NEW)
          - [ ] 2b. Create user dashboard (NEW)
```

### Conditional

`- [ ] (If coverage <85%) Add tests for uncovered code`

### Loop-back

`- [ ] 3. (If validation fails) Fix issues and return to step 2`

### Removal

`~~[ ] Add caching layer~~ (REMOVED: out of scope per user)`

### Trace Entry - In-Progress

Status: in-progress, Summary: Implementing presigned URL endpoint, Artifacts: Created src/api/avatar.py, Key decisions: Using boto3 with 5min expiry, Current focus: Token validation. Completed: route setup, S3 client. Remaining: validation, tests.

### Trace Entry - Failed

Status: failed, Summary: Tests failed — uploads >2MB timeout, Current focus: Running large file test, Challenges: S3 presigned URL 30s default timeout insufficient.

---

## References

- `protocols/recovery.md` - How to resume from trace after interruption
