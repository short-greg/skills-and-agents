# Protocol Creation Guideline

This guideline explains how to create protocols for the skills-and-agents framework.

---

## Table of Contents

- [What is a Protocol?](#what-is-a-protocol)
- [Implementation Steps](#implementation-steps)
- [Writing Style](#writing-style)
- [Validation Criteria](#validation-criteria)
- [Tips](#tips)
- [Template](#template)

---

## What is a Protocol?

A protocol is a **standardized process or pattern** that multiple primitives and workflows follow. Protocols capture essential principles and patterns, distilling expert knowledge into reusable concepts.

**Examples of protocols:**
- `tracking.md` - Progress documentation standards
- `recovery.md` - Resuming from interruption
- `quality.md` - Quality assessment dimensions
- `validation.md` - Evidence-based verification

**Key characteristics:**
- Reusable across multiple skills
- Instruction-focused (not descriptive)
- Contains actionable techniques
- 100-150 lines (150-180 acceptable with user permission)

**When to create a protocol:**
- Pattern is used by multiple primitives/workflows
- Protocol provides non-obvious constraints
- Process needs standardization across the framework
- **Don't create when:** Used by only one skill (put it there instead), too specific to a use case, no clear process to define

---

## Implementation Steps

1. Find out the goal and intent of the protocol
2. Clarify your understanding
3. Confirm pattern is used by multiple skills (not just one)
4. Do web research to find out:
   1. General web research on the pattern/process
   2. Domain-specific web research
5. Create the protocol according to the template
6. Validate the protocol
7. Do final confirmation from the creator

---

## Writing Style

Protocols are **instruction-focused**, not descriptive essays.

**Key principles:**
1. **Action-oriented language** - Use imperatives ("Output evaluation") or instructions ("1. Do X, 2. Do Y"), not descriptions
2. **Evaluation criteria over descriptions** - Focus on what to check, not what things mean
3. **Minimal descriptions** - Goal/Intent/Scope should be brief (2-3 sentences max)
4. **Output-focused language** - "Output an evaluation of X" is better than "Evaluate X"

**Content pattern:**
1. **Description (light)** - Goal/Intent/Scope for context
2. **Instructions** - How to do it (actionable steps in Techniques)
3. **Criteria** - How to evaluate success (in Core Concepts and/or Techniques)
4. **Examples** - Show it in action (optional)

**Structural differences from primitives:**
- 100-150 lines (150-180 acceptable with user permission)
- More instructional sections (Core Concepts, Techniques)
- Examples are optional
- No Key Results (protocols aren't invoked directly)

---

## Validation Criteria

For each criterion, you MUST output all evidence that it passes and all evidence that it fails. Once evidence is output, weigh the evidence to decide if it passes or fails.

**Structural:**
- [ ] **Follows template exactly:** All required sections present with correct format
- [ ] **100-150 lines:** Length stays within target range (150-180 acceptable with user permission)

**Content Quality:**
- [ ] **Coherent:** Sections flow logically without contradictions
- [ ] **Complete:** All necessary information provided, nothing critical missing
- [ ] **Concise:** Minimum necessary words, no duplication
- [ ] **Precise:** Specific, unambiguous language
- [ ] **LLM-focused:** Non-obvious constraints, no needless definitions
- [ ] **Actionable:** Provides clear techniques, not vague philosophy

**Protocol-Specific:**
- [ ] **Used by multiple skills:** Not just one primitive/workflow
- [ ] **Clear boundaries:** Scope states what's included and excluded
- [ ] **Research completed:** References section includes relevant sources

---

## Tips

1. **Start with the problem:** What inconsistency or gap does this protocol address?
2. **Keep it focused:** One protocol per pattern or process
3. **Make it actionable:** Include specific techniques, not just concepts
4. **Output-oriented instructions:** "Output evaluation" better than "Evaluate"
5. **Avoid vague platitudes:** Not "make good decisions", but "Evaluate: Which objectives does this optimize? Which does it sacrifice?"
6. **Reference, don't repeat:** Link to related protocols instead of duplicating content
7. **Test with multiple skills:** Verify the protocol works across different primitives/workflows

---

## Template

**Note:** Text in `[brackets]` are placeholders - replace with actual content. Text in `<angle brackets>` are guidelines - do NOT include in your final protocol. Instructions that go in the final template, are not included in either.

```markdown
# [Protocol Name]

**Type:** Protocol

[Brief one-sentence description of what this protocol does.]

---

## Goal

[What this protocol achieves — the end state it produces. One clear sentence.]

---

## Intent

[Why this protocol exists — what problem it solves or prevents. 2-3 sentences explaining the fundamental problem and why this matters.]

---

## Scope

**Addresses:** [What's included - specific areas, concerns, or contexts this protocol covers]

**Does not address:** [What's explicitly excluded - related but out of scope]

---

## Core Concepts

[Keep this light. Prefer evaluation criteria over verbose descriptions.]

**Best format - Table with evaluation criteria:**

| Concept | Check for strength | Check for excess |
|---------|-------------------|------------------|
| [Name] | [How to verify it's working] | [How to detect too much] |

**Alternative - Brief bulleted principles (2-3 sentences max each)**

**Avoid:** Long explanations of what concepts mean. Focus on what to check, not what to know.

---

## Techniques

[Instructions for how to do it + criteria for evaluating success.]

**[Technique name]:**

[Brief instruction on how to apply]

1. Step 1 - [action]
2. Step 2 - [action]
3. Step 3 - [action]

**Evaluation criteria:**
- [Question or check 1]
- [Question or check 2]

---

## Use Cases

[Specific contexts where this protocol is relevant]

- [Context 1]
- [Context 2]
- [Context 3]

---

## Patterns and Anti-Patterns <OPTIONAL>

<Only include if truly needed. Keep examples concise.>

### Patterns (✅)

**[Pattern name]:** [What it is, when to use, brief example]

### Anti-Patterns (❌)

**[Anti-pattern name]:** [What it is, why it fails, what to do instead]

---

## Examples <OPTIONAL>

<Only include if needed to clarify application. Keep concise.>

### Example 1: [Scenario]

**Context:** [Description of situation]

**Application:** [How protocol was applied]

**Result:** [Outcome achieved]

---

## References

<REQUIRED: Research must be completed before creating a protocol.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [protocol topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[protocol topic] techniques" or "[topic] best practices"
- Example: Creating `quality` → search "code quality assessment best practices"
- Example: Creating `risk_management` → search "risk management software engineering">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
