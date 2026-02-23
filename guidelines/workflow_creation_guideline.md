# Workflow Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions and validation criteria.

---

## What is a Workflow?

A workflow is a **multi-step process** that orchestrates primitives to achieve a larger goal.

**Workflows compose primitives:**
- `feature_workflow` - Orchestrates orient → define → design → implement → validate
- `bugfix_workflow` - Orchestrates orient → investigate → define → implement → validate
- `refactor_workflow` - Orchestrates orient → design → implement → validate

**Key characteristics:**
- Multi-step (has sequential phases)
- Orchestrates 2+ primitives
- 2-4 Key Results
- Includes recovery and progress tracking

---

## When to Create a Workflow

Create a workflow when you have:
- A process requiring multiple steps in sequence
- A task that orchestrates 2+ primitives to achieve a goal
- Clear phases or stages that build on each other

**Don't create a workflow when:**
- The action is atomic (use a primitive instead)
- It only uses one primitive (just use that primitive directly)

---

## Workflow-Specific Structure

Workflows differ from the base skill template in these ways:

### 1. Steps Section
Ordered list of high-level steps (each typically maps to one primitive).

### 2. Tasks Section
Replaces "Execution Items" with detailed tasks, each showing which primitive(s) to use.

### 3. Available Primitives Section
Lists primitives used in this workflow with brief description of when to use each.

### 4. Standard Requirements
All workflows must include: progress tracking, recovery behavior, iteration limit.

---

## Workflow Template

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

---

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]
4. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task
3. [workflow-specific constraint]
4. [workflow-specific constraint]
5. Iterate up to N times if [condition] before escalating

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

## Steps

Complete the Tasks in this order.

Steps:
1. [Step Name]
2. [Step Name]
3. [Step Name]
4. [Step Name]

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### [Task Name] (→ KR#)

[What this task does. Which primitive to use or what to do.]

Per `protocol.md` — [reference relevant protocols or patterns].

### [Task Name] (→ KR#, KR#)

Use `primitive-name` primitive. [Context for using the primitive in this workflow.]

### [Task Name] (→ KR#)

[What this task does.]

**On failure:** [What to do if this task fails.]

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — [when to use in this workflow]
- `define` — [when to use in this workflow]
- `design` — [when to use in this workflow]
- `implement` — [when to use in this workflow]
- `validate` — [when to use in this workflow]

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions
- [ ] **LLM-focused:** No unnecessary explanations
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements

---

## Additional Notes and Terms

[Domain-specific terms, customization points, or other notes. Write "None" if not applicable.]
```

---

## Additional Validation Criteria for Workflows

In addition to the 10 base criteria from [skill_creation_guideline.md](skill_creation_guideline.md), workflows must also pass:

### 11. Recovery
Requirements section must include progress tracking, recovery behavior, and iteration limit.

---

## Common Workflow Anti-Patterns

❌ **Single primitive workflow**
```markdown
## Tasks
### Understand Context (→ KR1)
Use `orient` primitive.

### Report Findings (→ KR2)
Summarize what was found.
```
*If the workflow only uses one primitive, just use that primitive directly.*

✅ **Multi-primitive workflow**
```markdown
## Tasks
### Understand Context (→ KR1)
Use `orient` primitive to understand current state.

### Define Requirements (→ KR1, KR2)
Use `define` primitive to establish goals and success criteria.

### Create Design (→ KR2)
Use `design` primitive to plan technical approach.
```

❌ **Missing recovery requirements**
```markdown
## Requirements
1. Follow project conventions
2. Validate at each step
```
*Workflows MUST include progress tracking, recovery, and iteration limits.*

✅ **Includes recovery**
```markdown
## Requirements
1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup
3. Follow project conventions
4. Iterate up to 3 times if validation fails before escalating
```

❌ **Steps don't match Tasks**
```markdown
## Steps
1. Understand Context
2. Create Design
3. Validate

## Tasks
### Understand Context (→ KR1)
### Define Requirements (→ KR2)
### Create Design (→ KR3)
```
*Steps list must match the Tasks. If "Define Requirements" is a Task, it should be in Steps.*

---

## Tips for Writing Good Workflows

1. **Each step typically maps to one primitive:** Orient → Define → Design → Implement → Validate
2. **Include recovery:** All workflows need progress tracking, recovery behavior, iteration limits
3. **2-4 Key Results:** Workflows can have more KRs than primitives (but not too many)
4. **List primitives used:** Include "Available Primitives" section showing which primitives this workflow uses
5. **Handle failures:** Specify what happens when tasks fail
6. **Reference protocols:** Use `per protocol.md` notation to reference relevant patterns
