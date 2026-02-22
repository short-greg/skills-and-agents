---
name: bugfix-workflow
description: >
  Use when fixing bugs systematically with hypothesis-driven investigation.
  Triggers on: "fix this bug", "debug this issue", "systematic bugfix".
argument-hint: "[bug description or reproduction steps]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
protocols:
  - tracking  # Track progress through investigate → hypothesize → fix → validate phases
  - recovery  # Resume from interruption at any phase
  - checklist_management  # Create dynamic checklist based on bug complexity
  - reasoning_patterns  # Form hypotheses before investigating, verify fix after
  - goals_and_objectives  # Define what "fixed" means with binary pass/fail criteria
  - manage_complexity_uncertainty_risk  # Identify root cause before fixing symptoms
  - doc_maintenance  # Update documentation if bug reveals incorrect docs
---

# Bugfix Workflow

**Goal:** Fix bugs by identifying root cause through hypothesis-driven investigation, implementing minimal fix, and preventing regression.

**Intent:** Prevent haphazard debugging where developers fix symptoms rather than causes and introduce regressions. Ensure root causes are identified through systematic investigation and minimal fixes are applied with regression tests.

**Scope:** Complete bug investigation and fix: reproduction, hypothesis generation, root cause confirmation, minimal fix, regression prevention.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (declarative goals with imperative hypothesis-test loop)

---

## Key Results

Per `goals_and_objectives` protocol — outcome-oriented results:

**Required:**
1. **Bug is reproducible** — failing test demonstrates the bug before fix
2. **Root cause is identified** — evidence confirms cause, not speculation
3. **Fix is minimal** — changes address root cause only, no unrelated modifications
4. **Regression is prevented** — new test catches this bug if reintroduced

**Conditional:**
5. **Documentation is updated** — if fix changes behavior or reveals missing docs (per `doc_maintenance`)
6. **All tests pass** — existing test suite passes with fix applied

---

## Available Primitives

`orient`, `investigate`, `brainstorm`, `define`, `implement`, `validate`

---

## Constraints

- Reproduce before diagnosing (create failing test first)
- Confirm root cause before implementing fix
- Test hypotheses systematically (most likely first)
- Fix must be minimal (no "while I'm here" changes)
- Must add regression test
- On validation failure, investigate before re-fixing

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Bug characteristics** — Clear reproduction? Intermittent? Environment-specific?
2. **Uncertainty level** — Obvious cause or deep investigation needed?
3. **Hypothesis count** — Low uncertainty: 2-3 hypotheses. High uncertainty: 5+
4. **Risk assessment** — Could fix introduce regressions? Critical system?

Output reasoning before proceeding.

---

## Progress Tracking (Required)

Per `checklist_management` protocol — create and maintain a checklist throughout execution.

**On workflow start, create this checklist:**

```markdown
## Bugfix Progress

- [ ] 1. Understand context (orient)
- [ ] 2. Reproduce bug with failing test (investigate)
- [ ] 3. Generate hypotheses (brainstorm)
- [ ] 4. Investigate root cause (investigate) — Gate: root cause confirmed
- [ ] 5. Define fix scope (define)
- [ ] 6. Implement minimal fix (implement)
- [ ] 7. Validate fix (validate)
```

**Rules:**
- Display checklist at start of workflow
- Mark items `[x]` immediately after completing each phase
- On hypothesis failure, add sub-items (e.g., "4a. Test hypothesis 1", "4b. Test hypothesis 2")
- On validation failure, add remediation items and loop back
- Report progress after each completed item: "✅ [item] — [brief summary]"

---

## Execution

Execute by selecting and sequencing primitives to achieve key results. The following phases provide guidance, not rigid steps.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand codebase, identify relevant code areas, testing conventions.

### Phase 2: Reproduce Bug

**Primitive:** `investigate` — See `primitives/investigate.md`

Create failing test that demonstrates the bug. This becomes the success criterion.

**On failure to reproduce:** Per `manage_complexity_uncertainty_risk` protocol — gather more information, check environment factors, consider intermittent issues.

### Phase 3: Generate Hypotheses

**Primitive:** `brainstorm` — See `primitives/brainstorm.md`

Generate potential root causes ranked by likelihood. Scale hypothesis count with uncertainty.

**Pre-hypothesis investigation:** If domain is unfamiliar, invoke `investigate` first to gather context.

### Phase 4: Investigate Root Cause

**Primitive:** `investigate` — See `primitives/investigate.md`

Test hypotheses through instrumentation, logging, analysis until root cause is confirmed.

**Iteration:** Test most likely hypothesis first. If ruled out, test next. If all ruled out, loop to Phase 3 for new hypotheses.

**Bounds:** Max 5 hypothesis iterations before escalating.

**Gate:** Root cause confirmed with evidence. User confirms before proceeding.

### Phase 5: Define Fix Scope

**Primitive:** `define` — See `primitives/define.md`

Define minimal fix scope — what should change and what must NOT change.

### Phase 6: Implement Fix

**Primitive:** `implement` — See `primitives/implement.md`

Apply minimal fix addressing root cause. Add regression test.

### Phase 7: Validate Fix

**Primitive:** `validate` — See `primitives/validate.md`

Confirm: failing test passes, all tests pass, fix addresses root cause.

**On failure:** Per `checklist_management` protocol:
1. Invoke `investigate` to determine why fix didn't work
2. Determine loop-back point (Phase 4 or Phase 6)
3. Add remediation tasks to checklist
4. Re-execute and re-validate

---

## Preconditions

**Must be provided:** Bug description (ask for reproduction steps if unclear)

**Self-satisfiable:** Project context, testing setup (read docs and code)

---

## Postconditions

**Success:** Bug fixed, root cause documented, regression test added, all tests pass.

**Failure:** Cannot reproduce, cannot identify root cause, fix introduces regressions, or user aborts.

---

## Recovery

Per `recovery` protocol — check for existing trace on startup, resume from last completed step.

**Step-specific notes:**
- Partial failing test → review and complete
- Hypothesis list exists → read and continue
- Instrumentation added → review findings before continuing
- Partial fix → review for correctness, complete or revise

---

## Iteration

Per `checklist_management` protocol:
- Max 5 hypothesis iterations before escalating
- Max 2 fix iterations before escalating
- If same failure recurs, escalate immediately

---

## Customization Points

**Prototype:** Manual reproduction acceptable, regression test recommended but optional.

**Production:** Automated regression test mandatory, root cause documentation required, code review for fix.

**Library:** Consider API compatibility, changelog entry for behavioral changes.
