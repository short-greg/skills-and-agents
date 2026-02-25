# Skill Creation Guideline

This guideline explains how to create skills (primitives and workflows) for the skills-and-agents framework.

## What is a Skill?

A skill is a structured instruction set that guides Claude through a specific cognitive process. Skills ensure consistent, reliable execution by defining clear goals, requirements, and execution steps.

**Two types of skills:**
- **Primitives** - Atomic cognitive actions (single, indivisible operations)
- **Workflows** - Multi-step processes that compose primitives

## Creating a New Skill

**Steps:**
1. Determine skill type (primitive vs workflow)
2. Research best practices (Claude Code first, then ask about expert/academic)
3. Review relevant protocols
4. Interview user to clarify design (if needed)
5. Write skill following template and style guidelines
6. Validate against criteria — if any criterion fails, fix and re-validate until all pass

---

### 1. Determine Skill Type

**Use a Primitive when:**
- The action is atomic (cannot be meaningfully decomposed)
- It answers one specific question or performs one cognitive operation
- It does not require orchestrating multiple other operations

**Use a Workflow when:**
- The process requires multiple steps in sequence
- It orchestrates 2+ primitives to achieve a larger goal
- It has phases or stages

### 2. Research Best Practices (Before Writing)

**Step 1: Search Claude Code's website FIRST**

Always start by searching Claude Code's documentation and best practices:

"Let me first search Claude Code's website for best practices on [skill topic]..."

- Search: "site:claude.ai OR site:docs.anthropic.com [skill topic] best practices"
- Incorporate Claude Code's guidelines into the skill
- Keep these as foundational - they take precedence

**Step 2: Ask user about additional research**

After Claude Code research, ask:

"Would you like me to also search for expert and academic best practices for this [primitive/workflow]? If so, should I search for:
1. General best practices (broader applicability)
2. LLM-specific best practices (AI agent context)
3. Both general and LLM-specific"

**If user says yes:**
1. Web search for academic and expert sources (IN ADDITION to Claude Code guidelines)
   - General: "[topic] best practices academic experts"
   - LLM-specific: "[topic] LLM agents best practices"
2. Incorporate findings into Actions/Steps sections (complementing Claude Code guidelines)
3. Add References section with ALL sources (Claude Code + research)

**This ensures skills reflect Claude Code guidelines PLUS expert knowledge, not just intuition.**

### 3. Review Available Protocols

Before creating a skill, review relevant protocols. These define patterns your skill should follow.

**Available protocols in `protocols/`:**
- **tracking.md** - Progress documentation for multi-step workflows
- **recovery.md** - Resuming workflows after interruption
- **checklists.md** - Dynamic checklists, feedback loops, workflow patterns
- **reasoning.md** - Reasoning and thinking techniques
- **quality.md** - Quality assessment dimensions and criteria
- **goals_and_objectives.md** - Setting objectives with key results (OKRs)
- **risk_management.md** - Managing complexity, uncertainty, and risk
- **documentation.md** - Documentation maintenance patterns
- **modularity.md** - Modular design principles

**Review process:**
1. Read protocols relevant to your skill's characteristics (multi-step? quality gates? complex?)
2. Incorporate their patterns into your skill design
3. Reference used protocols in your skill's Protocols section (if applicable)

### 4. Interview to Clarify Design

After initial research and protocol review, consider interviewing the user to clarify design decisions.

**When to interview:**
- Skill can be implemented multiple ways
- User preferences affect design (verbosity, gates, tool choices)
- Requirements unclear from initial request
- Tradeoffs exist between approaches

**Interview approach:**
- **Dynamic:** Generate questions based on specific skill context (not a fixed checklist)
- **Recommendation-based:** Provide your recommendation with alternatives
- **Trust your intelligence:** No need for example interviews - generate appropriate questions

**Consider these topics (and others as relevant):**
- Workflow type: Deterministic vs adaptive? Imperative vs declarative?
- Quality gates: User approval at checkpoints? Automatic validation only?
- Verbosity: Brief progress updates vs detailed traces?
- Tool integration: Specific tools required? Generic instructions?
- Error handling: Fail-fast vs continue with warnings?
- Degrees of freedom (see below): Where should the skill be flexible vs rigid?

**Degrees of Freedom:**
- **High:** Creative tasks (brainstorming, design options) - give broad guidance
- **Medium:** Standard workflows with preferred patterns - recommend but allow alternatives
- **Low:** Fragile operations (database migrations, deployments) - strict sequence required

### 5. Writing Style

Skills are **action-oriented, instruction-oriented, OR evaluation-oriented** — not descriptive essays.

**Key principles:**
1. **Language style** - Choose based on section:
   - **Action-oriented:** "Identify gaps", "Verify criteria" (imperatives)
   - **Instruction-oriented:** "Read X, then do Y, finally Z" (sequential commands)
   - **Evaluation-oriented:** "Gaps identified?", "Criteria verified?" (checks/questions)
   - **Never descriptive:** Not "The skill identifies..." or "This is the process of..."

2. **Prefer output-oriented instructions** - LLMs execute more rigorously when artifacts are explicit:
   - Preferred: "Output an evaluation of X against Y"
   - Acceptable: "Evaluate X against Y"

3. **Use imperative/requirement form** - Direct commands over conditional statements:
   - Preferred: "Existing record content MUST be known. Before updating, review what records say."
   - Avoid: "When existing record content is unknown. Understand what records currently say before updating."

4. **Evaluation criteria over descriptions** - Focus on what to check or do, not what things mean

5. **Minimal descriptions** - Goal/Intent/Scope brief (2-3 sentences max each)

6. **Instructions/evaluation first** - Actions/Steps sections are the heart of the skill

**Example transformations:**
- ❌ Descriptive: "Validation is the process of checking whether something meets criteria..."
- ✅ Action-oriented: "Verify against each criterion. Check evidence. Report pass/fail with specifics."
- ✅ Evaluation-oriented: "Criteria verified? Evidence checked? Pass/fail reported with specifics?"

### 6. Context Window Principle

**Context window is a public good.** Your skill shares context with system prompt, conversation history, other skills, and user's request.

**Default: Claude is already smart.** Only add context Claude doesn't have.

**Challenge every addition:**
- "Does Claude need this explanation?"
- "Does this justify its token cost?"
- "Is this information Claude already knows?"

**Keep skills focused on execution instructions, not education.**

### 7. Naming Conventions

**Preferred:** Gerund form (verb + -ing)
- `analyzing-code`, `processing-pdfs`, `managing-databases`

**Acceptable alternatives:**
- Action form: `analyze-code`, `process-pdfs`
- Noun form: `code-analysis`, `pdf-processing`

**Requirements:**
- Lowercase letters, numbers, hyphens only
- Max 64 characters
- No reserved words: "anthropic", "claude"

**Avoid:** Vague names (`helper`, `utils`), overly generic (`tools`, `data`)

### 8. Description Requirements

The description field in frontmatter must follow these rules:

**Always third person:**
- ✓ "Analyzes code for bugs and style issues"
- ✗ "I can help analyze code"
- ✗ "You can use this to analyze"

**Include both:**
1. **What it does** (specific, not vague)
2. **When to use** (triggers, key terms, contexts)

**Example:**
```yaml
description: >
  Analyzes Python code for bugs and style issues. Use when reviewing Python files,
  debugging, or checking code quality. Triggers on: "check this code", "find bugs",
  "review this file".
```

### 9. Conciseness Targets

- **Primitives:** 100-150 lines maximum
- **Workflows:** 150-250 lines maximum
- If longer, refactor into multiple skills or remove verbose descriptions

### 10. Follow the Template

Use the skill template below, customizing sections as appropriate for your skill type:
- Primitives: Use 2 Key Results, Actions section
- Workflows: Use 2-4 Key Results, Steps → Tasks sections

### 11. Validate the Skill

**This is the final step. Repeat until all criteria pass.**

For each criterion:
1. Output evidence for PASS
2. Output evidence for FAIL
3. Decide PASS or FAIL based on evidence

If any criterion fails: fix the issue, then re-validate all criteria.

---

## Skill Template

```markdown
---
name: skill-name
description: >
  [What this skill does. Be specific about when to use this skill.]
  You MUST satisfy the Goal, Key Results and follow the Requirements of this skill. They are specified in the instruction body.
  Triggers on: "[trigger phrase 1]", "[trigger phrase 2]".
argument-hint: "[optional: e.g. [subject] or [feature]]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Skill Name

**Goal:** [What this skill achieves - one sentence.]

**Intent:** [Why this skill exists - what problem it solves or prevents.]

**Scope:** [What this skill covers. Be specific about activities and artifacts.]

---

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. [constraint on HOW to execute]
2. [constraint on HOW to execute]
3. [constraint on HOW to execute]

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** [what must be provided]

**Elicit if not provided:**
- [what skill will detect or ask about]

**Optional:** [optional inputs that enhance the skill]

## Postconditions

The resulting state after the skill is finished.

**Success:** [state when skill completes successfully]

**Failure:** [state when skill cannot complete]

---

## Protocols

Use these protocols to satisfy your key results. If you have not read a protocol, you MUST read it prior to using it.

- **[protocol_name.md]** - Must use to [required action]
- **[protocol_name.md]** - Use when [optional trigger or condition]

---

## Execution Items

Execute these to achieve each Key Result. Each item shows which KR it serves.

### [Item Name] (→ KR#)

[What this does and when to use it.]

### [Item Name] (→ KR#, KR#)

[What this does and when to use it.]

**Note:**
- Workflows replace this section with: Steps (ordered list) → Tasks (detailed execution)
- Primitives replace this section with: Actions (detailed execution)

---

## Validation Criteria

For each criterion, output evidence for PASS and evidence for FAIL, then decide.

- [ ] **Naming:** Follows naming conventions (gerund preferred, lowercase/hyphens only, max 64 chars, no reserved words)
- [ ] **Description:** Third person, specific what + when, includes triggers
- [ ] **Structure:** Follows template. All sections present. Frontmatter complete. Action/instruction/evaluation-oriented language.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Execution items show (→ KR#), all KRs served
- [ ] **Protocols:** All relevant protocols referenced. Required: "Must use to [action]". Optional: "Use when [condition]".
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Coherent:** Execution flows logically, no contradictions between sections
- [ ] **Concise:** Meets line count target (primitives: 100-150, workflows: 150-250). Goal/Intent/Scope brief (2-3 sentences max each). No duplication.
- [ ] **Complete:** All necessary information provided, all KRs achievable
- [ ] **Precise:** Specific, unambiguous language, clear definitions
- [ ] **LLM-focused:** Instructions/evaluation criteria over descriptions. No unnecessary explanations.
- [ ] **Research-based (if applicable):** Claude Code's best practices researched FIRST and incorporated. Additional expert/academic sources (if applicable) complement Claude Code guidelines. References section included with all sources.

---

## Additional Notes and Terms

[Domain-specific terms or other notes. Write "None" if not applicable.]
```

---

## Validation Criteria Explained

### 1. Naming
Name follows conventions: gerund form preferred (`analyzing-code`), lowercase letters/numbers/hyphens only, max 64 characters, no reserved words ("anthropic", "claude").

### 2. Description
Description is third person ("Analyzes..." not "I can..."), includes what it does specifically, when to use it, and trigger phrases.

### 3. Structure
Skill follows the template structure. All required sections present in correct order. Frontmatter complete and valid. Uses action/instruction/evaluation-oriented language throughout.

### 4. KRs vs Requirements
**Key Results** describe outcomes (WHAT is achieved) - they are measurable results.
**Requirements** describe constraints (HOW to execute) - they are process rules.

**Anti-pattern:** Putting outcomes in Requirements or constraints in Key Results.

### 5. Traceability
Every execution item (Task/Action) must show which Key Result(s) it serves using (→ KR#) notation.
Every Key Result must have at least one execution item that serves it.

### 6. Protocols
All relevant protocols from `protocols/` are referenced in the Protocols section. Required protocols use "Must use to [action]". Optional protocols use "Use when [condition]". The skill does not duplicate protocol content—it references protocols by name.

### 7. Preconditions
Preconditions must be categorized into three groups:
- **Required:** Must be provided to proceed
- **Elicit if not provided:** Skill will detect or ask for these
- **Optional:** Nice to have but not required

### 8. No Redundancy
Each piece of information should appear exactly once. Don't repeat information across sections.

### 9. Coherent
Steps/actions must flow logically. No contradictions between sections. The skill should make sense as a whole.

### 10. Concise
Use as few words as possible. No unnecessary verbosity or duplication.

### 11. Complete
All necessary information is provided. All Key Results are achievable through the defined execution items.

### 12. Precise
Use specific, unambiguous language. Define terms clearly. Avoid vague descriptions.

### 13. LLM-focused
Only include information necessary for the LLM to execute the skill. Avoid explanations of why something works, background context the LLM already knows, or justifications for design choices. Skills are execution instructions, not educational material.

**Anti-pattern:** Explaining concepts the LLM already understands
```markdown
**Scope:** Validation checks if code works. Validation is important because...
```

✅ **Correct: Direct execution focus**
```markdown
**Scope:** Verifying outputs against success criteria by running tests and checking behavior.
```

### 14. Research-based
Claude Code's best practices researched FIRST. Additional expert/academic sources complement (don't replace) Claude Code guidelines. References section lists all sources.

---

## Key Principles

### Goal, Intent, Scope
- **Goal:** One sentence describing what the skill achieves
- **Intent:** Why this skill exists - what problem it solves or prevents
- **Scope:** Detailed description of what's covered, using concrete terms

### Key Results (WHAT)
- Measurable outcomes that define success
- 2 KRs for primitives, 2-4 KRs for workflows
- Must be verifiable as pass/fail
- Focus on results, not process

### Requirements (HOW)
- Constraints on execution process
- How the skill must be performed
- References to protocols (tracking.md, recovery.md, etc.)
- Iteration limits, ordering constraints, quality standards

### Preconditions vs Postconditions
- **Preconditions:** What must exist before starting
- **Postconditions:** State after skill completes (success/failure)

### Execution Items
- Each shows which KR(s) it serves
- Written as actions, not descriptions
- Enough detail to execute, but not over-specified

---

## Common Anti-Patterns

❌ **Outcomes in Requirements**
```markdown
## Requirements
1. Implementation must match design  # This is an outcome (KR), not a constraint
```

✅ **Correct: Constraints in Requirements**
```markdown
## Requirements
1. Read design before writing code  # This is a constraint on HOW
```

❌ **Process constraints in Key Results**
```markdown
## Key Results
1. All files are read before writing  # This is HOW, not WHAT
```

✅ **Correct: Outcomes in Key Results**
```markdown
## Key Results
1. Implementation matches design  # This is WHAT is achieved
```

❌ **Vague scope**
```markdown
**Scope:** This skill helps with testing.
```

✅ **Specific scope**
```markdown
**Scope:** Designing test strategies by identifying units to test, edge cases, and test boundaries per test pyramid principles.
```

❌ **Windows-style paths**
```markdown
scripts\helper.py  # Backslashes cause issues
```

✅ **Use forward slashes**
```markdown
scripts/helper.py  # Works across platforms
```

❌ **Nested skill references**
```markdown
Use workflow A, which calls workflow B, which calls primitive C
```

✅ **Keep references one level deep**
```markdown
Workflow directly references primitives it needs
```

❌ **Too many options**
```markdown
Choose from 15 different validation approaches...
```

✅ **Provide default with escape hatch**
```markdown
Use standard validation (validate.md). For custom needs, specify criteria.
```

❌ **Time-sensitive information**
```markdown
As of 2024, the best practice is...
```

✅ **Timeless or versioned**
```markdown
Current best practice (see References)... | Old patterns: [deprecated approach]
```

❌ **Vague descriptions**
```markdown
description: "Helps with documents"
```

✅ **Specific what + when**
```markdown
description: "Extracts text from PDFs. Use when processing PDF files, converting to text, or analyzing PDF content."
```

❌ **First/second person**
```markdown
description: "I can help you analyze code" | "You can use this to analyze"
```

✅ **Third person**
```markdown
description: "Analyzes code for bugs and style issues"
```
