# Protocol Creation Guideline

This guideline explains how to create protocols for the skills-and-agents framework.

---

## Table of Contents

- [Protocol Creation Guideline](#protocol-creation-guideline)
  - [Table of Contents](#table-of-contents)
  - [What is a Protocol?](#what-is-a-protocol)
  - [Steps](#steps)
  - [Writing Style](#writing-style)
  - [Validation Criteria](#validation-criteria)
  - [Tips](#tips)
  - [Template](#template)

---

## What is a Protocol?

A protocol is a **standardized process or pattern** that multiple primitives and workflows follow. Protocols capture essential principles and patterns, distilling expert knowledge into reusable concepts.

**Examples of protocols:**
- `tracking_and_recovery.md` - Progress tracking and resumption
- `software_quality.md` - Quality assessment dimensions
- `thinking.md` - Reasoning and thinking techniques
- `transparency.md` - Making work visible and traceable

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

## Steps

Follow these steps to create and validate a protocol.

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
1. **Action-oriented language** - Use imperatives and concrete actions, not vague descriptions
2. **Technique tables** - 7-column format with When/Why/How/Output/Validations (see template)
3. **"Why" uses infinitive** - Must start with "To" (e.g., "To ensure correct application...")
4. **"How" is executable** - Comma-separated actions that can be performed
5. **Validations as questions** - "Are premises valid?" not "Premises are valid"
6. **Exit conditions separate** - At bottom of patterns, not inline with steps
7. **Minimal descriptions** - Goal/Intent/Scope should be brief (2-3 sentences max)

**Content pattern:**
1. **Description (light)** - Tagline, Outline, Goal, Intent, Scope for context
2. **Artifacts and Outputs (optional)** - Define nouns that techniques reference
3. **Technique Tables** - Core Approaches with 7-column tables (Output/Artifact column links to artifacts)
4. **Example Patterns (optional)** - Step sequences with Exit Conditions sections
5. **References** - Sources consulted during research

**Structural differences from primitives:**
- 100-150 lines (150-180 acceptable with user permission)
- Technique tables use 7-column format
- Example patterns (not exhaustive)
- No Key Results (protocols aren't invoked directly)
- Exit conditions in separate section, not inline

---

## Validation Criteria

For each criterion, you MUST output all evidence that it passes and all evidence that it fails. Once evidence is output, weigh the evidence to decide if it passes or fails.

**Primary Criteria:**
- [ ] **Clear when to use protocol:** Goal/Intent/Scope make it obvious when this protocol applies
- [ ] **Clear how to use protocol:** Instructions are executable, not vague or philosophical
- [ ] **Clear when to use each technique:** "When" column provides concrete, actionable triggers
- [ ] **Clear how to execute each technique:** "How" column contains executable actions

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

[Brief one-sentence description of what this protocol does.]

[Tagline: action-oriented statement, e.g., "Apply techniques to think effectively."]

---

## Outline

<Lightweight table of contents for navigation. Single line with links.>

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts](#artifacts-and-outputs) — [list key artifacts]
- [Core Approaches](#core-approaches) — [list categories]
- [Example Patterns](#example-patterns) — [list patterns]

---

## Goal

[What this protocol achieves — direct statement without justification. One clear sentence.]

---

## Intent

[Why this protocol exists — what problem it solves or prevents. 2-3 sentences explaining the fundamental problem and why this matters.]

---

## Scope

[General statement of what this protocol covers — reasoning methods, approaches, patterns, etc. No "Does not address" section.]

---

## Artifacts and Outputs <OPTIONAL>

<Include when protocol defines nouns (artifacts, outputs, templates) that techniques reference.
Techniques should reference these artifacts rather than defining them inline.>

| Artifact/Output | Purpose | When to Use |
|-----------------|---------|-------------|
| **[Name]** | [What it is and does] | [When to use it] |

<If protocol has templates (e.g., checklist format), include here with guidance:>

**[Template Name]:** `[format]`

| Principle | Description | Good | Bad |
|-----------|-------------|------|-----|
| **[Principle]** | [What to do] | [Good example] | [Bad example] |

---

## Core Approaches

<If protocol has different categories of techniques, use subsections. Otherwise, use single table.>

### [Category Name]

[One sentence describing when to use this category.]

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **[Name]** | [Concrete trigger - when to use this] | To [achieve specific benefit/goal] | [Comma-separated executable actions] | [Artifact name from Artifacts and Outputs section, or — if none] | [Question checking correct application?] | [Question checking for misuse?] |

<Guidelines for table columns:>
- **When:** Concrete, actionable trigger (not vague like "challenging assumptions")
- **Why:** MUST start with "To" (infinitive form)
- **How:** Executable actions separated by commas (method/procedure)
- **Output/Artifact:** Deliverable produced; reference artifacts from Artifacts and Outputs section, use "—" if technique produces no artifact
- **Positive Validation:** Question format checking if technique was applied correctly
- **Negative Validation:** Question format checking for common misuse

---

## Example [Pattern/Technique Name] <OPTIONAL>

<Only include if protocol needs example step sequences. These are EXAMPLES, not exhaustive.
Title: "Example Thinking Patterns" or "Example Patterns">

[Brief description: "Structured example sequences of steps for different contexts. Far more are possible."]

### [Pattern Name]

Use when [specific context/trigger].

1. [Step 1]
2. [Step 2]
3. [Step 3]
4. [Step 4 with inline exit: → stop if X, continue if Y]
5. [Step 5]

**Exit Conditions:**
- [Condition 1] → stop, [what to do]
- [Condition 2] → stop, [what to do]
- [Condition 3] → stop, [what to do]

<Guidelines for patterns:>
- Numbered steps (main success path)
- Exit conditions in separate section at bottom (not inline)
- Exit conditions specify what to do (stop, request info, escalate, etc.)
- Patterns are examples, not exhaustive

---

## References

<REQUIRED: Research must be completed before creating a protocol.

Step 1 - Claude Code's documentation FIRST:
- Search: `site:claude.ai OR site:docs.anthropic.com [protocol topic] best practices`
- These guidelines take precedence

Step 2 - Expert/academic sources (in addition):
- Search: "[protocol topic] techniques" or "[topic] best practices"
- Example: Creating `quality` → search "code quality assessment best practices"
- Example: Creating `thinking` → search "reasoning techniques cognitive science">

**List ALL sources consulted:**
- [Claude Code documentation URL and topic]
- [Academic paper/source title and URL]
- [Expert framework/source title and URL]
```
