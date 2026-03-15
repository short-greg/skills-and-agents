# Skill Builder Template

This is an annotated example of the skill builder skill output. The setup-skill-builder workflow MUST read this file before creating the skill builder.

---

```markdown
---
name: skill-builder
description: >
  Consultative skill for creating new skills. Understands intent, clarifies uncertainties,
  proposes skill design, and validates output.
  <!-- COMMENT: Description explains what AND how -->
argument-hint: "[skill name or intent]"
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, AskUserQuestion, TodoWrite
---

# Skill Builder

<!-- COMMENT: Goal states the outcome, not the process -->
**Goal:** Create well-structured skills that follow project conventions and developer preferences.

<!-- COMMENT: Intent explains WHY this skill exists -->
**Intent:** Skills should be consistent with project patterns. A consultative approach ensures
skills meet actual needs rather than assumed requirements.

<!-- COMMENT: Scope defines boundaries -->
**Scope:** Create individual skills. Does not create protocols or workflows.

<!-- COMMENT: Workflow style from developer preferences -->
**Workflow Style:** {Developer's preference: Declarative/Imperative/Hybrid}

---

## Key Results - KR

1. Skill intent understood and clarified
2. Skill design proposed and approved
3. Skill created following conventions
4. Skill validated with evidence

## Requirements and Constraints - REQ

1. Read CLAUDE.md first — understand project conventions
2. Act as consultant — understand intent before proposing
3. Get approval before creating — no surprises
4. Each action must reference appropriate protocols
5. Validate with positive AND negative evidence

---

## Steps

<!-- COMMENT: Pure declarative pattern - only Step 1 plans the rest -->
<!-- For pure declarative: -->
0. Read CLAUDE.md → Understand project conventions
1. Plan skill creation → Execute A1. Output plan for all subsequent steps.

<!-- COMMENT: Hybrid pattern - imperative bookends, declarative middle -->
<!-- For hybrid: -->
0. Read CLAUDE.md → Understand project conventions
1. Understand intent → Execute A1
2. Plan and design skill → Plan the design and implementation based on intent
3. Validate skill → Execute A4

<!-- COMMENT: Pure imperative pattern - all steps defined -->
<!-- For pure imperative: -->
0. Read CLAUDE.md → Understand project conventions
1. Understand intent → Execute A1
2. Propose design → Execute A2
3. Create skill → Execute A3
4. Validate skill → Execute A4

---

## Actions

<!-- COMMENT: Actions reference protocols directly -->

### A1: Clarify Intent (→ KR1)

Intent: Understand what the developer actually needs
KR: Clarified skill intent with constraints and requirements documented
Preconditions:
- Required: Skill name or intent from user
Postconditions:
- Success: Output clarified skill intent with constraints
- Failure: Output partial understanding, output blockers noted
Exit Conditions:
- User cannot articulate intent → stop, suggest alternatives

Objectives (OBJ):
1. Understand the skill's purpose and use cases
2. Discover constraints and success criteria
3. Infer from context before asking

Constraints (CONST):
1. Read `elicitation.md` and apply Inference Before Asking — to minimize questions
2. Do not exceed 3 clarifying questions

Validation:
1. Add OBJ1-3 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A2: Propose Design (→ KR2)

Intent: Create skill design for approval
KR: Proposed skill structure approved by user
Preconditions:
- Required: Clarified skill intent
Postconditions:
- Success: Output proposed skill structure as code block, output approval confirmation
- Failure: Output proposal rejected, output feedback captured
Exit Conditions:
- User rejects 3 times → stop, escalate

Objectives (OBJ):
1. Follow project conventions from CLAUDE.md
2. Structure skill with appropriate workflow style
3. Present design as code block for review

Constraints (CONST):
1. Read `thinking.md` and apply Alternative Generation — to explore options
2. Output design as code block

Validation:
1. Add OBJ1-3 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A3: Create Skill (→ KR3)

Intent: Implement the approved skill design
KR: Skill file created at correct location
Preconditions:
- Required: Approved design
Postconditions:
- Success: Output skill file at `.claude/skills/{skill-name}/SKILL.md`
- Failure: Output partial skill, output errors documented
Exit Conditions:
- Design not approved → stop, return to A2

Instructions:
1. Read `discipline.md` and apply Sequential Processing — to ensure completeness
2. Create skill directory at `.claude/skills/{skill-name}/`
3. Write SKILL.md following approved design exactly
4. Verify file created successfully

---

### A4: Validate Skill (→ KR4)

Intent: Verify skill follows conventions and will work
KR: Validation summary with positive and negative evidence
Preconditions:
- Required: Skill file created
Postconditions:
- Success: Output validation passed, skill ready for use
- Failure: Output validation failures documented
Exit Conditions:
- Critical failure → stop, return to A3

Instructions:
1. Read `software_quality.md` and apply Correctness — to assess structure
2. Read `criteria_setting.md` and apply Binary Criteria — for pass/fail assessment
3. List evidence of success (structure correct, conventions followed)
4. List evidence of failure (missing sections, violations)
5. Evaluate against CLAUDE.md conventions
6. Output validation summary with evidence
```
