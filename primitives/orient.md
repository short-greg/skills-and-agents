---
name: orient
description: >
  Use when you need to understand the current state of something before deciding what to do.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation",
  "how bad is it", "what do we have", "take stock", "size this up", "what's working",
  "get my bearings", "gap analysis", "assess the situation".
argument-hint: "[subject to orient on]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Orient

**Goal:** Produce an accurate picture of the current state so that next steps can be decided with confidence.

**Intent:** Prevent acting on assumptions. Poor orientation leads to solving the wrong problem, missing real issues, or duplicating existing work.

**Scope:** Understanding the current situation before taking action. Includes: inventorying what exists (code, docs, infrastructure), evaluating what is working vs. broken, identifying gaps between current state and any known target, surfacing existing issues and technical debt, and noting risks inherent in the current state. Orientation is about current state assessment — it answers "where are we now?" not "where should we go?"

---

## Key Results

1. Current state is described accurately — no significant omissions
2. Gaps between current state and known target (if any) are identified
3. Issues and risks in the current state are surfaced, not hidden
4. Strengths and what's working are noted
5. The orientation is actionable — reader knows what decisions need to be made

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track what has been assessed vs what remains
- `recovery.md` — Resume orientation from where it was interrupted
- `reasoning.md` — Reason about orientation approach before starting, verify findings after
- `goals_and_objectives.md` — Assess current state relative to target goals when identifying gaps

---

## Preconditions

**Must be provided:**
- subject to orient on: what is being evaluated — ask if not clear from context

**Self-satisfiable:**
- current state: read relevant files, code, docs, or outputs to understand what exists
- prior work: look for existing assessments, specs, or decisions that shaped current state

**Non-essential:**
- target state: if known, gaps between current and target can be identified
- success criteria: if known, orientation can measure against them

---

## Postconditions

**Success:**
- Current state is documented accurately
- Gaps between current state and target (if any) are identified
- Issues and risks are surfaced
- What works and what doesn't is clearly distinguished
- The orientation is actionable — it's clear what decisions need to be made

**Failure:**
- Cannot access the subject (missing files, access denied, unclear what to orient on)
- Subject is too large or ill-defined to orient on meaningfully without scoping input from user

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

- **scope the orientation**: clarify boundaries — what is and isn't being evaluated
- **read materials**: read code, docs, specs, test results, logs — primary way to understand current state
- **identify gaps**: compare what exists against what's needed or expected
- **identify issues**: surface problems, bugs, inconsistencies, or technical debt
- **identify risks**: note what could go wrong or is fragile
- **identify strengths**: note what's working well — avoids overcorrection
- **categorize findings**: group findings by severity, type, or area
- **summarize state**: produce a concise summary of current state with evidence
- **flag decision points**: identify what decisions need to be made based on findings

---

## Confirm

Before declaring done, verify against each key result:
- Is current state described accurately?
- Are gaps identified?
- Are issues and risks surfaced?
- Is the orientation actionable?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Use Cases

- Before starting a refactor to understand what exists
- Before planning a feature to understand current capabilities
- Evaluating code quality, test coverage, or documentation completeness
- Understanding the state of a bug or incident before debugging
- Sizing work before committing to a plan

---

## Tools

None

## Hooks

None
