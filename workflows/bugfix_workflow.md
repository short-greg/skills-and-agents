**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: bugfix-workflow
description: >
  Use when fixing bugs systematically with hypothesis-driven investigation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Bug is reproducible — failing test demonstrates the bug before fix
2. Root cause is identified — evidence confirms cause, not speculation
3. Fix is minimal — changes address root cause only, no unrelated modifications
4. Regression is prevented — new test catches this bug if reintroduced, all tests pass

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task (partial failing test → review and complete, hypothesis list exists → read and continue, instrumentation added → review findings, partial fix → review for correctness)
3. Reproduce before diagnosing (create failing test first)
4. Confirm root cause before implementing fix
5. Test hypotheses systematically (most likely first)
6. On validation failure, investigate before re-fixing
7. Per `risk_management.md` — identify root cause before fixing symptoms
8. Per `documentation.md` — update documentation if bug reveals incorrect docs
9. Iterate up to 5 times for hypothesis testing before escalating, max 2 fix iterations before escalating

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Bug description

**Elicit if not provided:**
- Reproduction steps (ask if unclear)
- Project context (read docs and code)
- Testing setup (from existing tests)

**Optional:** None

## Postconditions

The resulting state after the skill is finished.

**Success:** Bug fixed, root cause documented, regression test added, all tests pass.

**Failure:** Cannot reproduce, cannot identify root cause, fix introduces regressions, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason about bug
2. Understand context (orient)
3. Reproduce bug
4. Generate hypotheses
5. Investigate root cause
6. Define fix scope
7. Implement fix
8. Validate fix

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Bug (→ KR1, KR2)

Per `reasoning.md` — before beginning, reason about:

1. **Bug characteristics** — Clear reproduction? Intermittent? Environment-specific?
2. **Uncertainty level** — Obvious cause or deep investigation needed?
3. **Hypothesis count** — Low uncertainty: 2-3 hypotheses. High uncertainty: 5+
4. **Risk assessment** — Could fix introduce regressions? Critical system?

Output your reasoning.

### Understand Context (→ KR1)

Use `orient` primitive. Understand codebase, identify relevant code areas, testing conventions.

### Reproduce Bug (→ KR1)

Use `investigate` primitive. Create failing test that demonstrates the bug. This becomes the success criterion.

**On failure to reproduce:** Per `risk_management.md` — gather more information, check environment factors, consider intermittent issues.

### Generate Hypotheses (→ KR2)

Use `brainstorm` primitive. Generate potential root causes ranked by likelihood. Scale hypothesis count with uncertainty.

### Investigate Root Cause (→ KR2)

Use `investigate` primitive. Test hypotheses through instrumentation, logging, analysis until root cause is confirmed.

**Iteration:** Test most likely hypothesis first. If ruled out, test next. If all ruled out, generate new hypotheses.

**Bounds:** Max 5 hypothesis iterations before escalating.

**Gate:** Root cause confirmed with evidence. Ask user to confirm before proceeding.

### Define Fix Scope (→ KR3)

Use `define` primitive. Define minimal fix scope — what should change and what must NOT change.

### Implement Fix (→ KR3, KR4)

Use `implement` primitive. Apply minimal fix addressing root cause. Add regression test.

### Validate Fix (→ KR4)

Use `validate` primitive. Confirm: failing test passes, all tests pass, fix addresses root cause.

**On failure:**
- Invoke `investigate` to determine why fix didn't work
- Determine which task to return to
- Add remediation tasks to checklist
- Re-execute and re-validate

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand codebase, identify relevant code areas, testing conventions
- `investigate` — Create failing test, trace execution, test hypotheses
- `brainstorm` — Generate potential root causes ranked by likelihood
- `define` — Define minimal fix scope
- `implement` — Apply minimal fix, add regression test
- `validate` — Confirm fix works, all tests pass

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

**Customization Points:**

- **Prototype:** Manual reproduction acceptable, regression test recommended but optional
- **Production:** Automated regression test mandatory, root cause documentation required, code review for fix
- **Library:** Consider API compatibility, changelog entry for behavioral changes
