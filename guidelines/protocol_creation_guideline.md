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
- Protocol provides non-obvious constraints
- Process needs standardization across the framework
- Quality concern needs systematic approach

**Don't create a protocol when:**
- Used by only one primitive/workflow (put it there instead)
- Too specific to a use case (belongs in a skill or guideline)
- No clear process to define (needs research first)

In the template, sections are required [REQUIRED], recommended [RECOMMENDED], or optional [OPTIONAL]. Only provide optional sections when really needed to use the protocol

---

## Protocol Structure

Protocols are **instruction-focused**, not descriptive essays. Follow this content pattern:

1. **Description (light)** - Goal/Intent/Scope for context
2. **Instructions** - How to do it (actionable steps in Techniques)
3. **Criteria** - How to evaluate success (in Core Concepts and/or Techniques)
4. **Patterns** - Examples showing it in action

See `protocols/instructions.md` for writing effective instruction-focused content.

---

## Protocol Template

```markdown
# [Protocol Name]

Brief one-sentence description of what this protocol does.

---

## Goal [REQUIRED]

What this protocol achieves — the end state it produces.

[One clear sentence.]

---

## Intent

Why this protocol exists — what problem it solves or prevents.

[2-3 sentences explaining the fundamental problem and why this matters.]

---

## Scope

**Addresses:** [What's included - specific areas, concerns, or contexts this protocol covers]

**Does not address:** [What's explicitly excluded - related but out of scope]

---

## Core Concepts

**Keep this light.** Prefer evaluation criteria over verbose descriptions.

**Best format:** Table with evaluation criteria

```markdown
| Concept | Check for strength | Check for excess |
|---------|-------------------|------------------|
| [Name] | [How to verify it's working] | [How to detect too much] |
```

**Alternative:** Brief bulleted principles (2-3 sentences max each)

**Avoid:** Long explanations of what concepts mean. Focus on what to check, not what to know.

---

## Techniques

**Instructions for how to do it + criteria for evaluating success.**

**Include:**
1. **Process steps** - Numbered instructions (1. Do X, 2. Do Y, 3. Do Z)
2. **Decision guidance** - How to make choices ("Make X explicit", "Hide Y", "Establish Z")
3. **Evaluation criteria** - How to assess if done correctly (checklist or questions)

**Format example:**

```markdown
**[Technique name]:**

[Brief instruction on how to apply]

1. Step 1 - [action]
2. Step 2 - [action]
3. Step 3 - [action]

**Evaluation criteria:**
- [Question or check 1]
- [Question or check 2]
```

**Avoid:** Vague platitudes like "make good decisions" or "coupling that makes sense is good." Be specific and actionable.

---

## Use Cases and Triggers

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

## Validation

For each criterion, you MUST output all evidence that it passes and all evidence that it fails. Once evidence is output, weight the evidence to decide if it passes or fails. Only validate one prototocol at a time. 

- [ ] **Clear:** Text is explicit and to the point. Each section communicates its purpose directly.
- [ ] **Actionable:** Provides clear guidance with specific techniques, not vague philosophy.
- [ ] **Concrete examples:** Real scenarios with context, application, and results (not abstract).
- [ ] **Clear boundaries:** Scope explicitly states what's included and excluded.
- [ ] **Integration addressed:** Explains how this relates to other protocols/components.
- [ ] **Coherent:** Sections flow logically without contradictions.
- [ ] **No redundancy:** Each piece of information appears exactly once.
- [ ] **Complete:** All necessary information provided, nothing critical missing.
- [ ] **Concise:** Concepts explained in minimum necessary words.
- [ ] **Precise:** Specific, unambiguous language. No vague terms.
- [ ] **LLM-focused:** Contains informative, non-obvious constraints. No needless definitions.
- [ ] **Follows template:** Only contains sections listed in template.
- [ ] **Valuable:** All sections add clear value. 

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
**Addresses:** All aspects of software development quality
**Does not address:** N/A
```
*Protocols should be focused on specific patterns or processes.*

✅ **Focused scope**
```markdown
## Scope
**Addresses:** Code quality assessment (correctness, clarity, maintainability, convention adherence)
**Does not address:** Performance benchmarking, security audits, deployment processes
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

❌ **Vague platitudes**
```markdown
## Techniques
- Coupling that makes sense is good
- Make sure cohesion is appropriate
- Balance all the objectives
```
*These provide no actionable guidance.*

✅ **Specific, actionable instructions**
```markdown
## Techniques
- Make dependencies explicit and depend on stable interfaces, not implementation details
- Group responsibilities that change together into same component
- Evaluate: Which objectives does this design optimize? Which does it sacrifice? Is that appropriate?
```

❌ **Used by only one skill**
```markdown
# Specific Feature Implementation Protocol
[Only used by feature_workflow.md]
```
*If only one skill uses it, put the content in that skill instead.*

❌ **Missing "When to Apply" - unclear triggers**
```markdown
## Use Cases
- Documentation maintenance
- Code reviews
- API changes
```
*Use Cases show scenarios, but don't show when to trigger the protocol.*

✅ **Clear triggering conditions**
```markdown
## Use Cases
- Documentation maintenance
- Code reviews
- API changes

## When to Apply
- When implementing code (`implement` primitive) - update docs in same commit
- During code review (`critique` primitive) - verify docs match code
- When changing APIs - update all affected docstrings
```

---

## Tips for Writing Good Protocols

1. **Start with the problem:** What inconsistency or gap does this protocol address?
2. **Keep it focused:** One protocol per pattern or process
3. **Make it actionable:** Include specific techniques, not just concepts
4. **Show real examples:** Concrete scenarios from actual use
5. **Test with users:** Verify the protocol is actually useful
6. **Reference, don't repeat:** Link to related protocols instead of duplicating content
