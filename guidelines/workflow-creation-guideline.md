# Workflow Creation Guideline

**Inherits from [skill_creation_guideline.md](skill_creation_guideline.md).** Read base guideline first.

---

## Prerequisites

Stop if any are false:
- [ ] Task requires multiple steps in sequence
- [ ] Task requires 2+ actions (not just one)
- [ ] Read [skill_creation_guideline.md](skill_creation_guideline.md)

If task uses only one action → execute that action directly, do not create a workflow.

---

## Definition

A **workflow** orchestrates 2+ actions to achieve a multi-step goal. Actions reference protocols directly.

| Attribute | Requirement |
|-----------|-------------|
| Actions | 2+ |
| Key Results | 3-4 |
| Lines | 150-200 (200-250 with permission) |
| Recovery | Required |
| Progress tracking | Required |

---

## Steps to Create

1. **Elicit goal** — Ask user: "What is the goal of this workflow?"
2. **Confirm understanding** — Paraphrase back, get user confirmation
3. **Verify prerequisites** — Confirm task requires 2+ actions (stop if not)
4. **Research**:
   - Search: `[workflow topic] best practices`
   - Search: `[domain] workflow patterns`
5. **Create protocol decision table**:

   | Protocol | Relevant Actions | Reasons to use | Reasons not to use | Decision |
   |----------|------------------|----------------|-------------------|----------|
   | thinking | Generate alternatives, Evaluate tradeoffs | ... | ... | Use/Skip |
   | elicitation | Gather requirements | ... | ... | Use/Skip |

6. **Define actions** — For each action, specify all fields (see Action Template below)
7. **Write workflow** — Follow template exactly
8. **Validate** — Run validation criteria checklist
9. **Get confirmation** — User approves final workflow

---

## Action Template

Actions are units of work that apply protocol techniques to achieve specific outcomes. Each action must be sufficiently small: one clear goal, one protocol (or tightly related set), predictable inputs → outputs.

```markdown
### Action Name

Intent: Why this action exists
KR: Outcome-oriented success criteria (comma-separated)
Preconditions:
- Required: What must be provided
- Required (request): What to request if not provided
- Optional: What enhances the action (default: value)
Postconditions:
- Success: Output X with Y documented
- Failure: Output request listing missing information needed to succeed
Exit Conditions:
- Condition — stop, action to take

Instructions: (use for predictable, sequential work)
1. Read `protocol.md` and apply Action Name to achieve X
2. Apply Technique Name to do Y
3. Output Z for confirmation

— OR —

Objectives: (use for judgment-heavy, adaptive work)
- Achieve X before proceeding
- Minimize Y while ensuring Z

Constraints:
- Must read `protocol.md` before proceeding
- Must not exceed N iterations
```

### When to Use Instructions vs Objectives+Constraints

**Use Instructions (imperative) when:**
- Task is well-understood with predictable steps
- Consistency and reproducibility matter (compliance, auditing)
- Risk of deviation is high-cost (safety-critical)
- Executor is less experienced or context is new

**Use Objectives+Constraints (declarative) when:**
- Uncertainty is high — situation may evolve unpredictably
- Multiple valid paths to outcome exist
- Executor has expertise and judgment to adapt
- Speed and initiative matter more than uniformity

**Hybrid approach:** Use objectives to define *what* and *why*, constraints to define boundaries, instructions only for critical sequences where deviation is dangerous.

---

## Writing Rules

1. **Specify protocols in actions** — Each action references which protocol(s) it uses
2. **Include recovery** — Add progress tracking, recovery behavior, iteration limits
3. **Keep descriptions brief** — Goal/Intent/Scope: 2-3 sentences max
4. **Label actions with KRs** — Format: `### Action Name (→ KR#)`
5. **Declare workflow style** — State Imperative, Declarative, or Hybrid

**Workflow Styles:**

| Style | Checklist | Steps | When to use |
|-------|-----------|-------|-------------|
| **Imperative** | Optional, fixed | Fixed sequence, no KR labels | Predictable process |
| **Declarative** | Required, adapts | KR labels, add/remove actions | Adaptive process |
| **Hybrid** | Required at bookends | Imperative setup + validation, declarative middle | Most workflows |

---

## Validation Checklist

For each criterion: output evidence it passes, output evidence it fails, then decide.

**Structural:**
- [ ] **Follows template** — All sections present, correct format
- [ ] **150-200 lines** — (200-250 with permission)
- [ ] **3-4 Key Results** — Not 2, not 5+

**Content:**
- [ ] **Coherent** — No contradictions between sections
- [ ] **Complete** — All KRs achievable from Actions
- [ ] **Concise** — No duplication, minimum words
- [ ] **Precise** — Unambiguous language, no hedging

**Workflow-Specific:**
- [ ] **Uses 2+ actions** — Not single-action
- [ ] **Recovery included** — Progress tracking, recovery, iteration limit
- [ ] **Actions specify protocols** — Each action references protocol(s)
- [ ] **Actions are complete** — Each has Intent, KR, Pre/Postconditions, Exit Conditions, Instructions or Objectives+Constraints
- [ ] **Research completed** — References section has sources

---

## Rules

1. **Declare workflow style** — State Imperative, Declarative, or Hybrid at top
2. **Use Hybrid for most workflows** — Imperative bookends (setup + validation), declarative middle
3. **Map each action to protocols** — Reference specific protocols and techniques
4. **State preconditions/postconditions** — Required for each action
5. **Add recovery** — Progress tracking per `tracking_and_recovery.md`, iteration limit
6. **Limit to 3-4 Key Results** — Focused but comprehensive
7. **Handle failures** — State what happens when action fails (retry, escalate, etc.)
8. **Do not create single-action workflows** — Execute the action directly instead
9. **Actions must be sufficiently small** — One clear goal, predictable inputs → outputs

---

## Template

**Note:** Text in `[brackets]` are placeholders - replace with actual content. Text in `<angle brackets>` are guidelines - do NOT include in your final workflow.

```markdown
---
name: workflow-name
description: >
  [What this workflow does. Include trigger keywords and phrases users naturally say.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
argument-hint: "[optional: e.g. [feature] or [bug description]]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Workflow Name

**Goal:** [What this workflow achieves - one sentence.]

**Intent:** [Why this workflow exists - what problem it solves or prevents.]

**Scope:** [Complete process description. What this workflow covers from start to finish.]

**Workflow Style:** [Goal-Oriented (Declarative) / Process-Oriented (Imperative) / Hybrid]

---

## Key Results - KR

You must satisfy these to complete the workflow successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]
<4. [measurable outcome] — optional 4th KR>

## Requirements and Constraints - REQ

Constraints on how to complete the workflow.

1. Progress tracked per `tracking_and_recovery.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking_and_recovery.md` — check for existing trace on startup, resume from last completed action
3. [workflow-specific constraint]
4. [workflow-specific constraint]
5. Iterate up to N times if [condition] before escalating

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what must be provided]

**Optional:**
- [optional inputs that enhance the workflow]

## Postconditions

The resulting state after the workflow is finished.

**Success:** [state when workflow completes successfully]

**Failure:** [state when workflow cannot complete]

---

## Steps

Steps define execution order. Format differs by workflow style.

**Writing Rules for Steps:**
- Use imperative form or "You MUST" — never "should" or "could"
- Say "Read X to understand..." — never "uses X"
- Specify exact outputs — never vague "do X"

<**Imperative** — Fixed sequence, no KR labels needed, checklist optional:

```
1. Read CLAUDE.md to understand the project conventions
2. Execute Gather Requirements action. Output your understanding for confirmation
3. Execute Design Solution action. Output your proposed approach for approval
4. Execute Implement Changes action per approved design
5. Execute Validate Implementation action. Output evidence of success
```

**Declarative** — Preliminary checklist keeps it on track, but adapts (add/remove items) as plan evolves:

```
1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with formula: `[workflow] - KR# - <action> - <details>`
2. Execute Orient to Project action → KR1. Review all project documentation
3. Execute Gather Requirements action → KR1-2. Output your assessment, then ask questions
4. Output action plan → Based on findings, output proposed actions with rationale. Get approval before proceeding
5. [Execute discovered actions] → KR2-3. Placeholder — replace with specific actions discovered
6. Execute Validate KRs action → KR1-4. Output positive and negative evidence for each KR, then decide
```

**Hybrid** — Imperative bookends (setup + validation), declarative middle:

```
1. Create preliminary checklist. You MUST use TodoWrite with formula: `[workflow] - KR# - <action>`
2. Execute Orient and Investigate actions to understand context
3. Output action plan for approval
4. [Execute discovered actions — placeholder, replace with specific actions]
5. Execute Validate KRs action. Output positive and negative evidence for each, then decide
```

>

---

## Actions

Actions are units of work that apply protocol techniques to achieve specific outcomes. Select actions based on context. Each action shows which KR it serves.

### [Action Name] (→ KR#)

Intent: [Why this action exists]
KR: [Success criteria for this action]
Preconditions:
- Required: [what must be provided]
- Required (request): [what to request if not provided]
- Optional: [what enhances the action] (default: [value])
Postconditions:
- Success: [output produced]
- Failure: [what to output on failure]
Exit Conditions:
- [condition] — stop, [action to take]

Instructions:
1. Read `[protocol].md` and apply [Technique Name] to [purpose]
2. [Next step]
3. Output [result] for [purpose]

— OR —

Objectives:
- [What to achieve]
- [What to minimize/maximize]

Constraints:
- Must read `[protocol].md` before proceeding
- [Other constraints]

---

## Additional Notes and Terms

<Domain-specific terms, customization points, or other notes. Write "None" if not applicable.>

---

## References

<REQUIRED: Research must be completed before creating a workflow.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [workflow topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[workflow topic] process best practices" or "[topic] workflow patterns"
- Example: Creating feature workflow → search "feature development workflow best practices"
- Example: Creating bugfix workflow → search "debugging process software engineering">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
