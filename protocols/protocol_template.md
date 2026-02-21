# Protocol Template

This template defines the standard structure for creating new protocols.

---

## Overview

Use this template when creating a new protocol. Protocols are standardized processes or patterns that multiple primitives and workflows follow.

**When to create a protocol:**
- Pattern is used by multiple primitives/workflows
- Process needs standardization across the framework
- Quality concern needs systematic approach

**When NOT to create a protocol:**
- Used by only one primitive/workflow (put it there instead)
- Too specific to a use case (belongs in a skill or guideline)
- No clear process to define (needs research first)

---

## Standard Protocol Structure

Protocols capture **essential principles and patterns**. They distill expert knowledge into reusable concepts.

```markdown
# [Protocol Name]

Brief one-sentence description of what this protocol does.

---

## Goal

What this protocol achieves — the end state it produces.

One clear sentence.

---

## Intent

Why this protocol exists — what problem it solves or prevents.

2-3 sentences explaining the fundamental problem and why this matters.

---

## Scope

What this protocol addresses. Be positive, concise, and precise.

Example formats:
- "This protocol addresses: [X], [Y], and [Z]"
- "This protocol covers [domain/area]: [specific aspects]"
- "This protocol applies to [context]: [key elements]"

Use precise terminology that defines boundaries clearly without negative framing.

---

## Key Results

What outcomes indicate this protocol is being applied successfully.

- Measurable result 1
- Measurable result 2
- Measurable result 3

---

## [Essential Concepts/Dimensions/Criteria]

The core ideas that define this protocol.

This section varies by protocol type:
- For assessment protocols: Criteria or dimensions
- For pattern protocols: Core patterns
- For process protocols: Key phases or stages

Keep this section focused on **what matters**, not **how to do it**.

---

## Processes

How to apply this protocol in practice.

Only include if the protocol requires a specific approach. Keep it high-level — principles, not steps.

---

## Patterns and Anti-Patterns

What works and what doesn't.

### Patterns (✅)

**Pattern 1: [Name]**
- What it is
- When to use
- Why it works
- Example

**Pattern 2: [Name]**
- What it is
- When to use
- Why it works
- Example

### Anti-Patterns (❌)

**Anti-Pattern 1: [Name]**
- What it is
- Why it fails
- What to do instead
- Example

**Anti-Pattern 2: [Name]**
- What it is
- Why it fails
- What to do instead
- Example

---

## Examples

Concrete scenarios showing the protocol in action.

### Example 1: [Scenario]

**Context:** ...

**Application:** ...

**Result:** ...

### Example 2: [Scenario]

**Context:** ...

**Application:** ...

**Result:** ...

---

## Key Takeaways

Essential insights from this protocol.

- Takeaway 1
- Takeaway 2
- Takeaway 3

---

## When to Apply This Protocol

Specific contexts where this protocol is relevant.

- Context 1
- Context 2
- Context 3
```

---

## Protocol Structure Guidelines

### Required Sections

Every protocol MUST include:

1. **Title and one-sentence description**
   - Clear, concise statement of what this is

2. **Goal**
   - One sentence: what this protocol achieves
   - Outcome-focused, not process-focused

3. **Intent**
   - 2-3 sentences: why this exists, what problem it solves
   - The fundamental motivation

4. **Scope**
   - What this protocol addresses
   - Positive, concise, precise terminology
   - Defines boundaries clearly through what it covers

5. **Key Results**
   - Measurable outcomes that indicate successful application
   - 3-5 bullets

6. **Essential Concepts** (name varies by protocol)
   - The core ideas, criteria, dimensions, or patterns
   - Focus on **what matters**, not how to do it
   - This is the heart of the protocol

7. **Patterns and Anti-Patterns**
   - What works (✅) and what doesn't (❌)
   - With examples showing why

8. **Examples**
   - Concrete scenarios demonstrating the protocol
   - Context → Application → Result

9. **Key Takeaways**
   - Essential insights distilled
   - 3-5 bullets

10. **When to Apply This Protocol**
    - Specific contexts where this is relevant
    - Integration with primitives/workflows

### Recommended Sections

Include if applicable:

1. **Processes**
   - High-level approach, not step-by-step instructions
   - Only if the protocol requires a specific methodology
   - Keep it principle-based, not procedural

### Optional Sections

Include rarely:

1. **Background** — If essential context exists
2. **References** — If based on external research

---

## Writing Guidelines

### Essence Over Procedure

Protocols capture expert knowledge for **skills and workflows** to reference. They provide foundational principles and patterns that skills apply during execution.

**Protocols are essential knowledge.** They explain what matters and why, enabling skills to make informed decisions.

**Good protocol:**
- Distills expert insight into essential concepts
- Provides patterns that apply across contexts
- Explains **what makes X work** and **why it matters**
- Written for practitioners who need to understand principles

**Bad protocol:**
- Reads like a procedural checklist
- Overly prescriptive steps
- Focuses on "do step 1, 2, 3" instead of underlying principles
- Superficial coverage without depth

### Goal Section

One sentence, outcome-focused.

✅ **Good:**
- "Enable assessment of system modularity across multiple dimensions"
- "Prevent documentation drift through systematic update practices"

❌ **Bad:**
- "Help with modularity stuff"
- "Make code better"

### Intent Section

2-3 sentences explaining why this exists and what problem it solves.

✅ **Good:**
- "Systems with poor modularity are hard to understand, test, and modify. Without clear criteria, teams cannot assess modularity objectively or identify improvements. This protocol provides a framework for evaluating modularity across six essential dimensions."

❌ **Bad:**
- "Make things better"
- "Help developers write good code"

### Scope Section

What this protocol addresses. Use **positive, concise, precise** terminology.

✅ **Good:**
- "This protocol addresses checklist structure, dynamic modification during execution, and progress tracking"
- "This protocol covers modularity assessment: cohesion, coupling, information hiding, interface design, dependency direction, and size"
- "This protocol applies to documentation maintenance: synchronization timing, update scope, and drift prevention"

❌ **Bad (negative framing):**
- "This protocol covers checklists. It does NOT cover validation or workflow design."
- "This is about modularity. It does NOT cover performance or security."

**Why positive framing?** Forces precise terminology. Saying what you cover requires naming specific concepts. Saying what you don't cover is vague and unbounded.

### Essential Concepts Section

Focus on **what matters**, not **how to do it**.

**Good:**
- Define core dimensions, criteria, or patterns
- Explain why each matters
- Provide conceptual clarity

**Bad:**
- Step-by-step procedures
- Overly prescriptive instructions
- Surface-level coverage

### Examples

**Use realistic scenarios:**
- Draw from actual use cases
- Show before/after when relevant
- Include context and results

**Keep examples focused:**
- One concept per example
- Short enough to grasp quickly
- Clear what's being demonstrated

### Patterns and Anti-Patterns

**Patterns (✅):**
- When to use
- How to apply
- Why it works
- Example

**Anti-patterns (❌):**
- What not to do
- Why it's problematic
- What to do instead
- Example

---

## Protocol Checklist

Before finalizing a protocol, verify:

- [ ] **Title is clear**: One sentence description exists
- [ ] **Purpose is explicit**: Goal, Intent, Scope all defined
- [ ] **Content is actionable**: Clear guidance, not just philosophy
- [ ] **Examples are concrete**: Real scenarios, not abstract
- [ ] **Boundaries are clear**: Scope says what's included AND excluded
- [ ] **Integration is addressed**: How this relates to other components
- [ ] **Summary is useful**: Key takeaways clearly stated
- [ ] **References are listed**: When to use this protocol is explicit

---

## Adding Protocol to Framework

After creating a protocol:

1. **Add to protocols/README.md:**
   - Add entry under appropriate category
   - Update "When to Use" section
   - Update "Protocol Relationships" diagram

2. **Reference from relevant components:**
   - Add to primitives that use it
   - Add to workflows that use it
   - Add to other protocols that relate to it

3. **Update documentation:**
   - Add to CLAUDE.md if it's a core pattern
   - Update setup guidelines if it affects user setup

4. **Test the protocol:**
   - Ensure it's actually useful
   - Verify examples work
   - Check that guidance is clear

---

## Example: Checklist Management Protocol

See `protocols/checklist_management.md` for a complete example following this template.

**Key characteristics:**
- Clear purpose (Goal/Intent/Scope)
- Concrete patterns (dynamic items, conditionals, loop-backs)
- Examples showing application
- Anti-patterns to avoid
- Integration with tracking protocol
- Summary with key takeaways

---

## Common Mistakes

### ❌ Mistake 1: Vague Purpose

**Bad:**
```markdown
**Goal:** Make checklists better
**Intent:** Help with workflow stuff
**Scope:** Checklists
```

**Good:**
```markdown
**Goal:** Enable workflows to track progress reliably and adapt to discovered needs
**Intent:** Prevent workflows from skipping steps, losing track, or becoming rigid
**Scope:** Covers checklist structure, dynamic modification, progress tracking. Does NOT cover validation logic or workflow design.
```

### ❌ Mistake 2: No Examples

**Bad:** Long philosophical discussion with no concrete examples

**Good:** Clear examples showing the protocol in action

### ❌ Mistake 3: Unclear Scope

**Bad:** "This protocol is about X" (but what about Y and Z?)

**Good:** "This protocol covers X and Y. It does NOT cover Z (see protocol-z.md)"

### ❌ Mistake 4: No Integration Guidance

**Bad:** Protocol exists in isolation

**Good:** Explicitly states when to use it, what it relates to, how it integrates

---

## Summary

**Protocol structure:**
- Title + one-sentence description
- Overview with key principles
- Purpose (Goal, Intent, Scope)
- Main content (process, guidelines, patterns)
- Examples and anti-patterns
- Integration with other components
- Summary with key takeaways

**Writing guidelines:**
- Be specific (concrete examples)
- Be actionable (clear guidance)
- Be complete (cover common cases)
- Be explicit (state boundaries)

**After creating:**
- Add to protocols/README.md
- Reference from relevant components
- Update documentation
- Test that it's useful

**Reference this template:**
- When creating new protocols
- When reviewing existing protocols
- When standardizing a pattern across primitives/workflows
