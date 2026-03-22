# Skill Builder Template

Template for skills created by the skill-builder.

**Notation:**
- `{PLACEHOLDER}` — Configurable values. Replace based on developer preferences.
- `<guidelines>` — Guidance for the skill-builder. Do not include in output.

---

## Consultant Behavior (When Creating Skills)

When creating a skill, the skill-builder must act as a consultant:

**Reason Out Loud:**
- "To create a good [skill type] skill, I need to understand..."
- "Based on the project type, I should consider..."
- "I found [X] in the codebase, which suggests..."

**Assume and Explain Role:**
- After understanding what skill is needed, state your role
- Example: "For this deployment skill, I'll think as a DevOps engineer focused on reliability and rollback safety"
- Explain what this role means for the skill design

**Share Findings Before Asking:**
- Discover what exists (patterns, conventions, similar skills)
- Output what you found and what it implies
- Then ask targeted questions about gaps

**Recommend with Rationale:**
- Don't just ask "what tools do you want?"
- Instead: "This skill will modify files and run tests, so I recommend Edit, Bash, and TodoWrite for tracking. Does that sound right?"

**Research Before Recommending:**
- Use WebSearch to find best practices for the skill's domain
- Search: "[skill topic] best practices", "[domain] workflow patterns"
- Incorporate findings into skill design

---

## When to Create a Skill

<Before creating a skill, verify it's appropriate:>

<Create a skill when:>
<- Task requires 2+ actions in sequence>
<- Task will be repeated (not one-off)>
<- Task benefits from tracking/recovery>
<- Task needs consistent execution>

<Do NOT create a skill when:>
<- Task is single action — execute the action directly>
<- Task is one-off with no reuse value>
<- Task is purely exploratory/research>

---

## Skill Definition

<Minimum requirements for a well-formed skill:>

<| Attribute | Requirement |>
<|-----------|-------------|>
<| Actions | 2+ |>
<| Key Results | As few as needed to be complete |>
<| Recovery | Required for multi-step skills |>
<| Progress tracking | Required (TodoWrite) |>
<| Line count | Target 150-200 lines (200-250 with good reason) |>

---

## Available Claude Code Tools

When creating skills, recommend appropriate tools. Reason out loud about why each is needed:

| Tool | Purpose | Permission | Recommend When |
|------|---------|------------|----------------|
| Read | Read files | No | All skills need this |
| Write | Create/overwrite files | Yes | Skills that create artifacts |
| Edit | Targeted file edits | Yes | Skills that modify existing files |
| Glob | Find files by pattern | No | Search, discovery, refactoring |
| Grep | Search file contents | No | Code search, pattern finding |
| Bash | Shell commands | Yes | Git, builds, tests, CLI tools |
| NotebookEdit | Jupyter cell editing | Yes | Data science, notebook skills |
| TodoWrite | Progress tracking | No | Multi-step skills (non-interactive) |
| Task* | Task management | No | Interactive sessions, background work |
| WebSearch | Web search | Yes | Skills needing current info |
| WebFetch | Fetch URL content | Yes | Documentation, API research |
| AskUserQuestion | User input | No | Consultative, decision-making |
| Agent | Spawn subagents | No | Complex parallel work |
| LSP | Code intelligence | No | Type checking, navigation |
| EnterPlanMode | Design before coding | No | Complex implementation skills |

**Tool Selection Reasoning:**
- "This skill creates files, so it needs Write"
- "This skill searches code, so Glob and Grep"
- "This skill needs user decisions, so AskUserQuestion"
- "This is a multi-step skill, so TodoWrite for tracking"

---

## MCP and Hooks Discovery

Before finalizing a skill, check what's available and recommend additions:

**Discovery:**
1. Check existing configs: `~/.claude/settings.json`, `.claude/settings.json`
2. Check existing hooks: `~/.claude/hooks/`, `.claude/hooks/`
3. Research: Use WebSearch to find MCPs relevant to skill's domain

**Reasoning:**
- "For a [skill purpose] skill, these MCPs would help because..."
- "I found an existing [X] hook that this skill should use"
- "This skill would benefit from a [Y] MCP for [reason]"

**Common MCP categories:**
- Database MCPs (for data access skills)
- API MCPs (for integration skills)
- File system MCPs (for specialized file handling)
- Communication MCPs (Slack, email, etc.)

---

## Consultation Approach by Interaction Mode

| Mode | Approach |
|------|----------|
| Lead | Output proposal with rationale, confirm only essential points, proceed efficiently |
| Senior | Share intermediate findings, recommend approach, get approval at gates |
| Peer | Brainstorm options together, collaborate on design, discuss trade-offs |
| Junior | Present all options with trade-offs, await direction at each decision |

---

## Skill Analysis Questions

Use these to understand intent. Reason out loud about which questions matter for this skill.

**Understanding the Problem:**
- What problem does this skill solve?
- Why is this skill necessary? (vs manual, vs existing skill)
- Who will use this skill? What triggers it?

**Defining Scope:**
- What are the deliverables?
- Should this be one skill or multiple?
- Does it compose with other skills?

**Determining Role:**
- What domain role? (software engineer, designer, analyst, reviewer)
- What behavioral stance? (executor, advisor, pair programmer)

**Deciding Behavior:**
- How autonomous? (proceed vs confirm at each step)
- How handle errors? (fail, retry, ask, rollback)

---

## Variation Dimensions

| Dimension | Options | Determined by |
|-----------|---------|---------------|
| Workflow Style | Declarative, Imperative, Hybrid | Developer preference |
| Action Style | Instructions, Objectives+Constraints | Per-action need |
| Role | Domain + Behavioral | Skill intent |
| Autonomy | Proceed, confirm at gates, confirm each step | Interaction Mode |
| Artifacts | SKILL.md only, +references, +code, +hooks | Skill needs |

---

## Skill Validation Checklist

**Structure:**
- [ ] All sections present (Goal, Intent, Scope, Role, KRs, REQs, Pre/Postconditions, Steps, Actions, Notes, Risks, References)
- [ ] Key Results — as few as needed to be complete
- [ ] 2+ Actions with complete structure
- [ ] Target 150-200 lines (200-250 with good reason)

**Content:**
- [ ] Coherent — no contradictions between sections
- [ ] Complete — all KRs achievable from Actions
- [ ] Concise — no duplication, minimum words
- [ ] Precise — unambiguous language, no hedging
- [ ] Referenced protocols exist in `protocols/`
- [ ] Tools in allowed-tools are available

**Quality:**
- [ ] Recovery included — progress tracking, recovery from interruption (if multi-step)
- [ ] Research completed — References section has sources consulted
- [ ] Actions are small — one clear goal each, predictable inputs → outputs

---

## Recommended Initial Skills

When setting up a project, recommend creating a `/consult` skill for flexible consultative tasks.

See [consult-template.md](consult-template.md) for the full template.

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
- Validate against Skill Validation Checklist (above)
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
