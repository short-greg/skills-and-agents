# Skill Creation Guideline

This guideline explains how to create skills (primitives and workflows) for the skills-and-agents framework.

## What is a Skill?

A skill is a structured instruction set that guides Claude through a specific cognitive process. Skills ensure consistent, reliable execution by defining clear goals, requirements, and execution steps.

**Two types of skills:**
- **Primitives** - Atomic cognitive actions (single, indivisible operations)
- **Workflows** - Multi-step processes that compose primitives

## Creating a New Skill

### 1. Determine Skill Type

**Use a Primitive when:**
- The action is atomic (cannot be meaningfully decomposed)
- It answers one specific question or performs one cognitive operation
- It does not require orchestrating multiple other operations

**Use a Workflow when:**
- The process requires multiple steps in sequence
- It orchestrates 2+ primitives to achieve a larger goal
- It has phases or stages

### 2. Follow the Template

Use the skill template below, customizing sections as appropriate for your skill type:
- Primitives: Use 2 Key Results, Actions section
- Workflows: Use 2-4 Key Results, Steps → Tasks sections

### 3. Validate the Skill

Before finalizing, verify against all Validation Criteria below.

For each criterion, provide:
- **Reasons to PASS:** Evidence supporting the criterion is met
- **Reasons to FAIL:** Evidence the criterion is not met
- **DECISION:** PASS or FAIL based on the evidence

---

## Skill Template

```markdown
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

1. [constraint on HOW to execute]
2. [constraint on HOW to execute]
3. [constraint on HOW to execute]

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

### [Item Name] (→ KR#)

[What this does and when to use it.]

### [Item Name] (→ KR#, KR#)

[What this does and when to use it.]

**Note:**
- Workflows replace this section with: Steps (ordered list) → Tasks (detailed execution)
- Primitives replace this section with: Actions (detailed execution)

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
- [ ] **LLM-focused:** No unnecessary explanations - only include information necessary for LLM execution

---

## Additional Notes and Terms

[Domain-specific terms or other notes. Write "None" if not applicable.]
```

---

## Validation Criteria Explained

### 1. Structure
All required sections must be present with clear, imperative one-line section headers. Frontmatter must be complete and valid.

### 2. KRs vs Requirements
**Key Results** describe outcomes (WHAT is achieved) - they are measurable results.
**Requirements** describe constraints (HOW to execute) - they are process rules.

**Anti-pattern:** Putting outcomes in Requirements or constraints in Key Results.

### 3. Traceability
Every execution item (Task/Action) must show which Key Result(s) it serves using (→ KR#) notation.
Every Key Result must have at least one execution item that serves it.

### 4. Preconditions
Preconditions must be categorized into three groups:
- **Required:** Must be provided to proceed
- **Elicit if not provided:** Skill will detect or ask for these
- **Optional:** Nice to have but not required

### 5. No Redundancy
Each piece of information should appear exactly once. Don't repeat information across sections.

### 6. Coherent
Steps/actions must flow logically. No contradictions between sections. The skill should make sense as a whole.

### 7. Concise
Use as few words as possible. No unnecessary verbosity or duplication.

### 8. Complete
All necessary information is provided. All Key Results are achievable through the defined execution items.

### 9. Precise
Use specific, unambiguous language. Define terms clearly. Avoid vague descriptions.

### 10. LLM-focused
Only include information necessary for the LLM to execute the skill. Avoid explanations of why something works, background context the LLM already knows, or justifications for design choices. Skills are execution instructions, not educational material.

**Anti-pattern:** Explaining concepts the LLM already understands
```markdown
**Scope:** Validation checks if code works. Validation is important because...
```

✅ **Correct: Direct execution focus**
```markdown
**Scope:** Verifying outputs against success criteria by running tests and checking behavior.
```

---

## Key Principles

### Goal, Intent, Scope
- **Goal:** One sentence describing what the skill achieves
- **Intent:** Why this skill exists - what problem it solves or prevents
- **Scope:** Detailed description of what's covered, using concrete terms

### Key Results (WHAT)
- Measurable outcomes that define success
- 2 KRs for primitives, 2-4 KRs for workflows
- Must be verifiable as pass/fail
- Focus on results, not process

### Requirements (HOW)
- Constraints on execution process
- How the skill must be performed
- References to protocols (tracking.md, recovery.md, etc.)
- Iteration limits, ordering constraints, quality standards

### Preconditions vs Postconditions
- **Preconditions:** What must exist before starting
- **Postconditions:** State after skill completes (success/failure)

### Execution Items
- Each shows which KR(s) it serves
- Written as actions, not descriptions
- Enough detail to execute, but not over-specified

---

## Common Anti-Patterns

❌ **Outcomes in Requirements**
```markdown
## Requirements
1. Implementation must match design  # This is an outcome (KR), not a constraint
```

✅ **Correct: Constraints in Requirements**
```markdown
## Requirements
1. Read design before writing code  # This is a constraint on HOW
```

❌ **Process constraints in Key Results**
```markdown
## Key Results
1. All files are read before writing  # This is HOW, not WHAT
```

✅ **Correct: Outcomes in Key Results**
```markdown
## Key Results
1. Implementation matches design  # This is WHAT is achieved
```

❌ **Vague scope**
```markdown
**Scope:** This skill helps with testing.
```

✅ **Specific scope**
```markdown
**Scope:** Designing test strategies by identifying units to test, edge cases, and test boundaries per test pyramid principles.
```
