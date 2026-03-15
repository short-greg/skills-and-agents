# Tracking and Recovery

Systematic progress recording that enables visibility, recovery, and adaptation.

Maintain persistent records during execution to survive interruption and support intelligent resumption.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts](#artifacts) — TodoWrite, Trace File, Checklists
- [Core Approaches](#core-approaches) — Progress Recording, State Capture, Recovery
- [Example Patterns](#example-patterns) — Starting, Recording, Lightweight, Recovery

---

## Goal

Enable workflows to maintain progress records that survive context loss and support visibility, recovery, and dynamic adaptation.

---

## Intent

Without tracking, interrupted workflows lose all context. Silent failures go undetected. Rigid plans can't adapt to reality. Tracking solves this by maintaining both real-time visibility and persistent state, enabling recovery from interruption and dynamic modification as needs become clear.

---

## Scope

Techniques for recording progress, capturing execution state, and recovering from interruption. Applies to any multi-step process that needs visibility, durability, or resumability.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use |
|----------|---------|-------------|
| **TodoWrite Checklist** | Real-time user-facing progress display | Workflows needing visible progress |
| **Trace File** | Persistent record at `${TASK_DIR}/trace.md`, survives compaction | Any workflow needing recovery capability |
| **Step Entry** | Individual step record: status, summary, artifacts, decisions, focus, challenges | Each step within a tracked workflow |

**Checklist Item Template:** `<Skill> - <KR> - <Task> - <Details>`

| Principle | Description | Good | Bad |
|-----------|-------------|------|-----|
| **Action verb** | Start with verb (Create, Review, Submit) | "Create user model" | "User model" |
| **One action** | Single task, not combined | "Add validation" | "Add validation and tests" |
| **Specific outcome** | Define what "done" looks like | "Returns 201 with user ID" | "Make it work" |
| **Measurable** | Verifiable completion criteria | "All tests pass" | "Tests are good" |

- Example: `feature - KR2 - Create user endpoint - POST /users, returns 201 with user ID`

---

## Core Approaches

### Progress Recording

Use progress recording techniques to make execution visible and durable.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Dual Tracking** | Executing multi-step process | To provide both visibility and durability | Maintain real-time display (TodoWrite) for user visibility AND persistent file (trace) for recovery, update both on state changes | Is progress visible to user? Will state survive interruption? | Is tracking only in-memory? Will context be lost on crash? |
| **Checklist Building** | Starting tracked process | To ensure all objectives are covered | Build from Key Results/objectives first, map tasks to each, format: `<Process> - KR<num> - <task>` for traceability | Does every objective have at least one task? Is format consistent? | Are tasks listed without linking to objectives? |
| **Dynamic Adaptation** | Requirements change during execution | To keep plan aligned with reality | Add items with letter suffixes (2a, 2b), remove with strikethrough and reason, use conditionals `(If X) action` and loop-backs `(If failed) return to step N` | Are changes documented with reasons? Can plan adapt? | Is checklist static despite changing needs? |

### State Capture

Use state capture techniques to record meaningful execution state.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Pre-Recording** | Before starting each step | To make interrupted steps visible | Add step entry with `in-progress` status and initial focus BEFORE starting work, compaction can happen anytime | Is step recorded before work begins? | Is recording only done after completion? |
| **Artifact Tracking** | Work produces outputs | To enable verification and handoff | Record files created/modified, key decisions with rationale, current focus (specific sub-task being worked on) | Are artifacts listed? Are decisions explained? | Is only status recorded without details? |
| **Completion Recording** | Step finishes or fails | To capture outcome accurately | On complete: update status, write 1-3 sentence summary, finalize artifacts. On fail: record what/why, keep focus (shows failure point), stop workflow | Are summaries concise? Does failure stop execution? | Are summaries verbose? Does work continue after failure? |

### Recovery

Use recovery techniques to resume intelligently after interruption.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Trace Detection** | Starting any tracked workflow | To avoid re-running completed work | Check for trace file BEFORE any action, if found read and interpret state, never assume fresh start | Is trace checked on startup? | Is fresh start assumed without checking? |
| **Context Reconstruction** | Trace found after interruption | To restore understanding before resuming | Re-read task spec, review key decisions, verify artifacts exist and match expectations, understand why/what before continuing | Is context rebuilt before resuming? Are artifacts verified? | Is trace assumed accurate without verification? |
| **Resume Decision** | Determining where to continue | To handle ambiguous states correctly | Completed workflow → inform user, stop. Between steps → summarize completed, resume next. Mid-step → ask user: continue, re-run, investigate. Failed → ask user how to proceed | Does user decide for ambiguous cases? | Are failures auto-retried? Are mid-step interruptions silently re-run? |

---

## Example Patterns

Structured example sequences for tracking. Far more are possible.

### Starting a Tracked Workflow

Use when beginning any multi-step process that needs tracking.

1. Check for existing trace file (Trace Detection)
2. If trace exists: reconstruct context and determine resume point (Recovery)
3. If no trace: list all Key Results/objectives
4. Create preliminary checklist mapping tasks to objectives (Checklist Building)
5. Initialize trace file with process name, start time, status
6. Pre-record first step before starting work (Pre-Recording)

**Exit Conditions:**
- Existing trace shows workflow completed → stop, inform user, don't re-run
- Existing trace shows mid-step or failed → stop, ask user how to proceed
- Objectives unclear → stop, clarify before building checklist

### Recording Step Execution

Use for each step during tracked execution.

1. Pre-record step with `in-progress` status (Pre-Recording)
2. Update artifacts, decisions, focus as work progresses (Artifact Tracking)
3. On completion: status → `completed`, write 1-3 sentence summary, finalize artifacts (Completion Recording)
4. On failure: status → `failed`, record what/why, keep focus, stop workflow (Completion Recording)
5. Update both real-time display and trace file (Dual Tracking)

**Exit Conditions:**
- Step fails → stop workflow, surface failure to user
- User requests change → adapt checklist with documented reason (Dynamic Adaptation)
- Requirements discovered → add items dynamically, continue

### Lightweight Tracking

Use when full dual tracking is overhead but progress visibility still needed.

1. Create text checklist in trace file or output (Text Checklist)
2. Mark items as you complete them: `- [x] completed`, `- [ ] pending`
3. Add brief notes for key decisions or blockers
4. Update on completion or interruption

**Exit Conditions:**
- Task becomes complex enough to need recovery → upgrade to full tracking
- No visibility needed → skip tracking entirely

### Recovering from Interruption

Use when trace file exists from previous session.

1. Read trace file, identify last step status (Trace Detection)
2. Re-read task spec, review recorded decisions (Context Reconstruction)
3. Verify artifacts exist and match trace (Context Reconstruction)
4. Determine resume action based on state (Resume Decision):
   - Workflow completed → inform user, stop
   - Between steps → summarize, resume next step
   - Mid-step → ask user: continue, re-run, or investigate
   - Failed step → ask user how to proceed
5. Add resume entry to trace, continue from determined point

**Exit Conditions:**
- Trace shows completed → stop, don't re-run
- Artifacts missing or corrupted → stop, ask user how to proceed
- User chooses to restart → clear trace, begin fresh

---

## Possible Actions

Select or execute possible actions based on context; this list is not exhaustive. Actions describe what to do; use techniques from Core Approaches for how.

| Action | Goal | When | How |
|--------|------|------|-----|
| **Initialize Tracking** | To set up progress visibility and recovery capability | Use when starting any multi-step process | Check for existing trace, build checklist from objectives, set up dual tracking |
| **Pre-Record Step** | To make in-progress work visible before starting | Use when beginning each step | Add entry with `in-progress` status and focus before work begins |
| **Update Step Record** | To capture progress and finalize outcome | Use when step produces outputs or completes | Record artifacts/decisions during work; on completion update status and write summary |
| **Detect Prior State** | To identify incomplete work from previous sessions | Use when starting workflow that may have prior state | Check for trace file, read state, never assume fresh start |
| **Resume from Interruption** | To continue appropriately after context loss | Use when prior incomplete state detected | Reconstruct context, verify artifacts, determine resume point, ask user for ambiguous states |
| **Adapt Tracking** | To modify plan when requirements change | Use when new requirements discovered or priorities shift | Add items with suffixes, remove with reason, use conditionals and loop-backs |
| **Report Final Result** | To communicate outcome with evidence | Use when workflow completes or cannot continue | Confirm each KR status, output success/failure, explain any failures |

---

## Risks

These are risks that misuse of this protocol poses to a project with mitigation strategies.

| Risk | When | Mitigation Strategy |
|------|------|---------------------|
| **Lost Progress** | Tracking skipped or steps not pre-recorded | Default to tracking for multi-step work; always pre-record step status before starting |
| **Tracking Overhead** | Full tracking applied to simple tasks | Use lightweight tracking for simple tasks; reserve dual tracking for workflows needing recovery |
| **Corrupted Resume** | Prior state assumed accurate without verification | Always verify artifacts exist and match trace before resuming; ask user for ambiguous states |
| **Duplicate Work** | Fresh start assumed without checking for existing trace | Always check for trace file on workflow start before any action |
| **Scope Creep** | Dynamic adaptation only adds items, never removes | Balance additions with removals; document why each change made |
| **Plan-Reality Drift** | Plan never adapted despite changing conditions | Review plan against reality periodically; adapt tracking when mismatch detected |
| **Silent Completion** | No final result communicated to user | Always end with explicit success/failure statement |
| **False Success** | Success claimed without verifying all KRs met | Require evidence for each KR before reporting success; list KR status explicitly |

---

## References

- [Microsoft Agent Framework - Checkpointing and Resuming Workflows](https://learn.microsoft.com/en-us/agent-framework/tutorials/workflows/checkpointing-and-resuming)
- [Checkpointing in Stream Processing Best Practices - Propelius](https://propelius.ai/blogs/checkpointing-in-stream-processing-best-practices)
- [LangGraph State Management - Medium](https://medium.com/@bharatraj1918/langgraph-state-management-part-1-how-langgraph-manages-state-for-multi-agent-workflows-da64d352c43b)
- [Project Tracking Methods and Metrics - Slack](https://slack.com/blog/collaboration/top-methods-and-metrics-for-tracking-project-progress)
- [Task Resumption and Reconstruction - Frontiers in Psychology](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2021.659451/full)
- [Workflow Recovery Overview - Informatica](https://docs.informatica.com/data-integration/powercenter/10-5/advanced-workflow-guide/workflow-recovery/workflow-recovery-overview.html)
