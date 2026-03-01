# Primitive Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions.

---

## Table of Contents

- [Primitive Creation Guideline](#primitive-creation-guideline)
  - [Table of Contents](#table-of-contents)
  - [What is a Primitive?](#what-is-a-primitive)
  - [Steps](#steps)
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
- `designing` - "How should we build this?"
- `implementing` - "How do I build this?"
- `evaluating` - "Is this correct and of quality?"
- `investigating` - "What do we need to know?"
- `brainstorming` - "What are the possibilities?"
- `planning` - "What steps and in what order?"
- `maintaining` - "Is this system healthy and well-documented?"

**Primitives are Goal Oriented**
Each primitive has a clear Goal (what it achieves) and Key Results (how to verify achievement). Actions exist to serve Key Results. This creates accountability: a primitive succeeds or fails based on whether KRs are met.

**Key characteristics:**
- Single-purpose (does one thing well)
- Self-contained (can be used independently)
- Exactly 2 Key Results (primitives are focused)
- 80-130 lines (130-150 acceptable with user permission)

**When to create a primitive:**
- A distinct cognitive operation not covered by existing primitives
- An atomic action that cannot be broken down further
- A clear question it answers that no other primitive answers

---

## Steps

Follow these steps to create and validate a primitive.

1. Elicit the goal and intent of the primitive
2. Clarify your understanding
3. Confirm it is not already covered
4. Critique whether it is already covered
5. Do web research to find out
   1. General web research from experts
   2. Non-domain-specific scientific research
   3. Domain-specific web research
6. **Create decision table** - Map each protocol to actions. Output a table with columns:
   - Protocol name
   - Maps to Action (which action in the primitive would use this protocol?)
   - Reasons to use (how it helps that action)
   - Reasons not to use (why it might not fit)
   - Decision (use or don't use)

   Note: If a protocol maps to no action, it's likely not needed. If you find a protocol that SHOULD map to an action that doesn't exist, consider adding that action.

   **Available protocols:**
   - `thinking.md` — reasoning and thinking techniques
   - `tracking_and_recovery.md` — progress tracking and resumption
   - `transparency.md` — documentation and decision recording
   - `software_quality.md` — quality assessment dimensions
   - `system_modularity.md` — component design and boundaries
   - `goal_setting.md` — establishing objectives
   - `criteria_setting.md` — defining measurable criteria
   - `instruction_giving.md` — writing clear instructions
   - `interviewing.md` — eliciting information from users
   - `pragmatics.md` — communication style and framing
   - `frame_of_mind.md` — establishing role and stance
   - `risk_management.md` — identifying and mitigating risks
7. Create the primitive according to the template
8. Validate the primitive
9. Do final confirmation from the creator


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
- All primitives inherit from `base.md` (execution pattern)

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
- [ ] **Goal-oriented**: Actions each achieve a specific goal, goals are all in line with the scope
- [ ] **Action-scope**: Do actions go beyond the scope of the primitive or do they leak to other primitives.

---


## Tips

1. **Start with the question:** What specific question does this primitive answer?
2. **Keep it atomic:** If you find yourself writing "first... then... finally...", it's probably a workflow
3. **Exactly 2 KRs:** Consolidate related outcomes into compound Key Results
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
  [Concise definition - one sentence starting with gerund, e.g. "Understanding the current state of a system, project, or task."]. [What it does in more detail.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
  keywords: [keyword1], [keyword2], [keyword3], [keyword4], [keyword5]
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

Inherits from `base.md` — output lightweight checklist, resolve preconditions, plan actions, execute, report result.

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

### [Action Name] (→ KR#)

<Execute when [condition that triggers this action]. [What this action accomplishes and how to achieve it using protocols/methods.]>

### [Action Name] (→ KR#, KR#)

<Execute when [condition that triggers this action]. [What this action accomplishes and how to achieve it using protocols/methods.]>

---

## Additional Notes

<Additional notes, clarifications, terms, or distinctions from other primitives. Write "None" if not applicable.>

---

## References

<REQUIRED: Research must be completed before creating a primitive.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [primitive topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[primitive topic] techniques academic" or "[topic] best practices software engineering"
- Example: Creating `thinking` → search "reasoning techniques academic cognitive science"
- Example: Creating `designing` → search "software design principles best practices">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
