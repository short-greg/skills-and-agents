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

<Steps must make the action that is executed clear. Except for creating the preliminary checklist, each step should have an action associated (e.g., "Execute A1") unless the step is very simple.>

<DECLARATIVE PATTERN — Use when developer prefers goal-oriented, adaptive approach:>
1. Create preliminary checklist → KR1-N. Use TodoWrite with formula: `{skill-name} - KR# - <action>`
2. {Preliminary steps — define in skill, e.g., planning} → KR#. Execute A#
3. {Execute actions — filled in at runtime to achieve KRs} → KR?
4. Validate KRs → Output positive and negative evidence for each. Execute A#

<IMPERATIVE PATTERN — Use when developer prefers fixed steps, reproducibility:>
1. {First step} → Execute A1
2. {Second step} → Execute A2
3. {Third step} → Execute A3
4. Validate all outputs → Execute A4

<HYBRID PATTERN — Use when need consistency at boundaries, flexibility in middle:>
1. Create preliminary checklist → KR1-N. Use TodoWrite
2. {Fixed boundary step} → Execute A1
3. {Adaptive middle — filled in at runtime} → KR?
4. Validate → Execute final validation action

---

## Action Field Definitions

| Field | Purpose |
|-------|---------|
| **Intent** | Why this action exists. The purpose it serves. |
| **KR** | Success criteria for this action. How to know it succeeded. |
| **Preconditions** | What must exist before this action runs. Required = must have. Optional = enhances if present. |
| **Postconditions** | What state results from this action. Success = normal outcome. Failure = fallback outcome. |
| **Exit Conditions** | When to stop early. Format: `{condition} → stop, {what to do}` |
| **Validation** | How to verify objectives were met. Used with Objectives+Constraints style. |

**Action Body Styles (choose one):**

| Style | When to Use | Contains |
|-------|-------------|----------|
| **Instructions** | Predictable, sequential work | Numbered steps to follow |
| **Objectives + Constraints** | Judgment-heavy, adaptive work | Outcomes + rules |

**Objectives vs Constraints:**

| | Objectives | Constraints |
|---|------------|-------------|
| **What** | Outcomes to achieve | Rules to follow |
| **Focus** | What success looks like | How to behave |
| **Format** | Statements of outcomes | "You MUST" / "You MUST NOT" |
| **Example** | "Design proposal approved" | "You MUST propose with rationale" |

---

## Writing Rules

<Guidelines for writing clear, maintainable skills:>

<**Descriptions:**>
<- Goal/Intent/Scope: 2-3 sentences max>
<- Keep descriptions brief — say what's needed, no more>

<**Actions:**>
<- Actions must be sufficiently small: one clear goal, predictable inputs → outputs>
<- Use output-oriented verbs (list, evaluate, derive, document, output) not internal verbs (find, check, look)>
<- Each action references which protocol(s) it uses>

<**Language:**>
<- Use imperative form or "You MUST" — never "should" or "could">
<- Say "Read X to understand..." — never "uses X">
<- Specify exact outputs — never vague "do X">

---

## Error Handling

<Ask about error handling preferences during skill creation. Options:>

<| Strategy | When to Use |>
<|----------|-------------|>
<| Fail fast | Critical operations where partial completion is dangerous |>
<| Retry | Transient failures (network, rate limits) |>
<| Ask user | Ambiguous situations requiring judgment |>
<| Rollback | Multi-step operations that can be reversed |>

<For each skill, determine:>
<- How should this skill handle errors?>
<- Should it have rollback capability?>
<- How should it handle ambiguity? (always ask, assume with documentation)>

<Bake the chosen strategy into the skill's Exit Conditions and Postconditions.>

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

<OBJECTIVES+CONSTRAINTS STYLE — Use for judgment-heavy, adaptive work. See Action Field Definitions above.>

Objectives:
1. {OUTCOME_TO_ACHIEVE}
2. {ANOTHER_OUTCOME}

Constraints:
1. You MUST {REQUIRED_BEHAVIOR}
2. You MUST NOT {PROHIBITED_BEHAVIOR}
3. Read `{PROTOCOL}.md` and apply {TECHNIQUE}

Validation:
1. Add objectives to TodoWrite checklist
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
