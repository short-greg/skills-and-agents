# Primitive Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions and validation criteria.

---

## What is a Primitive?

A primitive is an **atomic cognitive action** - a single, indivisible operation that cannot be meaningfully decomposed into smaller steps.

**Primitives answer one specific question:**
- `orient` - "Where are we now?"
- `define` - "What are we building and how will we know it's done?"
- `validate` - "Does this meet the criteria?"
- `critique` - "How good is this and how could it be better?"

**Key characteristics:**
- Atomic (not decomposable)
- Single-purpose (does one thing well)
- Self-contained (can be used independently)
- 2 Key Results (primitives are focused)

---

## Writing Style for Primitives

Primitives are **instruction-focused** or **evaluation-focused**, not descriptive essays.

**Key principles:**
1. **Action-oriented/instruction-oriented OR evaluation-oriented language**
   - Actions: Use imperatives: "Identify gaps", "Verify criteria", not "The primitive identifies..."
   - Evaluation: Use checks: "Gaps identified?", "Criteria verified?", not "The validation process..."
2. **Evaluation criteria over descriptions** - Focus on what to check or do, not what things mean
3. **Minimal descriptions** - Goal/Intent/Scope should be brief context only (2-3 sentences max)
4. **Instructions first** - Actions section is the heart of the primitive

**Example transformations:**
- ❌ Descriptive: "Validation is the process of checking whether something meets criteria..."
- ✅ Action-oriented: "Verify against each criterion. Check evidence. Report pass/fail with specifics."
- ✅ Evaluation-oriented: "Criteria verified? Evidence checked? Pass/fail reported with specifics?"

---

## Research Best Practices

**Before writing a new primitive, research in this order:**

**Step 1: Claude Code's website FIRST**

1. Search Claude Code's documentation: "site:claude.ai OR site:docs.anthropic.com [primitive topic] best practices"
2. Incorporate Claude Code's guidelines as foundation
3. These take precedence

**Step 2: Expert/academic best practices (in addition)**

1. **Web search** for academic and expert sources on the cognitive operation
   - Example: Creating `reason` primitive → search "reasoning techniques academic cognitive science"
   - Example: Creating `validate` primitive → search "validation criteria software engineering best practices"

2. **Incorporate findings** into Actions section (complementing Claude Code guidelines)
   - Add techniques from academic sources
   - Reference expert frameworks
   - Include non-obvious strategies

3. **Add References section** with ALL sources (Claude Code + research)

**This ensures primitives reflect Claude Code guidelines PLUS expert knowledge, not just intuition.**

---

## When to Create a Primitive

Create a primitive when you have:
- A distinct cognitive operation not covered by existing primitives
- An atomic action that cannot be broken down further
- A clear question it answers that no other primitive answers

**Before creating a new primitive:**
1. **Research** best practices from experts/academics (see above)
2. Check if an existing primitive already covers this (>50% overlap means extend that one instead)
3. Verify it's truly atomic (can't be decomposed into multiple primitives)
4. Confirm it has a distinct purpose

---

## Primitive-Specific Structure

Primitives differ from the base skill template in these ways:

### 1. Conciseness Target
Primitives should be **100-150 lines maximum**. If longer, refactor into multiple primitives or remove verbose descriptions.

### 2. Simpler Requirements
Primitives typically have fewer constraints than workflows (2-4 requirements vs 5-9).

### 3. Exactly 2 Key Results
Primitives are atomic, so they have exactly 2 KRs (not 3-4 like workflows).

### 4. Actions Section
Replace "Execution Items" with "Actions" - the detailed steps to execute the primitive.
- Use **tables** for categorizing actions when appropriate (clearer than prose)
- Each action maps to KR(s): `(→ KR#)`
- Include **evaluation criteria** at end of Actions section

### 5. No Steps Section
Primitives don't have a separate Steps section - they go straight to Actions.

---

## Primitive Template

```markdown
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
- [ ] **LLM-focused:** No unnecessary explanations
- [ ] **Distinct Purpose:** Answers question no other primitive answers
- [ ] **Atomic:** Single cognitive action, not decomposable

---

## Additional Notes and Terms

[Domain-specific terms or other notes. Write "None" if not applicable.]
```

---

## Additional Validation Criteria for Primitives

In addition to the 10 base criteria from [skill_creation_guideline.md](skill_creation_guideline.md), primitives must also pass:

### 11. Distinct Purpose
The primitive must answer a question that no existing primitive answers. Check for >50% overlap with existing primitives.

### 12. Atomic
The primitive must be a single cognitive action, not decomposable into multiple primitives or steps.

---

## Common Primitive Anti-Patterns

❌ **Too many Key Results**
```markdown
## Key Results
1. Current state is described
2. Gaps are identified
3. Issues are surfaced
4. Strengths are noted
5. Orientation is actionable
```
*Primitives should have exactly 2 KRs. Consolidate related outcomes.*

✅ **Correct: 2 Key Results**
```markdown
## Key Results
1. Current state described accurately with gaps, issues, risks, and strengths identified
2. Orientation is actionable — reader knows what decisions need to be made
```

❌ **Not atomic (references other primitives)**
```markdown
## Actions
### Analyze System (→ KR1)
Use `investigate` to understand the system, then use `critique` to identify issues.
```
*This is a workflow, not a primitive. Primitives don't orchestrate other primitives.*

✅ **Atomic actions**
```markdown
## Actions
### Analyze System (→ KR1)
Read code and documentation. Identify what works vs broken. Surface issues with specific locations.
```

❌ **Scope references other primitives**
```markdown
**Scope:** Identifying issues and improvements by using validate and critique primitives.
```
*Scope should be self-contained.*

✅ **Self-contained scope**
```markdown
**Scope:** Identifying issues and improvements by reviewing work for quality, finding bugs, and suggesting specific fixes.
```

---

## Tips for Writing Good Primitives

1. **Start with the question:** What specific question does this primitive answer?
2. **Keep it atomic:** If you find yourself writing "first... then... finally...", it's probably a workflow
3. **2 KRs only:** Consolidate related outcomes into compound Key Results
4. **Simple requirements:** Primitives typically need 2-4 requirements, not 9
5. **Self-contained:** Don't reference other primitives in scope or actions
6. **Clear triggers:** Include natural phrases users might say to invoke this primitive
