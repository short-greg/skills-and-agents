# Create Primitive Protocol

This protocol defines how to create new primitives for the skills-and-agents framework.

---

## What is a Primitive

A primitive is an atomic cognitive action — a single, well-defined unit of reasoning that can be composed into larger workflows. Primitives are:

- **Atomic**: They do one thing well
- **Composable**: They can be sequenced into workflows
- **Self-contained**: Their scope does not reference other primitives
- **Goal-oriented**: They have clear preconditions and postconditions
- **Enforceable**: Expert Reasoning (first) and Confirm (last) ensure quality

---

## When to Create a Primitive

Create a new primitive when:

1. A recurring cognitive action appears across multiple workflows
2. The action has clear preconditions and postconditions
3. The action is atomic — it cannot be meaningfully decomposed further
4. The action is distinct from existing primitives

Do NOT create a primitive when:

- The action is a variation of an existing primitive (extend the existing one)
- The action is a workflow (sequence of primitives)
- The action is tool-specific (belongs in a skill, not a primitive)

---

## Primitive Categories

Primitives fall into categories based on their purpose:

| Category | Purpose | Examples |
|----------|---------|----------|
| Understanding | Grasping current state or requirements | orient, define, investigate |
| Planning | Deciding what to do | plan, design, brainstorm |
| Execution | Doing the work | implement |
| Verification | Checking the work | validate, critique |
| Maintenance | Keeping things healthy | bookkeep, upkeep |

When creating a primitive, identify which category it belongs to. This helps ensure the primitive is distinct from existing ones.

---

## Writing Guidelines

### Scope

The scope is the most critical section — it defines what the primitive does and doesn't do.

**Requirements:**
- **Concise**: One paragraph, typically 2-4 sentences
- **Precise**: Specific enough that two people would agree on whether an action falls within scope
- **Complete**: Covers all activities the primitive handles
- **Non-overlapping**: Minimizes overlap with other primitives; where overlap exists, the distinction must be clear

**How to write:**
1. Start with the core activity in one phrase (e.g., "Determining whether work is done")
2. Follow with "Includes:" and list the specific activities covered
3. End with a distinguishing statement that clarifies what question this primitive answers

**Pattern:**
```
**Scope:** [Core activity phrase]. Includes: [activity 1], [activity 2], [activity 3], and [activity 4]. [Primitive name] is [type of action] — it answers "[question it answers]" not "[question it doesn't answer]".
```

**Example (validate):**
> **Scope:** Determining whether work is done by verifying outputs against SMARB success criteria. Includes: running tests and checking results, verifying behavior against acceptance criteria, checking edge cases and error handling, confirming integration works, and producing a binary pass/fail verdict for each criterion. Validation is objective verification — it answers "does this meet the specified criteria?" not "is this good quality?"

**Anti-patterns:**
- ❌ "Does not cover X" — state positively what IS covered
- ❌ "Unlike primitive Y, this primitive..." — no primitive references
- ❌ Vague activities like "handles various tasks"
- ❌ Multi-paragraph scope — keep it to one focused paragraph

### Goal

One sentence stating the outcome. Not the process, not the intent — the deliverable.

**Pattern:** `[Verb] [what is produced] [qualifying phrase]`

**Examples:**
- "Produce a binary pass/fail verdict on whether work is complete"
- "Establish when work is done — what needs to be built, what success looks like"
- "Identify issues, weaknesses, and opportunities for improvement"

### Intent

One sentence explaining why this primitive exists — what problem it prevents.

**Pattern:** `Prevent [bad outcome]. [Elaboration on the consequence]`

**Examples:**
- "Prevent shipping broken or incomplete work."
- "Prevent wasted effort by ensuring the task is well-understood before any solution is attempted."

### Description (Frontmatter)

Must enable correct primitive selection. Include:
- When to use (trigger condition)
- Natural language triggers (phrases users say)

**Pattern:**
```yaml
description: >
  Use when [situation]. Triggers on: "[phrase 1]", "[phrase 2]", "[phrase 3]".
```

The triggers should cover variations — formal ("validate this"), casual ("does this work"), and domain-specific ("QA this").

### Key Results

Each key result must be:
- **Binary**: Checkable as pass/fail, not "partially met"
- **Verifiable**: Can be confirmed with evidence
- **Independent**: Each result can be checked separately

**Anti-patterns:**
- ❌ "Good quality code" — not verifiable
- ❌ "Reasonably complete" — not binary
- ❌ "User is satisfied" — not independently verifiable

### Possible Actions

Actions are options, not steps. Group by purpose and explain each.

**Pattern:**
```
**[Category]** (optional: purpose of this category)
- **[action name]**: [what it does] — [when useful / what failure it prevents]
```

The "when useful / what failure it prevents" part is essential — it helps the LLM decide whether to use the action.

---

## Required Sections

Every primitive must have these sections (use `skill_template.md` as the base):

### Frontmatter

```yaml
---
name: primitive-name
description: >
  Use when [trigger condition]. Triggers on: [natural language triggers].
argument-hint: "[what argument is expected]"
disable-model-invocation: false
user-invocable: true
allowed-tools: [tools this primitive needs]
---
```

**Notes:**
- `description` must include natural language triggers — phrases users actually say
- `allowed-tools` should be minimal — only what the primitive genuinely needs
- `disable-model-invocation: true` for primitives with side effects (e.g., implement)

### Goal, Intent, Scope

```markdown
**Goal:** [What this primitive achieves — one sentence, states the outcome.]

**Intent:** [Why this primitive exists — what problem it prevents.]

**Scope:** [Detailed description of what is covered. State positively. Do NOT reference other primitives — the scope must be self-contained.]
```

**Scope rules:**
- State what IS covered, not what is NOT covered
- Be specific about activities, artifacts, and concerns
- Do not reference other primitives by name
- If comparing to another primitive, describe the distinction in terms of what each does, not by name

### Preconditions

```markdown
**Must be provided:**
- [condition]: [how to get it if missing — typically "ask if not clear"]

**Self-satisfiable:**
- [condition]: [how the primitive will gather it]

**Non-essential:**
- [condition]: [what it enables if present]
```

**Notes:**
- "Must be provided" = primitive cannot proceed without this, must ask user
- "Self-satisfiable" = primitive can gather this itself (read files, search, etc.)
- "Non-essential" = primitive works without, but better with

### Postconditions

```markdown
**Success:**
- [state that will be true when primitive succeeds]

**Failure:**
- [condition under which primitive fails — be specific about why]
```

**Notes:**
- Success postconditions should be verifiable
- Failure conditions help the LLM know when to stop and report failure

### Key Results

```markdown
1. [Verifiable outcome — should be checkable as pass/fail]
2. [Verifiable outcome]
...
```

**Notes:**
- Key results are used in Confirm to verify the primitive succeeded
- Each should be binary (pass/fail), not fuzzy
- For primitives that produce definitions, key results should mention SMARB criteria

### Required Actions

```markdown
**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in [this domain] would approach [this task].
Cover: their strategy, what they would do first and why, alternatives they would consider,
fallbacks if the primary approach fails, common mistakes to avoid, and what a high-quality
[outcome] looks like for this type of [task].
This is not [doing the task] — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**
Before declaring done, verify against each key result:
- [Check for key result 1]
- [Check for key result 2]
...

Report outcome explicitly: state whether the [primitive] succeeded or failed, and why.
```

**Notes:**
- Expert Reasoning prevents rushing into poor approaches
- Confirm prevents declaring success without verification
- Both are required for ALL primitives

### Possible Actions

```markdown
**[Category]**
- **[action name]**: [what it does] — [when useful / what failure it prevents]

**[Category]**
- **[action name]**: [what it does] — [when useful / what failure it prevents]
```

**Notes:**
- Group actions by category (Preparation, Analysis, Execution, etc.)
- Each action should explain both what it does AND when/why to use it
- "Select and sequence based on context and expert reasoning" — actions are not rigid steps

### Use Cases

```markdown
- [Primary use case]
- [Secondary use case]
```

### Tools and Hooks

```markdown
## Tools
[List of tools, or "None"]

## Hooks
[List of hooks, or "None"]
```

---

## Quality Checklist

Before finalizing a primitive, verify:

- [ ] **Distinct**: Primitive is meaningfully different from existing primitives
- [ ] **Atomic**: Primitive cannot be meaningfully decomposed further
- [ ] **Self-contained**: Scope does not reference other primitives by name
- [ ] **Positive scope**: Scope states what IS covered, not what is NOT
- [ ] **Clear triggers**: Description includes natural language phrases users would say
- [ ] **Minimal tools**: Only tools genuinely needed are listed
- [ ] **Verifiable key results**: Each key result is checkable as pass/fail
- [ ] **Expert Reasoning present**: Required Actions includes Expert Reasoning first
- [ ] **Confirm present**: Required Actions includes Confirm last
- [ ] **Actions categorized**: Possible Actions are grouped by purpose
- [ ] **Actions explained**: Each action explains what AND when/why

---

## Process

1. **Identify the gap**: What cognitive action is missing from the current primitives?
2. **Check for overlap**: Is this genuinely distinct from existing primitives, or a variation?
3. **Determine category**: Understanding, Planning, Execution, Verification, or Maintenance?
4. **Draft using template**: Copy `skill_template.md` and fill in all sections
5. **Write scope carefully**: Positive, detailed, self-contained, no primitive references
6. **Define key results**: Binary, verifiable outcomes
7. **Add Expert Reasoning and Confirm**: Required for all primitives
8. **Categorize actions**: Group by purpose, explain each
9. **Run quality checklist**: Verify all criteria before finalizing
10. **Review with user**: Confirm the primitive is accurate and useful

---

## Checking for Overlap

Before creating a new primitive, verify it doesn't overlap with existing ones:

| If your primitive... | Consider instead... |
|---------------------|---------------------|
| Gathers information to understand something | investigate (if uncertain), orient (if assessing current state) |
| Establishes what to build | define |
| Figures out how to build | design (technical), plan (sequencing) |
| Generates options | brainstorm |
| Writes code or artifacts | implement |
| Checks if work is done | validate |
| Reviews for quality | critique |
| Updates documentation | bookkeep |
| Cleans up or maintains | upkeep |

**Legitimate new primitives** fill gaps in this coverage. If your primitive overlaps significantly with an existing one, consider:
1. Extending the existing primitive's scope
2. Adding actions to the existing primitive
3. Whether the distinction is meaningful enough to justify a separate primitive

**The test:** Can you explain in one sentence what question your primitive answers that no existing primitive answers?

---

## Common Mistakes

- **Scope references other primitives**: Say what this primitive does, not what others do
- **Scope is negative**: Say what IS covered, not what is NOT
- **Scope overlaps significantly**: If >50% of activities overlap with another primitive, reconsider
- **Key results are fuzzy**: "Good quality" is not verifiable; "All tests pass" is
- **Actions are rigid steps**: Actions are options, not a fixed sequence
- **Missing Expert Reasoning**: Every primitive needs this to prevent poor approaches
- **Missing Confirm**: Every primitive needs this to verify success
- **Too many tools**: Only include tools the primitive genuinely needs
- **Not atomic**: If the primitive requires multiple distinct cognitive actions, it's a workflow
