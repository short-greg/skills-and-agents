# Primitive Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions.

---

## Table of Contents

- [Primitive Creation Guideline](#primitive-creation-guideline)
  - [Table of Contents](#table-of-contents)
  - [What is a Primitive?](#what-is-a-primitive)
  - [Implementation Steps](#implementation-steps)
  - [Writing Style](#writing-style)
  - [Validation Criteria](#validation-criteria)
  - [Tips](#tips)
  - [Template](#template)

---

## What is a Primitive?

A primitive is an **atomic cognitive action** - a single, indivisible operation that cannot be meaningfully decomposed into smaller steps.

**Primitives answer one specific question:**
- `orienting` - "Where are we now?"
- `defining` - "What are we building and how will we know it's done?"
- `critiquing` - "How good is this and how could it be better?"
- `implementing` - "How do I build this?"

**Key characteristics:**
- Atomic (not decomposable)
- Single-purpose (does one thing well)
- Self-contained (can be used independently)
- Exactly 2 Key Results (primitives are focused)
- 80-130 lines (130-150 acceptable with user permission)

**When to create a primitive:**
- A distinct cognitive operation not covered by existing primitives
- An atomic action that cannot be broken down further
- A clear question it answers that no other primitive answers
- **Before creating:** Research best practices, check for >50% overlap with existing primitives, verify it's truly atomic

---

## Implementation Steps

1. Find out the goal and intent of the primitive
2. Clarify your understanding
3. Confirm it is not already covered
4. Do web research to find out
   1. General web research
   2. Domain-specific web research
5. **Create decision table** - List all relevant protocols in a table with columns:
   - Protocol name
   - Reasons to use (how it helps this primitive)
   - Reasons not to use (why it might not fit)
   - Decision (use or don't use)
6. Create the primitive according to the template
7. Validate the primitive
8. Do final confirmation from the creator

---

## Writing Style

Primitives are **instruction-focused** or **evaluation-focused**, not descriptive essays.

**Key principles:**
1. **Action-oriented OR evaluation-oriented language** - Use imperatives ("Identify gaps") or checks ("Gaps identified?"), not descriptions ("The primitive identifies...")
2. **Evaluation criteria over descriptions** - Focus on what to check or do, not what things mean
3. **Minimal descriptions** - Goal/Intent/Scope should be brief (2-3 sentences max)
4. **Output-focused language** - Use "Output evidence" not "Find evidence" to prevent LLMs skipping documentation

**Structural differences from workflows:**
- 80-130 lines (130-150 acceptable with user permission)
- 2-4 requirements vs workflows with 5-9
- Exactly 2 Key Results vs workflows with 3-4
- Actions show KR linkage and protocol usage
- All primitives reference `primitive_execution.md` protocol

---

## Validation Criteria

For each criterion, you MUST output all evidence that it passes and all evidence that it fails. Once evidence is output, weigh the evidence to decide if it passes or fails.

**Structural:**
- [ ] **Follows template exactly:** All sections present with correct format (2 KRs, 2-4 Requirements, Preconditions categorized, Actions show → KR# and protocols, etc.)
- [ ] **80-130 lines:** Length stays within target range (130-150 acceptable with user permission)

**Content Quality:**
- [ ] **Coherent:** Sections flow logically without contradictions
- [ ] **Complete:** All necessary information provided, all KRs achievable from Actions
- [ ] **Concise:** Minimum necessary words, no duplication
- [ ] **Precise:** Specific, unambiguous language
- [ ] **LLM-focused:** Non-obvious constraints, no needless definitions

**Primitive-Specific:**
- [ ] **Distinct Purpose:** Answers question no other primitive answers (<50% overlap with existing)
- [ ] **Atomic:** Single cognitive action, not decomposable into multiple primitives
- [ ] **Self-contained:** Doesn't reference other primitives in scope or actions
- [ ] **Research completed:** References section includes Claude Code docs + expert/academic sources

---

## Tips

1. **Start with the question:** What specific question does this primitive answer?
2. **Keep it atomic:** If you find yourself writing "first... then... finally...", it's probably a workflow
3. **2 KRs only:** Consolidate related outcomes into compound Key Results
4. **Simple requirements:** Primitives typically need 2-4 requirements, not 9
5. **Self-contained:** Don't reference other primitives in scope or actions
6. **Clear triggers:** Include natural phrases users might say to invoke this primitive
7. **Output-focused:** Use "Output X" not "Find X" to ensure LLM documents work

---

## Template

**Note:** Text in `[brackets]` are placeholders - replace with actual content. Text in `<angle brackets>` are guidelines - do NOT include in your final primitive. Instructions that go in the final template, are not included in either.

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

## Protocols

Protocols that this primitive uses.

- **[protocol_name.md]** — Must use to [required action]
- **[protocol_name.md]** — Use when [optional condition]

---

## Steps

Execute per protocol `primitive_execution.md` — track, plan, execute, validate.

---

## Preconditions

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

Select based on context. Each action shows which KR it serves.

### [Action Name] (→ KR#, uses protocol1.md, protocol2.md)

[Required state]. [How to achieve it.]

### [Action Name] (→ KR#, KR#, uses protocol1.md)

[Required state]. [How to achieve it.]

---

## Additional Notes and Terms

<Domain-specific terms or other notes. Write "None" if not applicable.>

---

## References

<REQUIRED: Research must be completed before creating a primitive.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [primitive topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[primitive topic] techniques academic" or "[topic] best practices software engineering"
- Example: Creating `reasoning` → search "reasoning techniques academic cognitive science"
- Example: Creating `designing` → search "software design principles best practices">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
