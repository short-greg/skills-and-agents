# Workflow Creation Guideline

**Inherits from [skill_creation_guideline.md](skill_creation_guideline.md).** Read base guideline first.

---

## Prerequisites

Stop if any are false:
- [ ] Task requires multiple steps in sequence
- [ ] Task orchestrates 2+ modes (not just one)
- [ ] Read [skill_creation_guideline.md](skill_creation_guideline.md)

If task uses only one mode → use that mode directly, do not create a workflow.

---

## Definition

A **workflow** orchestrates 2+ modes to achieve a multi-step goal.

| Attribute | Requirement |
|-----------|-------------|
| Modes | 2+ |
| Key Results | 3-4 |
| Lines | 150-200 (200-250 with permission) |
| Recovery | Required |
| Progress tracking | Required |

---

## Steps to Create

1. **Elicit goal** — Ask user: "What is the goal of this workflow?"
2. **Confirm understanding** — Paraphrase back, get user confirmation
3. **Verify prerequisites** — Confirm task requires 2+ modes (stop if not)
4. **Research**:
   - Search: `[workflow topic] best practices`
   - Search: `[domain] workflow patterns`
5. **Create decision tables**:

   | Mode | Reasons to use | Reasons not to use | Decision |
   |------|----------------|-------------------|----------|
   | orienting | ... | ... | Use/Skip |

6. **Write workflow** — Follow template exactly
7. **Validate** — Run validation criteria checklist
8. **Get confirmation** — User approves final workflow

---

## Writing Rules

1. **Specify modes in tasks** — Each task states which mode(s) it uses
2. **Include recovery** — Add progress tracking, recovery behavior, iteration limits
3. **Keep descriptions brief** — Goal/Intent/Scope: 2-3 sentences max
4. **Label tasks with KRs** — Format: `### Task Name (→ KR#)`
5. **Declare workflow style** — State Imperative, Declarative, or Hybrid

**Workflow Styles:**

| Style | Checklist | Steps | When to use |
|-------|-----------|-------|-------------|
| **Imperative** | Optional, fixed | Fixed sequence, no KR labels | Predictable process |
| **Declarative** | Required, adapts | KR labels, add/remove tasks | Adaptive process |
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
- [ ] **Complete** — All KRs achievable from Tasks
- [ ] **Concise** — No duplication, minimum words
- [ ] **Precise** — Unambiguous language, no hedging

**Workflow-Specific:**
- [ ] **Uses 2+ modes** — Not single-mode
- [ ] **Recovery included** — Progress tracking, recovery, iteration limit
- [ ] **Tasks specify modes** — Each task states which mode(s)
- [ ] **Research completed** — References section has sources

---

## Rules

1. **Declare workflow style** — State Imperative, Declarative, or Hybrid at top
2. **Use Hybrid for most workflows** — Imperative bookends (setup + validation), declarative middle
3. **Map each step to one mode** — Orient → Define → Design → Implement → Validate
4. **Orchestrate modes or workflows** — Tasks can use either
5. **State preconditions/postconditions** — Required for declarative tasks
6. **Add recovery** — Progress tracking per `tracking.md`, recovery per `recovery.md`, iteration limit
7. **Limit to 3-4 Key Results** — More than modes, but not excessive
8. **Handle failures** — State what happens when task fails (retry, escalate, etc.)
9. **Do not create single-mode workflows** — Use the mode directly instead

---

## Template

**Note:** Text in `[brackets]` are placeholders - replace with actual content. Text in `<angle brackets>` are guidelines - do NOT include in your final workflow. Instructions that go in the final template, are not included in either.

```markdown
---
name: workflow-name
description: >
  [What this workflow does. Include trigger keywords and phrases users naturally say.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

You must satisfy these to complete the skill successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]
<4. [measurable outcome] — optional 4th KR>

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task
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

The resulting state after the skill is finished.

**Success:** [state when workflow completes successfully]

**Failure:** [state when workflow cannot complete]

---

## Steps

Steps define execution order. Format differs by workflow style.

**Writing Rules for Steps:**
- Use imperative form or "You MUST" — never "should" or "could"
- Say "Read X to understand..." — never "uses X"
- Specify exact outputs — never vague "do X"
- Include framing output (Position Statement) early
- Include action plan output after investigation/interview

<**Imperative** — Fixed sequence, no KR labels needed, checklist optional:

```
1. Read CLAUDE.md to understand the project conventions
2. Interview the user to gather requirements. Output your understanding for confirmation
3. Design the solution. Output your proposed approach for approval
4. Implement the changes per approved design
5. Validate the implementation. Output evidence of success
```

**Declarative** — Preliminary checklist keeps it on track, but adapts (add/remove items) as plan evolves:

```
1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with this formula: `[workflow] - KR# - <task> - <details>`
2. Output Position Statement → How you will approach this task as an expert (see identity_and_profile.md)
3. Orient to project → KR1. Read `orienting.md` to understand how to orient. Review all project documentation
4. Investigate and interview → KR1-2. Read `interviewing.md`. Output your assessment, then ask questions arising from that assessment
5. Output action plan → Based on findings, output proposed tasks with rationale. Get approval before proceeding
6. [Execute discovered tasks] → KR2-3. This is a placeholder. Replace with specific tasks discovered during interview
7. Validate all KRs → KR1-4. Read `evaluating.md`. Output both positive and negative evidence for each KR, then decide if it passes
```

**Hybrid** — Imperative bookends (setup + validation), declarative middle:

```
1. Create preliminary checklist. You MUST use TodoWrite with formula: `[workflow] - KR# - <task>`
2. Output Position Statement — your expert framing for this task
3. Orient and investigate to understand context
4. Output action plan for approval
5. [Execute discovered tasks — placeholder, replace with specific tasks]
6. Validate all KRs. Output positive and negative evidence for each, then decide
```

>

---

## Tasks

**IMPORTANT:** Each task specifies modes to use. When executing a task you MUST read those modes if you haven't already. Select tasks based on context. Each task shows which KR it serves.

### [Task Name] (→ KR#)

**Goal:** [What this task achieves]

**When:** [When to execute this task]

**Mode:** [mode-name](../modes/mode-name.md)

[OPTIONAL] **Protocol (Override):** <Can include special protocols or require other protocols be used>

**Instructions:** [How to execute. Be specific about what to do, but let AI choose techniques from the mode.]

**Inputs:**
- [required input] (required)
- [optional input] (optional)

**Outputs:** 

<Outputs can be a reference to a style, output must align with the mode, though, specify any default output values>

- [required output <if has a default value mention it>]
- [optional output]

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
