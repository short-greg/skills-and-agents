---
name: orient
description: >
  Use when you need to understand the current state of something before deciding what to do. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation", "how bad is it", "what do we have", "take stock", "size this up", "what's working", "get my bearings", "gap analysis", "assess the situation".
argument-hint: "[subject to orient on]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Orient

**Goal:** Produce accurate picture of current state so next steps can be decided with confidence.

**Intent:** Prevent acting on assumptions. Poor orientation leads to solving the wrong problem, missing real issues, or duplicating existing work.

**Scope:** Understanding current situation before taking action. Includes inventorying what exists (code, docs, infrastructure), evaluating what works vs broken, identifying gaps between current and target state, surfacing existing issues and technical debt, noting risks in current state. Orientation answers "where are we now?" not "where should we go?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Current state described accurately with gaps, issues, risks, and strengths identified
2. Orientation is actionable — reader knows what decisions need to be made

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Track what assessed vs what remains per `tracking.md`
2. Reason about orientation approach before starting, verify findings after per `reasoning.md`
3. No significant omissions — inventory comprehensively
4. Distinguish what works from what doesn't clearly

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Subject to orient on

**Elicit if not provided:**
- Current state (read relevant files, code, docs, outputs)
- Prior work (look for existing assessments, specs, decisions)

**Optional:** Target state (if known, enables gap identification), success criteria

## Postconditions

The resulting state after the primitive is finished.

**Success:** Current state documented accurately, gaps and issues surfaced, what works distinguished from what doesn't, orientation is actionable.

**Failure:** Cannot access subject (missing files, access denied), subject too large or ill-defined without scoping input.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Assess Current State (→ KR1)

Read materials (code, docs, specs, test results, logs) to understand what exists. Identify gaps (compare what exists against what's needed), identify issues (problems, bugs, inconsistencies, technical debt), identify risks (what could go wrong or is fragile), identify strengths (what's working well).

Categorize findings by severity, type, or area.

### Summarize and Flag Decisions (→ KR1, KR2)

Produce concise summary of current state with evidence. Flag decision points — identify what decisions need to be made based on findings.

---

## Additional Notes and Terms

**Use cases:** Before starting refactor, before planning feature, evaluating code quality or test coverage, understanding bug state before debugging, sizing work before committing to plan.
