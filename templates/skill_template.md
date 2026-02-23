---
name: skill-name
description: >
  [What this skill does. Be specific about when to use this skill.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this skill. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
argument-hint: "[optional: e.g. [subject] or [feature]]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Skill Name

**Goal:** [What this skill achieves - one sentence.]

**Intent:** [Why this skill exists - what problem it solves or prevents.]

**Scope:** [What this skill covers. Be specific about activities and artifacts.]

---

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. [constraint]
2. [constraint]
3. [constraint]

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what must be provided]

**Elicit if not provided:**
- [what skill will detect or ask about]

**Optional:** [optional inputs that enhance the skill]

## Postconditions

The resulting state after the skill is finished.

**Success:** [state when skill completes successfully]

**Failure:** [state when skill cannot complete]

---

## Execution Items

Execute these to achieve each Key Result. Each item shows which KR it serves.

### [Execution Item Name] (→ KR#)

[What this does and when to use it.]

### [Execution Item Name] (→ KR#, KR#)

[What this does and when to use it.]

**Note:** Workflows replace this section with Steps → Tasks. Primitives replace this with Actions.

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Execution items show (→ KR#), all KRs served
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Coherent:** Execution flows logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

[Domain-specific terms or other notes. Write "None" if not applicable.]
