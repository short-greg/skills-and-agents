---
name: setup-skill-builder
description: >
  Create skill builder and initial skills for the project.
  Reads decisions from task.md and CLAUDE.md, creates project-specific skill builder, generates initial skills.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup skill builder", "setup skills", "create skill builder", "make skills".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup Skill Builder

**Goal:** Create project-specific skill builder and initial skills based on project conventions.

**Intent:** Skills should follow project conventions discovered during setup-init and documented in CLAUDE.md.

**Scope:** Create skill builder skill, interview about skill priorities, create initial skills, validate all deliverables.

**Workflow Style:** Hybrid. Imperative bookends (read context, validate), declarative middle (skill creation adapts to needs).

---

## Key Results - KR

1. Skill builder skill created following project conventions
2. Initial skills created based on developer priorities
3. All setup deliverables validated with evidence

## Requirements and Constraints - REQ

1. Read task.md and CLAUDE.md first — understand context before creating
2. Follow the Interaction Mode from CLAUDE.md
3. Track progress per `tracking_and_recovery.md`
4. Understood developer priorities about skill creation
5. Validate deliverables with positive AND negative evidence
6. When creating a skill all protocols be read and understood first in order to create appropriate actions.
7. The skill-builder-skill that gets output MUST
   1. Act as a consultant. It must understand the intent, it must clarify uncertainties, it must provide guidance based on the developers nees.
   2. Create a proposal for the skill that gets approved by the user before.
   3. Plan out the actions that the skill will need.
   4. Include preferences about whether the developer prefers declarative or imperative. If it is imperative it expresses the order of the actions. If declarative the skill-builder will aim to satisfy the goal.
   5. Include preferences about whether the developer prefers prescriptive or descriptive
   6. Validate the skill before finalizing it, finding evidence why it fails and why it succeeds and confirm that it follows conventions
   7. Include a couple of patterns for how to write steps (a) pure declarative (only a setp to plan steps), (b) mixed (e.g. it includes steps at the beginning to set up and at the end to update CLAUDE.md or documentation), (c) pure imperative (i.e. all steps are defined)

## Preconditions

**Required:**
- task.md from setup-init
- CLAUDE.md with Interaction Mode and conventions
- Environment set up by setup-env

**Optional:**
- Existing skills to reference
- Developer's immediate skill needs

## Postconditions

**Success:**
- Skill builder skill created and documented
- Initial skills created per developer priorities
- Validation summary with evidence for each KR
- CLAUDE.md updated with available skills

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify task.md and CLAUDE.md exist. **If not, STOP with appropriate message.**
1. Create preliminary checklist → KR1-3. Use TodoWrite with formula: `setup-skill-builder - KR# - <action> - <details>`
2. Read context → Read task.md and CLAUDE.md to understand conventions and Interaction Mode
3. Interview skill preferences → KR1, KR2. Execute A1
4. Create skill builder → KR1. Execute A2
5. Create skills → KR2. Execute A3 (loop for each priority skill)
6. Validate deliverables → KR3. Execute A4
7. Update CLAUDE.md → Add available skills section
8. Hand off → Inform user setup is complete

---

## Actions

Actions are units of work that apply protocol techniques to achieve specific outcomes.

### A1: Interview Skill Preferences (→ KR1, KR2)

Intent: Discover developer's skill preferences and priorities
KR: Workflow style, documentation style, and priority skills documented
Preconditions:
- Required: task.md and CLAUDE.md context read
Postconditions:
- Success: Output documented preferences (workflow style, documentation style, priority skills list)
- Failure: Output partial preferences with defaults applied, output what could not be determined
Exit Conditions:
- Developer declines preferences interview → stop, apply documented defaults, output defaults used
- Context files inaccessible → stop, output which files missing

Objectives (OBJ):
1. Discover workflow style preference (declarative/imperative/hybrid)
2. Discover documentation style preference (prescriptive/descriptive)
3. Discover which skills developer needs first
4. Infer from context before asking

Constraints (CONST):
1. Read `elicitation.md` and apply Inference Before Asking — to minimize questions
2. Read `elicitation.md` and apply Targeted Questioning — to ask only what cannot be inferred
3. Respect Interaction Mode from CLAUDE.md when questioning

Validation:
1. Add OBJ1-4 to TodoWrite checklist
2. Mark each objective complete as achieved
3. Output validation: list each OBJ with evidence it was achieved
4. Output documented preferences summary

---

### A2: Create Skill Builder Skill (→ KR1)

Intent: Create consultative skill builder with developer preferences baked in
KR: Skill builder skill created following project conventions
Preconditions:
- Required: Skill preferences from A1
- Required: task.md and CLAUDE.md
- Required: skill-builder-guideline.md read
Postconditions:
- Success: Output skill builder skill at `.claude/skills/skill-builder/SKILL.md`
- Failure: Output partial skill, output validation failures
Exit Conditions:
- skill-builder-guideline.md inaccessible → stop, request file
- Preferences incomplete → stop, complete A1 first

Instructions:
1. Read `skill-builder-template.md` — to understand expected output structure
2. Read `skill-builder-guideline.md` — to understand requirements
3. Read `discipline.md` and apply Sequential Processing — to ensure completeness
4. Read `tracking_and_recovery.md` and apply Progress Recording — to track creation
5. Create skill builder following all REQ requirements
6. Incorporate developer's workflow style preference
7. Incorporate developer's documentation style preference
8. Place at `.claude/skills/skill-builder/SKILL.md`
9. Validate against requirements

---

### A3: Create Skill (→ KR2)

Intent: Create each priority skill using the skill builder
KR: Initial skills created and placed in `.claude/skills/`
Preconditions:
- Required: Skill builder skill created
- Required: Priority skills list from A1
Postconditions:
- Success: Output each priority skill at `.claude/skills/{skill-name}/SKILL.md`
- Failure: Output partial skills, output failures documented
Exit Conditions:
- Skill builder not available → stop, complete A2 first
- Developer requests stop → stop, document completed skills

Instructions:
1. For each priority skill in list:
   a. Use skill builder to create the skill
   b. Place in `.claude/skills/{skill-name}/SKILL.md`
   c. Mark complete in progress tracker
2. Output summary of created skills

---

### A4: Validate Deliverables (→ KR3)

Intent: Verify setup outputs exist and are correct
KR: Validation summary with positive and negative evidence for each deliverable
Preconditions:
- Required: All skills created
Postconditions:
- Success: Output validation summary with evidence for each KR
- Failure: Output validation failures with remediation steps
Exit Conditions:
- Critical validation failure → stop, report and await guidance
- Skills incomplete → stop, complete A3 first

Instructions:
1. Read `software_quality.md` and apply Quality Assessment — to evaluate artifacts
2. Read `criteria_setting.md` and apply Binary Criteria — for pass/fail assessment
3. For each deliverable (skill builder + each priority skill):
   a. List evidence of success (file exists, structure correct, conventions followed)
   b. List evidence of failure (missing sections, convention violations)
   c. Document both
4. Output validation summary with evidence

---

## Additional Notes and Terms

**Skill Placement:** Skills go in `.claude/skills/`

**Workflow Styles:**
- **Declarative (Goal-Oriented):** "Achieve X outcome" — adapts approach to situation. Use for creative tasks, context-dependent decisions.
- **Imperative (Process-Oriented):** "Follow these exact steps" — consistent execution. Use for compliance, reproducibility.
- **Hybrid:** Imperative bookends, declarative middle. Use when need consistency at boundaries but flexibility in execution.

**Documentation Styles:**
- **Prescriptive:** Tells you exactly what to do. Less ambiguity, less flexibility.
- **Descriptive:** Explains concepts and options. More flexibility, requires more judgment.

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Skills don't follow conventions | Skill builder created without reading CLAUDE.md | Require CLAUDE.md read as precondition; validate against conventions |
| 2 | Scope creep in skills | Priority skills become too complex | Keep each skill focused; split if exceeds 200 lines |
| 3 | Missing protocol references | Actions created without linking to protocols | Validate each action references at least one protocol with technique |
| 4 | Preferences not captured | Developer style preferences lost between A1 and A2 | Document preferences explicitly; reference in A2 preconditions |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [skill-builder-guideline.md](../../guidelines/skill-builder-guideline.md)
- [workflow-creation-guideline.md](../../guidelines/workflow-creation-guideline.md)
