**This template inherits from [skill_template.md](skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: workflow-name
description: >
  [What this workflow does. Include trigger keywords and phrases users naturally say.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
argument-hint: "[optional: e.g. [feature] or [bug description]]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, TodoWrite
---

# Workflow Name

**Goal:** [What this workflow achieves - one sentence.]

**Intent:** [Why this workflow exists - what problem it solves, what it prevents.]

**Scope:** [What this workflow covers. Be specific about the types of activities and artifacts.]

---

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — [specific recovery behavior]
3. [constraint]
4. Iterate up to N times if validation fails, [specific iteration behavior]

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what user must provide]

**Elicit if not provided:**
- [what workflow will detect or ask about]

**Optional:** [optional inputs that enhance the workflow]

## Postconditions

The resulting state after the skill is finished.

**Success:** [state when workflow completes successfully]

**Failure:** [state when workflow fails]

## Steps

Complete the Tasks in this order.

Steps:
1. [step referencing task]
2. [step referencing task]
3. [step referencing task]

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### [Task Name] (→ KR#)

[What this task does. Reference primitives or protocols as needed.]

### [Task Name] (→ KR#, KR#)

[What this task does.]

### [Task Name] (→ KR#)

[What this task does.]

### Validate Workflow (→ KR#)

Use `validate` primitive. Verify the workflow against all Validation Criteria below.

For each criterion, provide:
- **Reasons to PASS:** Evidence supporting the criterion is met
- **Reasons to FAIL:** Evidence the criterion is not met
- **DECISION:** PASS or FAIL based on the evidence

Check all 10 criteria:
1. Structure: All sections present with one-line imperatives. Frontmatter complete.
2. KRs vs Requirements: KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
3. Traceability: Tasks show (→ KR#), all KRs served by at least one task
4. Preconditions: Categorized as Required, Elicit if not provided, or Optional
5. No redundancy: Each piece of information appears exactly once
6. Recovery: Includes progress tracking, recovery behavior, iteration limit in Requirements
7. Coherent: Steps flow logically, no contradictions between sections
8. Concise: As few words as possible, no duplication
9. Complete: All necessary information provided, all KRs achievable from Tasks
10. Precise: Specific, unambiguous language, clear definitions

Report overall result (X/10). On failure: revise to address failures, then re-validate.

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
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

[Domain-specific terms, customization points, or other notes. Write "None" if not applicable.]

**Note on Protocols:** Protocols (tracking, recovery, checklists, etc.) are typically handled by primitives and specified in Requirements. Only include a separate Protocols section if the workflow requires domain-specific protocols beyond the standard ones.
