# Workflow Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions.

---

## Table of Contents

- [Workflow Creation Guideline](#workflow-creation-guideline)
  - [Table of Contents](#table-of-contents)
  - [What is a Workflow?](#what-is-a-workflow)
  - [Implementation Steps](#implementation-steps)
  - [Writing Style](#writing-style)
  - [Validation Criteria](#validation-criteria)
  - [Tips](#tips)
  - [Template](#template)

---

## What is a Workflow?

A workflow is a **multi-step process** that orchestrates primitives to achieve a larger goal.

**Workflows compose primitives:**
- `feature_workflow` - Orchestrates orienting → defining → designing → implementing → validating
- `bugfix_workflow` - Orchestrates orienting → investigating → defining → implementing → validating
- `refactor_workflow` - Orchestrates orienting → designing → implementing → validating

**Key characteristics:**
- Multi-step (has sequential phases)
- Orchestrates 2+ primitives
- 3-4 Key Results (more complex than primitives)
- Includes recovery and progress tracking
- 150-200 lines (200-250 acceptable with user permission)

**When to create a workflow:**
- A process requiring multiple steps in sequence
- A task that orchestrates 2+ primitives to achieve a goal
- Clear phases or stages that build on each other
- **Don't create when:** The action is atomic (use a primitive instead), only uses one primitive (just use that primitive directly)

---

## Implementation Steps

1. Find out the goal and intent of the workflow
2. Clarify your understanding
3. Confirm it requires multiple primitives/workflows (not just one)
4. Identify which primitives/workflows might be orchestrated
5. Do web research to find out:
   1. General web research on the process
   2. Domain-specific web research
6. **Create decision tables**:
   - **Primitives/Workflows table** with columns:
     - Primitive/Workflow name
     - Reasons to use (how it helps this workflow)
     - Reasons not to use (why it might not fit)
     - Decision (use or don't use)
   - **Protocols table** with same columns
7. Create the workflow according to the template
8. Validate the workflow
9. Do final confirmation from the creator

---

## Writing Style

Workflows are **orchestration-focused** - they coordinate primitives and other workflows to achieve larger goals.

**Key principles:**
1. **Task-oriented language** - Tasks should specify which primitives/workflows/protocols to use (strongly recommended)
2. **Recovery-aware** - Must include progress tracking, recovery behavior, iteration limits
3. **Minimal descriptions** - Goal/Intent/Scope should be brief (2-3 sentences max)
4. **Clear task structure** - Each task shows → KR#, and ideally uses primitive-name/workflow-name, uses protocol-name
5. **Specify workflow style** - Declare whether Declarative or Imperative, Implicit or Explicit

**Workflow Styles:**

**Goal-Oriented vs Process-Oriented:**
- **Goal-Oriented (Declarative)** - Specify what to achieve, let primitives/workflows determine how. Relies heavily on preconditions/postconditions of skills used.
- **Process-Oriented (Imperative)** - Specify exact steps to execute. More control, less flexibility.
- **Recommendation:** Checklist setup (first step) and validation (last step) should be process-oriented. Middle steps can be goal-oriented or process-oriented based on user needs.

**Implicit vs Explicit:**
- **Implicit** - Minimal instructions, rely on primitive/workflow intelligence
- **Explicit** - Detailed instructions at each step

**Content pattern:**
1. **Steps** - High-level phases (each typically maps to one primitive or workflow)
2. **Tasks** - Detailed tasks showing which primitives/workflows/protocols to use
3. **Preconditions/Postconditions** - What's needed, what's produced (crucial for declarative workflows)

**Structural differences from primitives:**
- 150-200 lines (200-250 acceptable with user permission)
- 3-4 Key Results vs primitives with 2
- Orchestrates multiple primitives and/or workflows
- Must include recovery requirements
- Tasks replace Actions
- Must specify workflow style

---

## Validation Criteria

For each criterion, you MUST output all evidence that it passes and all evidence that it fails. Once evidence is output, weigh the evidence to decide if it passes or fails.

**Structural:**
- [ ] **Follows template exactly:** All sections present with correct format (3-4 KRs, Tasks show → KR# and primitives/protocols used, etc.)
- [ ] **150-200 lines:** Length stays within target range (200-250 acceptable with user permission)

**Content Quality:**
- [ ] **Coherent:** Sections flow logically without contradictions
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Concise:** Minimum necessary words, no duplication
- [ ] **Precise:** Specific, unambiguous language
- [ ] **LLM-focused:** Non-obvious constraints, no needless definitions

**Workflow-Specific:**
- [ ] **Multi-primitive:** Uses 2+ primitives (not just one)
- [ ] **Recovery included:** Requirements include progress tracking, recovery behavior, iteration limit
- [ ] **Tasks specify primitives/protocols when used:** Tasks show which primitives and protocols to use (strongly recommended, not required for all tasks)
- [ ] **Research completed:** References section includes relevant sources

---

## Tips

1. **Specify workflow style:** Declare Goal-Oriented (Declarative), Process-Oriented (Imperative), or Hybrid
2. **Hybrid recommended:** First step (checklist setup) and last step (validation) process-oriented; middle steps goal-oriented
3. **Each step typically maps to one skill:** Orient → Define → Design → Implement → Validate (can be primitives or workflows)
4. **Tasks can use primitives OR workflows:** Don't just orchestrate primitives - workflows can orchestrate other workflows too
5. **Include preconditions/postconditions:** Crucial for goal-oriented workflows - specify what each skill needs and produces
6. **Include recovery:** All workflows need progress tracking per `tracking.md`, recovery per `recovery.md`, iteration limits
7. **3-4 Key Results:** Workflows can have more KRs than primitives (but not too many)
8. **Handle failures:** Specify what happens when tasks fail (retry, escalate, etc.)
9. **Don't create single-skill workflows:** If it only uses one primitive/workflow, just use that skill directly

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

## Protocols

Protocols that this workflow uses.

- **[protocol_name.md]** — Must use to [required action]
- **[protocol_name.md]** — Use when [optional condition]

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what must be provided]

**Elicit if not provided:**
- [what workflow will detect or ask about]

**Optional:** [optional inputs that enhance the workflow]

## Postconditions

The resulting state after the skill is finished.

**Success:** [state when workflow completes successfully]

**Failure:** [state when workflow cannot complete]

---

## Steps


Complete the Tasks in this order.

1. [Step Name]
2. [Step Name]
3. [Step Name]
4. [Step Name]

<Recommended: Require the first steps to be to set up the checklist with key results then resolve all preconditions that are okay to "elicit". Then require the final step be to validate the key results of the workflow.

Even if it is goal-oriented. However, in goal-oriented, if a step fails it may go back. In process-oriented, it will only go back if required and may fail if a step fails.

Example (use the required format, though):
1. Set up the checklist
2. Elicit all pre-conditions marked as "Elicit"
3. Finalize the checklist with tasks to execute
4. List of all tasks after finalizing
5. Validate that the step was completed>

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves. Strongly recommended: specify which primitives/workflows/protocols to use.

### [Task Name] (→ KR#, uses primitive-name, uses protocol-name)

[What this task does. Context for using the primitive in this workflow.]

**Preconditions:** [What primitive needs - crucial for goal-oriented workflows]
**Postconditions:** [What primitive produces - crucial for goal-oriented workflows]

**On failure:** [What to do if this task fails - retry, escalate, etc.]

### [Task Name] (→ KR#, KR#, uses workflow-name)

[What this task does. Tasks can use other workflows, not just primitives.]

**Preconditions:** [What workflow needs]
**Postconditions:** [What workflow produces]

### [Task Name] (→ KR#, uses primitive-name)

[What this task does.]

Per `protocol.md` — [reference specific protocol guidance].

### [Task Name] (→ KR#)

[What this task does - if no specific primitive/workflow/protocol needed, just describe the task.]

---

## Available Primitives <OPTIONAL>

<Only include if helpful to list primitives used. Many workflows won't need this section.>

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow.

<Here is an example:
- `orienting` — [when to use in this workflow]
- `defining` — [when to use in this workflow]
- `designing` — [when to use in this workflow]
- `implementing` — [when to use in this workflow]>

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
