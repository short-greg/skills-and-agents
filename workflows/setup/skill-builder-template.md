# Skill Builder Template

Template for skills created by the skill-builder. See setup-skill-builder.md for consultation guidance, analysis questions, variation dimensions, and validation checklist.

**Notation:**
- `{PLACEHOLDER}` — Configurable values. Replace based on developer preferences.
- `<guidelines>` — Guidance for the skill-builder. Do not include in output.

---

## Risk Analysis (Before Creating Skill)

Before finalizing any skill, the skill-builder must perform risk analysis and consult the developer:

**Identify risks in these categories:**
1. **Scope risks** — Is the skill too broad? Too narrow? Overlapping with existing skills?
2. **Execution risks** — What could go wrong during skill execution? Data loss? Incorrect outputs?
3. **Integration risks** — How might this skill conflict with other skills or project conventions?
4. **Autonomy risks** — Is the skill too autonomous for its impact? Should it confirm more?
5. **Artifact risks** — Could generated files overwrite important content? Create clutter?

**Output risk analysis:**
- List each identified risk with severity (high/medium/low)
- Propose mitigations for high/medium risks
- Ask developer to accept, modify, or add mitigations
- Incorporate accepted mitigations into skill's Risks section

---

## Design Proposal (Before Creating Draft)

Before creating a full skill draft, propose the high-level design for approval:

**Output design proposal:**
1. Goal, Intent, Scope, Role — one sentence each
2. Proposed KRs (3-4) — measurable outcomes
3. Proposed Actions — outline with names and one-line purposes
4. Workflow style recommendation — with rationale for why this style fits
5. Anticipated risks — high-level, to be detailed later

**Get approval before proceeding to draft.** This prevents wasted effort on wrong direction.

---

## Iterative Improvement

Skills rarely perfect on first draft. The skill-builder should:

**After creating initial draft:**
1. Output the draft skill for developer review
2. Ask: "What would you change about this skill?"
3. Iterate based on feedback (up to 3 iterations before escalating)

**During iteration, check:**
- Does the skill match the developer's mental model?
- Are the actions at the right granularity?
- Is the workflow style appropriate for how the developer works?
- Are risks adequately addressed?

**After final approval:**
- Validate against checklist (see setup-skill-builder.md)
- Output evidence for each validation criterion

---

## Skill Template

```markdown
---
name: {SKILL_NAME}
description: >
  {SKILL_DESCRIPTION}
  Triggers on: "{TRIGGER_1}", "{TRIGGER_2}".
argument-hint: "{ARGUMENT_HINT}"
user-invocable: true
allowed-tools: {TOOLS}
---

# {SKILL_TITLE}

**Goal:** {GOAL}
<One sentence. What this skill achieves.>

**Intent:** {INTENT}
<Why this skill exists — what problem it solves or prevents.>

**Scope:** {SCOPE}
<What this skill covers. What it does NOT cover.>

**Role:** {ROLE}
<Domain role (software engineer, UI designer, data analyst, reviewer, etc.) + Behavioral stance (executor, advisor, pair programmer, auditor).>

**Workflow Style:** {WORKFLOW_STYLE}
<Declarative (goal-oriented), Imperative (process-oriented), or Hybrid.>

---

## Key Results - KR

<Appropriate number of measurable outcomes. Evaluate how many are absolutely necessary by finding evidence each is needed or not needed.>

1. {KR1}
2. {KR2}
3. {KR3}
4. {KR4}

## Requirements and Constraints - REQ

<Include based on developer preferences from setup-skill-builder interview.>

1. {IF_TRACKING: Progress tracked per `tracking_and_recovery.md`}
2. {IF_RECOVERY: Recoverable from interruption}
3. {IF_CLAUDE_READ: Read CLAUDE.md first}
4. {IF_VALIDATION: Validate with positive AND negative evidence}
5. {SKILL_SPECIFIC_CONSTRAINT}
6. Iterate up to {N} times before escalating

---

## Preconditions

**Required:** {REQUIRED_INPUTS}

**Optional:**
- {OPTIONAL_INPUT}

## Postconditions

**Success:** {SUCCESS_STATE}

**Failure:** {FAILURE_STATE}

---

## Steps

<Choose ONE pattern based on developer's workflow style preference.>

<DECLARATIVE PATTERN — Use when developer prefers goal-oriented, adaptive approach:>
1. Create preliminary checklist → KR1-N. Use TodoWrite with formula: `{skill-name} - KR# - <action>`
2. Plan execution → Based on context, output proposed actions for approval
3. [Execute discovered actions] → Placeholder, replace with specific actions
4. Validate KRs → Output positive and negative evidence for each

<IMPERATIVE PATTERN — Use when developer prefers fixed steps, reproducibility:>
1. Execute A1 → {first action}
2. Execute A2 → {second action}
3. Execute A3 → {third action}
4. Execute A4 → Validate all outputs

<HYBRID PATTERN — Use when need consistency at boundaries, flexibility in middle:>
1. Create preliminary checklist → KR1-N. Use TodoWrite
2. Read context → Execute A1
3. Plan and execute → Adapt based on findings
4. Validate → Execute final validation action

---

## Actions

<Each action must have: Intent, KR, Preconditions, Postconditions, Exit Conditions, and either Instructions OR (Objectives + Constraints + Validation).>

### A1: {ACTION_NAME} (→ KR#)

Intent: {WHY_THIS_ACTION_EXISTS}
KR: {SUCCESS_CRITERIA_FOR_THIS_ACTION}
Preconditions:
- Required: {WHAT_MUST_BE_PROVIDED}
- Required (request): {WHAT_TO_REQUEST_IF_NOT_PROVIDED}
- Optional: {WHAT_ENHANCES} (default: {VALUE})
Postconditions:
- Success: Output {RESULT} with {DETAILS} documented
- Failure: Output request listing {MISSING_INFO}
Exit Conditions:
- {CONDITION} — stop, {ACTION_TO_TAKE}

<INSTRUCTIONS STYLE — Use for predictable, sequential work:>

Instructions:
1. Read `{PROTOCOL}.md` and apply {TECHNIQUE} — to {PURPOSE}
2. {NEXT_STEP}
3. Output {RESULT} for {PURPOSE}

<OBJECTIVES+CONSTRAINTS STYLE — Use for judgment-heavy, adaptive work:>

Objectives (OBJ):
1. {WHAT_TO_ACHIEVE}
2. {WHAT_TO_MINIMIZE_OR_MAXIMIZE}

Constraints (CONST):
1. Read `{PROTOCOL}.md` and apply {TECHNIQUE} — to {PURPOSE}
2. {OTHER_CONSTRAINT}

Validation:
1. Add OBJ1-N to TodoWrite checklist
2. Output validation: list each objective with evidence

---

## Additional Notes and Terms

{SKILL_SPECIFIC_NOTES}

<Include domain-specific terms, customization points, or notes relevant to this skill. Write "None" if not applicable.>

---

## Risks (RISK)

<Skill-specific risks identified during risk analysis. Include risks from these categories:
- Execution risks: What could go wrong when the skill runs?
- Data risks: Could the skill damage or lose data?
- Integration risks: How might this skill conflict with other parts of the project?
- Autonomy risks: Is the skill taking actions without appropriate confirmation?
Each risk should have a concrete mitigation that the skill will follow.>

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | {RISK} | {WHEN} | {MITIGATION} |

---

## References

<Required. Include sources consulted during skill creation.>

- {REFERENCES}
- Project CLAUDE.md
```
