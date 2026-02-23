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

[COMMENT: Include guidelines within the structure to keep it simpler]

Protocols capture **essential principles and patterns**. They distill expert knowledge into reusable concepts.

```markdown
# [Protocol Name]

Brief one-sentence description of what this protocol does.

---

## Goal

What this protocol achieves — the end state it produces.

<One clear sentence.>

---

## Intent

Why this protocol exists — what problem it solves or prevents.

<2-3 sentences explaining the fundamental problem and why this matters.>

---

## Scope

What this protocol addresses. Be positive, concise, and precise.

<
Example formats:
- "This protocol addresses: [X], [Y], and [Z]"
- "This protocol covers [domain/area]: [specific aspects]"
- "This protocol applies to [context]: [key elements]"

Use precise terminology that defines boundaries clearly without negative framing.
>


## Core Concept

The core ideas that define this protocol.

<
This section varies by protocol type:
- For assessment protocols: Criteria or dimensions
- For pattern protocols: Core patterns
- For process protocols: Key phases or stages

Keep this section focused on **what matters**, not **how to do it**.
>

## Techniques

[COMMENT: Add One line description for techniques used for this protocol, describes how to do it]

<
TODO: Fill in what this section should contain
>

## Use Cases

Specific contexts where this protocol is relevant.

- Context 1
- Context 2
- Context 3

## Patterns and Anti-Patterns [OPTIONAL]

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

## Examples [OPTIONAL]

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

## References [OPTIONAL]

[COMMENT: Describe]

- <List all references with a reason why >

>

```
---

## Protocol Validation

Before finalizing a protocol, verify:

- [ ] **Text is clear, concise, and explicit**: <Describe>
- [ ] **Content is actionable**: Clear guidance, not just philosophy
- [ ] **Examples are concrete**: Real scenarios, not abstract
- [ ] **Boundaries are clear**: Scope says what's included AND excluded
- [ ] **Integration is addressed**: How this relates to other components
- [ ] **Coherent**: The logic for the protocol is coherent.

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
