**This template inherits from [skill_template.md](skill_template.md) with primitive-specific additions:**
- **Actions section** — Replaces generic Execution Items
- **Simpler Requirements** — Primitives typically have fewer constraints than workflows
- **2 Key Results** — Primitives are atomic, so fewer KRs

---
name: primitive-name
description: >
  [What this primitive does. A primitive is an atomic cognitive action - it does one thing well.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
argument-hint: "[optional: e.g. [subject] or [topic]]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Primitive Name

**Goal:** [What this primitive achieves - one sentence.]

**Intent:** [Why this primitive exists - what problem it solves or prevents.]

**Scope:** [What this primitive covers. A primitive answers one question that no other primitive answers.]

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. [measurable outcome]
2. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. [constraint on execution]
2. [constraint on execution]

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what must be provided to proceed]

**Elicit if not provided:**
- [what primitive will detect or ask about]

**Optional:** [optional inputs that enhance the primitive]

## Postconditions

The resulting state after the primitive is finished.

**Success:** [state when primitive completes successfully]

**Failure:** [state when primitive cannot complete]

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### [Action Name] (→ KR#)

[What this action does, when to use it.]

### [Action Name] (→ KR#, KR#)

[What this action does, when to use it.]

### Validate Results (→ KR#)

Verify the primitive against all Validation Criteria below.

For each criterion, provide:
- **Reasons to PASS:** Evidence supporting the criterion is met
- **Reasons to FAIL:** Evidence the criterion is not met
- **DECISION:** PASS or FAIL based on the evidence

Check all 8 criteria:
1. Structure: All sections present with one-line imperatives
2. KRs vs Requirements: KRs are outcomes (WHAT), Requirements are constraints (HOW)
3. Traceability: Actions show (→ KR#), all KRs served by at least one action
4. Preconditions: Categorized as Required, Elicit if not provided, or Optional
5. Coherent: Actions flow logically, no contradictions
6. Concise: As few words as possible, no duplication
7. Complete: All necessary information provided, all KRs achievable from Actions
8. Precise: Specific, unambiguous language

Report overall result (X/8). On failure: identify gaps and take corrective actions.

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW)
- [ ] **Traceability:** Actions show (→ KR#), all KRs served by at least one action
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **Coherent:** Actions flow logically, no contradictions
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Actions
- [ ] **Precise:** Specific, unambiguous language

---

## Additional Notes and Terms

[Domain-specific terms or other notes. Write "None" if not applicable.]
