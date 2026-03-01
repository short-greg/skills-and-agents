---
name: orienting
description: >
  Understanding the current state of a system, project, or task. Understands current state before deciding what to do.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation",
  "take stock", "size this up", "gap analysis", "assess the situation".
  keywords: mapping, inventorying, locating, surveying, assessing
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

- **discipline.md** — Must use for Map Structure, Inventory Assets, Identify Gaps and Issues (systematic enumeration, no omissions)
- **thinking.md** — Must use for Identify Gaps and Issues, Assess Risks and Strengths (analytical reasoning)
- **risk_management.md** — Must use for Assess Risks and Strengths (risk assessment)
- **interviewing.md** — Use for Locate Information (when user has context not in artifacts)
- **pragmatics.md** — Use for Summarize State and Flag Decisions (framing findings)
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

Inherits from `base.md` — output lightweight checklist, resolve preconditions, plan actions, execute, report result.

---

## Terms

**Orienting Target:** What is being oriented on (system, project, task, codebase, infrastructure, process, etc.)

**Orienting Method:** How to orient (mapping structure, inventorying available assets, locating relevant information, assessing state against criteria, etc.)

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

Select actions based on context. Each action shows which KR it serves.

### Map Structure (→ KR1)

Execute structure mapping using `discipline.md` (MECE Enumeration) when understanding layout or topology of orienting target. Systematically enumerate ALL components: directory structure, component relationships, module dependencies, data flows, or process sequences. Document hierarchy, connections, boundaries. Create representation showing how parts relate to whole.

### Inventory Assets (→ KR1)

Execute asset inventory using `discipline.md` (Coverage Tracking) when cataloging existing resources. Systematically list ALL files, components, modules, dependencies, configurations, documentation, tests, or infrastructure. Categorize by type, purpose, or area. Track coverage to ensure no significant omissions. Note what exists, what's complete, what's partial, what's missing.

### Locate Information (→ KR1)

Execute information location using `interviewing.md` (Open Questions, Inference First) when specific information is needed for orientation. Search for documentation, implementation details, configuration, test coverage, known issues, or prior decisions. Apply interviewing when user has context not available in artifacts. Document where critical information resides.

### Identify Gaps and Issues (→ KR1)

Execute gap and issue identification using `thinking.md` (Analytical) and `discipline.md` (MECE Enumeration) when target state is known or standards exist. Systematically enumerate ALL gaps: missing components, incomplete implementations, broken functionality, inconsistencies, technical debt, deviations from requirements. Distinguish what's present from what's needed. Note severity and impact of each.

### Assess Risks and Strengths (→ KR1)

Execute risk and strength assessment using `thinking.md` (Analytical) and `risk_management.md` (Risk Assessment) when understanding stability and reliability. Identify risks (fragile code, lack of tests, tight coupling, security vulnerabilities, performance bottlenecks). Identify strengths (well-tested code, clear documentation, good separation). Categorize by likelihood and impact.

### Summarize State and Flag Decisions (→ KR1, KR2)

Execute summary creation using `pragmatics.md` (Directness Calibration) when orientation is complete. Produce concise summary of current state with supporting evidence. Group findings by category or area. Flag decision points — identify what decisions need to be made based on findings (what to fix first, what approach to take, what to investigate further). Frame findings appropriately for audience.

---

## Additional Notes

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Farnam Street - The OODA Loop](https://fs.blog/ooda-loop/)
- [Corporate Finance Institute - OODA Loop Guide](https://corporatefinanceinstitute.com/resources/management/ooda-loop/)
