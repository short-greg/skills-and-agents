---
name: orienting
description: >
  Understanding the current state of a system, project, or task. Understands current state before deciding what to do.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation",
  "take stock", "size this up", "gap analysis", "assess the situation".
argument-hint: "[subject to orient on]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Orienting

**Goal:** Produce accurate picture of current state so next steps can be decided with confidence.

**Intent:** Prevent acting on assumptions. Poor orientation leads to solving the wrong problem, missing real issues, or duplicating existing work.

**Scope:** Understanding current situation before taking action. Includes inventorying what exists (code, docs, infrastructure), evaluating what works vs broken, identifying gaps between current and target state, surfacing existing issues and technical debt, noting risks in current state. Orientation answers "where are we now?" not "where should we go?"

---

## Key Results - KR

1. Current state described accurately — gaps, issues, risks, and strengths identified
2. Orientation is actionable — reader knows what decisions need to be made

## Requirements and Constraints - REQ

1. No significant omissions — inventory comprehensively
2. Distinguish what works from what doesn't clearly
3. Be aware of cognitive biases that might affect assessment

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to reason about approach and verify findings
- **checklists.md** - Must use to ensure comprehensive coverage
- **tracking.md** - Use when tracking what assessed vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Subject to orient on

**Elicit if not provided:**
- Current state (read relevant files, code, docs, outputs)
- Prior work (look for existing assessments, specs, decisions)

**Optional:** Target state (if known, enables gap identification), success criteria

## Postconditions

**Success:** Current state documented accurately, gaps and issues surfaced, orientation is actionable

**Failure:** Cannot access subject, subject too large or ill-defined

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

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Farnam Street - The OODA Loop](https://fs.blog/ooda-loop/)
- [Corporate Finance Institute - OODA Loop Guide](https://corporatefinanceinstitute.com/resources/management/ooda-loop/)
