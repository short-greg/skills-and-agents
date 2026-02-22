---
name: bugfix-workflow
description: >
  Use when fixing bugs systematically with hypothesis-driven investigation.
  Triggers on: "fix this bug", "debug this issue", "systematic bugfix".
argument-hint: "[bug description or reproduction steps]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Bugfix Workflow

**Goal:** Fix bugs by identifying root cause through hypothesis-driven investigation, implementing minimal fix, and preventing regression.

**Intent:** Prevent haphazard debugging where developers fix symptoms rather than causes and introduce regressions. Ensure root causes are identified through systematic investigation and minimal fixes are applied with regression tests.

**Scope:** Complete bug investigation and fix: reproduction, hypothesis generation, root cause confirmation, minimal fix, regression prevention.

---

## Key Results

1. Uncertainties about the bug are resolved through investigation before fixing
2. Bug is reproducible — failing test demonstrates the bug before fix
3. Root cause is identified — evidence confirms cause, not speculation
4. Fix is minimal — changes address root cause only, no unrelated modifications
5. Regression is prevented — new test catches this bug if reintroduced
6. All tests pass — existing test suite passes with fix applied
7. Documentation is updated if fix changes behavior or reveals missing docs

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress through investigate → hypothesize → fix → validate phases
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist after reasoning about the bug. Update it dynamically.
- `reasoning.md` — Form hypotheses before investigating. Verify fix after.
- `goals_and_objectives.md` — Define what "fixed" means with binary pass/fail criteria.
- `risk_management.md` — Identify root cause before fixing symptoms.
- `documentation.md` — Update documentation if bug reveals incorrect docs.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to fix the bug. If you do not understand a primitive, read it before using it.

- `orient` — Understand codebase, identify relevant code areas, testing conventions.
- `investigate` — Create failing test, trace execution, test hypotheses.
- `brainstorm` — Generate potential root causes ranked by likelihood.
- `define` — Define minimal fix scope.
- `implement` — Apply minimal fix, add regression test.
- `validate` — Confirm fix works, all tests pass.

---

## Constraints

- Reproduce before diagnosing (create failing test first)
- Confirm root cause before implementing fix
- Test hypotheses systematically (most likely first)
- Fix must be minimal (no "while I'm here" changes)
- Must add regression test
- On validation failure, investigate before re-fixing

---

## Tasks

Execute these tasks to achieve the key results. Select and sequence based on your reasoning about the bug.

### Reason About Bug

Per `reasoning.md` — before beginning, reason about:

1. **Bug characteristics** — Clear reproduction? Intermittent? Environment-specific?
2. **Uncertainty level** — Obvious cause or deep investigation needed?
3. **Hypothesis count** — Low uncertainty: 2-3 hypotheses. High uncertainty: 5+
4. **Risk assessment** — Could fix introduce regressions? Critical system?

Output your reasoning.

### Understand Context

Use `orient` primitive. Understand codebase, identify relevant code areas, testing conventions.

### Reproduce Bug

Use `investigate` primitive. Create failing test that demonstrates the bug. This becomes the success criterion.

**On failure to reproduce:** Per `risk_management.md` — gather more information, check environment factors, consider intermittent issues.

### Generate Hypotheses

Use `brainstorm` primitive. Generate potential root causes ranked by likelihood. Scale hypothesis count with uncertainty.

### Investigate Root Cause

Use `investigate` primitive. Test hypotheses through instrumentation, logging, analysis until root cause is confirmed.

**Iteration:** Test most likely hypothesis first. If ruled out, test next. If all ruled out, generate new hypotheses.

**Bounds:** Max 5 hypothesis iterations before escalating.

**Gate:** Root cause confirmed with evidence. Ask user to confirm before proceeding.

### Define Fix Scope

Use `define` primitive. Define minimal fix scope — what should change and what must NOT change.

### Implement Fix

Use `implement` primitive. Apply minimal fix addressing root cause. Add regression test.

### Validate Fix

Use `validate` primitive. Confirm: failing test passes, all tests pass, fix addresses root cause.

**On failure:**
- Invoke `investigate` to determine why fix didn't work
- Determine which task to return to
- Add remediation tasks to checklist
- Re-execute and re-validate

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist after reasoning about the bug, based on what you learned
- Mark items complete immediately after finishing each task
- On hypothesis failure, add sub-items for each hypothesis tested
- On validation failure, add remediation items
- Report progress after each completed item

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

Per `recovery.md` — check for existing trace on startup, resume from last completed task.

**Task-specific notes:**
- Partial failing test → review and complete
- Hypothesis list exists → read and continue
- Instrumentation added → review findings before continuing
- Partial fix → review for correctness, complete or revise

---

## Iteration

Per `checklists.md`:
- Max 5 hypothesis iterations before escalating
- Max 2 fix iterations before escalating
- If same failure recurs, escalate immediately

---

## Customization Points

**Prototype:** Manual reproduction acceptable, regression test recommended but optional.

**Production:** Automated regression test mandatory, root cause documentation required, code review for fix.

**Library:** Consider API compatibility, changelog entry for behavioral changes.
