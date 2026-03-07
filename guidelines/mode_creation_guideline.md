# Mode Creation Guideline

**This guideline inherits from [skill_creation_guideline.md](skill_creation_guideline.md).**

Read the base skill guideline first for common instructions.

---

## Table of Contents

- [Mode Creation Guideline](#mode-creation-guideline)
  - [Table of Contents](#table-of-contents)
  - [What is a Mode?](#what-is-a-mode)
  - [Steps](#steps)
  - [Writing Style](#writing-style)
  - [Validation Criteria](#validation-criteria)
  - [Tips](#tips)
  - [Template](#template)

---

## What is a Mode?

A mode is an **atomic cognitive action** - a single, indivisible operation that cannot be meaningfully decomposed into smaller steps.

**Modes answer one specific question:**
- `orienting` - "Where are we now?"
- `defining` - "What are we building and how will we know it's done?"
- `designing` - "How should we build this?"
- `implementing` - "How do I build this?"
- `evaluating` - "Is this correct and of quality?"
- `investigating` - "What do we need to know?"
- `brainstorming` - "What are the possibilities?"
- `planning` - "What steps and in what order?"
- `maintaining` - "Is this system healthy and well-documented?"

**Modes are Goal Oriented**
Each mode has a clear Goal (what it achieves) and Key Results (how to verify achievement). Actions exist to serve Key Results. This creates accountability: a mode succeeds or fails based on whether KRs are met.

**Key Results Must Be Flexible**
Key Results must be **outcome-focused, not task-specific**. The same mode should support multiple use cases while achieving the same KRs.

Example: `investigating` mode can be used for:
- **Research Unknowns** - Inputs: technical unknowns; Outputs: findings, approaches, recommendations
- **Research Common Practices** - Inputs: domain to research; Outputs: best practices, tradeoffs, recommendations
- **Diagnose Bug** - Inputs: symptoms, errors; Outputs: root cause, evidence, recommendations

All achieve the same KRs: (1) Uncertainty reduced with grounded findings, (2) Recommendations provided.

Different workflow tasks use different combinations of Actions, but all satisfy the same Key Results.

**Key characteristics:**
- Single-purpose (does one thing well)
- Self-contained (can be used independently)
- Exactly 2 Key Results (modes are focused)
- KRs are outcome-focused, not task-specific (flexible for multiple use cases)
- 80-130 lines (130-150 acceptable with user permission)

**When to create a mode:**
- A distinct cognitive operation not covered by existing modes
- An atomic action that cannot be broken down further
- A clear question it answers that no other mode answers

---

## Steps

Follow these steps to create and validate a mode.

1. Elicit the goal and intent of the mode
2. Clarify your understanding
3. Confirm it is not already covered
4. Critique whether it is already covered
5. Do web research to find out
   1. General web research from experts
   2. Non-domain-specific scientific research
   3. Domain-specific web research
6. **Create decision table** - Map each protocol to actions. Output a table with columns:
   - Protocol name
   - Maps to Action (which action in the mode would use this protocol?)
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
7. Create the mode according to the template
8. Validate the mode
9. Do final confirmation from the creator


---

## Writing Style

Modes are **instruction-focused** or **evaluation-focused**, not descriptive essays.

**Key principles:**
1. **Action-oriented OR evaluation-oriented language** - Use imperatives ("Identify gaps") or checks ("Gaps identified?"), not descriptions ("The mode identifies...")
2. **Evaluation criteria over descriptions** - Focus on what to check or do, not what things mean
3. **Minimal descriptions** - Goal/Intent/Scope should be brief (2-3 sentences max)
4. **Output-focused language** - Use "Output evidence" not "Find evidence" to prevent LLMs skipping documentation

**Structural differences from workflows:**
- 80-130 lines (130-150 acceptable with user permission)
- 2-4 requirements vs workflows with 5-9
- Exactly 2 Key Results vs workflows with 3-4
- Actions show KR linkage and protocol usage
- All modes inherit from `base_mode.md` (execution pattern)

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

**Mode-Specific:**
- [ ] **Distinct Purpose:** Answers question no other mode answers (<50% overlap with existing)
- [ ] **Atomic:** Single cognitive action, not decomposable into multiple modes
- [ ] **Self-contained:** Doesn't reference other modes in scope or actions
- [ ] **Research completed:** References section includes Claude Code docs + expert/academic sources
- [ ] **Goal-oriented**: Actions each achieve a specific goal, goals are all in line with the scope
- [ ] **Action-scope**: Do actions go beyond the scope of the mode or do they leak to other modes
- [ ] **Flexible Key Results:** KRs are outcome-focused, not task-specific; can support at least 3 different use cases while achieving same KRs

---


## Tips

1. **Start with the question:** What specific question does this mode answer?
2. **Keep it atomic:** If you find yourself writing "first... then... finally...", it's probably a workflow
3. **Exactly 2 KRs:** Consolidate related outcomes into compound Key Results
4. **Flexible KRs:** Write KRs that are outcome-focused, not task-specific - test by listing 3+ different use cases that achieve the same KRs
5. **Simple requirements:** Modes typically need 2-4 requirements, not 9
6. **Self-contained:** Don't reference other modes in scope or actions
7. **Clear triggers:** Include natural phrases users might say to invoke this mode
8. **Output-focused:** Use "Output X" not "Find X" to ensure LLM documents work

---

## Template

**Note:** Text in `[brackets]` are placeholders - replace with actual content. Text in `<angle brackets>` are guidelines - do NOT include in your final mode. Instructions that go in the final template, are not included in either.

```markdown
---
name: mode-name
description: >
  [Concise definition - one sentence starting with gerund, e.g. "Understanding the current state of a system, project, or task."]. [What it does in more detail.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
  keywords: [keyword1], [keyword2], [keyword3], [keyword4], [keyword5]
argument-hint: "[optional: e.g. [subject] or [topic]]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Mode Name

**Goal:** [What this mode achieves - one sentence.]

**Intent:** [Why this mode exists - what problem it solves or prevents.]

**Scope:** [What this mode covers. A mode answers one question that no other mode answers.]

---

## Key Results - KR

You must satisfy these to complete the mode successfully.

1. [measurable outcome]
2. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to execute the mode.

1. [constraint on execution]
2. [constraint on execution]

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** [what must be provided to proceed]

**Optional:**
- [optional input that enhances the mode]
- [optional input that enhances the mode]

## Postconditions

**Success:** [state when mode completes successfully]

**Failure:** [state when mode cannot complete]. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### [Action Name] (→ KR#)

**Goal:** [What this action achieves]

**When:** [When to execute this action]

**Protocol:** `protocols/[protocol_name].md`

**Instructions:** [How to execute using techniques from the protocol. Be specific about what to do, but don't prescribe exact techniques - let AI choose from protocol.]

**Inputs:**
- [required input] (required)
- [optional input] (optional)

**Default Output:** [Expected output format if not specified]

---

### [Action Name] (→ KR#, KR#)

**Goal:** [What this action achieves]

**When:** [When to execute this action]

**Protocols:** `protocols/[protocol_name].md`, `protocols/[protocol_name2].md`

**Instructions:** [How to execute using techniques from the protocols. Be specific about what to do, but don't prescribe exact techniques - let AI choose from protocols.]

**Inputs:**
- [required input] (required)
- [optional input] (optional)

**Default Output:** [Expected output format if not specified]

---

## Additional Notes and Terms

<Additional notes, clarifications, terms, or distinctions from other modes. Write "None" if not applicable.>

---

## References

<REQUIRED: Research must be completed before creating a mode.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [mode topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[mode topic] techniques academic" or "[topic] best practices software engineering"
- Example: Creating `thinking` → search "reasoning techniques academic cognitive science"
- Example: Creating `designing` → search "software design principles best practices">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
