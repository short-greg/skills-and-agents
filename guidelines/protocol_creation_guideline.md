# Protocol Creation Guideline

This guideline explains how to create protocols for the skills-and-agents framework.

## What is a Protocol?

A protocol is a **standardized process or pattern** that multiple primitives and workflows follow. Protocols capture essential principles and patterns, distilling expert knowledge into reusable concepts.

**Examples of protocols:**
- `tracking.md` - Progress documentation standards
- `recovery.md` - Resuming from interruption
- `quality.md` - Quality assessment dimensions
- `risk_management.md` - Risk identification and mitigation

---

## When to Create a Protocol

Create a protocol when:
- Pattern is used by multiple primitives/workflows
- Process needs standardization across the framework
- Quality concern needs systematic approach

**Don't create a protocol when:**
- Used by only one primitive/workflow (put it there instead)
- Too specific to a use case (belongs in a skill or guideline)
- No clear process to define (needs research first)

---

## Protocol Structure

Protocols capture essential principles and patterns. They focus on WHAT matters and HOW to do it.

### Required Sections
- **Goal** - What this protocol achieves
- **Intent** - Why this protocol exists
- **Scope** - What this protocol addresses
- **Core Concept** - The key ideas that define this protocol
- **Techniques** - How to apply this protocol
- **Use Cases** - Specific contexts where relevant

### Optional Sections
- **Patterns and Anti-Patterns** - What works and what doesn't
- **Examples** - Concrete scenarios showing protocol in action
- **References** - Related resources or protocols

---

## Protocol Template

```markdown
# [Protocol Name]

Brief one-sentence description of what this protocol does.

---

## Goal

What this protocol achieves — the end state it produces.

[One clear sentence.]

---

## Intent

Why this protocol exists — what problem it solves or prevents.

[2-3 sentences explaining the fundamental problem and why this matters.]

---

## Scope

What this protocol addresses. Be positive, concise, and precise.

[Example formats:
- "This protocol addresses: [X], [Y], and [Z]"
- "This protocol covers [domain/area]: [specific aspects]"
- "This protocol applies to [context]: [key elements]"

Use precise terminology that defines boundaries clearly without negative framing.]

---

## Core Concept

The core ideas that define this protocol.

[This section varies by protocol type:
- For assessment protocols: Criteria or dimensions
- For pattern protocols: Core patterns
- For process protocols: Key phases or stages

Keep focused on **what matters**, not **how to do it**.]

---

## Techniques

How to apply this protocol - the practical methods and approaches.

[One-line descriptions of techniques used for this protocol. Describes how to do it.]

---

## Use Cases

Specific contexts where this protocol is relevant.

- Context 1
- Context 2
- Context 3

---

## Patterns and Anti-Patterns [OPTIONAL]

What works and what doesn't.

### Patterns (✅)

**Pattern 1: [Name]**
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

---

## Examples [OPTIONAL]

Concrete scenarios showing the protocol in action.

### Example 1: [Scenario]

**Context:** [Description of situation]

**Application:** [How protocol was applied]

**Result:** [Outcome achieved]

---

## References [OPTIONAL]

Related protocols, resources, or background material.

- [Reference with reason why it's relevant]
```

---

## Validation Criteria

Before finalizing a protocol, verify:

- [ ] **Clear and concise:** Text is explicit and to the point
- [ ] **Actionable:** Provides clear guidance, not just philosophy
- [ ] **Concrete examples:** Real scenarios, not abstract descriptions
- [ ] **Clear boundaries:** Scope defines what's included
- [ ] **Integration addressed:** How this relates to other protocols/components
- [ ] **Coherent:** Logic flows naturally, no contradictions
- [ ] **LLM-focused:** No unnecessary explanations for LLM

---

## Validation Criteria Explained

### 1. Clear and Concise
Protocol text must be explicit and to the point. Avoid verbosity. Each section should communicate its purpose directly.

### 2. Actionable
Protocols must provide clear guidance that can be followed, not just philosophical statements. Include specific techniques and approaches.

**Anti-pattern:** Vague philosophy
```markdown
## Core Concept
Quality is important and should be considered carefully.
```

✅ **Correct: Actionable guidance**
```markdown
## Core Concept
Quality dimensions: correctness, clarity, maintainability, performance, security.
```

### 3. Concrete Examples
Examples must show real scenarios with specific context, application, and results. Avoid abstract or hypothetical examples.

### 4. Clear Boundaries
Scope section must clearly define what the protocol covers. Use positive framing (what IS covered) rather than extensive lists of exclusions.

### 5. Integration Addressed
Protocol should explain how it relates to other protocols or components in the framework. References section can elaborate on this.

### 6. Coherent
All sections must flow logically. Core Concept explains what matters, Techniques explain how to do it, Use Cases show where to apply it.

### 7. LLM-focused
Only include information necessary for the LLM to understand and apply the protocol. Avoid background context the LLM already knows.

---

## Adding Protocol to Framework

After creating a protocol:

1. **Add to protocols/README.md:**
   - Add entry under appropriate category
   - Update "When to Use" section
   - Update relationships documentation

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

## Common Protocol Anti-Patterns

❌ **Too broad - trying to cover everything**
```markdown
## Scope
This protocol covers all aspects of software development quality.
```
*Protocols should be focused on specific patterns or processes.*

✅ **Focused scope**
```markdown
## Scope
This protocol addresses code quality assessment: correctness, clarity, maintainability, and adherence to conventions.
```

❌ **No techniques - only philosophy**
```markdown
## Techniques
Think carefully about quality and make good decisions.
```
*Protocols must provide actionable techniques.*

✅ **Actionable techniques**
```markdown
## Techniques
- Review code against quality dimensions systematically
- Categorize issues by severity (critical, non-blocking, suggestion)
- Provide specific improvement recommendations with locations
```

❌ **Used by only one skill**
```markdown
# Specific Feature Implementation Protocol
[Only used by feature_workflow.md]
```
*If only one skill uses it, put the content in that skill instead.*

---

## Tips for Writing Good Protocols

1. **Start with the problem:** What inconsistency or gap does this protocol address?
2. **Keep it focused:** One protocol per pattern or process
3. **Make it actionable:** Include specific techniques, not just concepts
4. **Show real examples:** Concrete scenarios from actual use
5. **Test with users:** Verify the protocol is actually useful
6. **Reference, don't repeat:** Link to related protocols instead of duplicating content
