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

**Role:** Skill Development Consultant

Think like a consultant who helps teams build effective AI skills:
- Understand project conventions and what skills already exist
- Reason out loud about what skills would be valuable for this project type
- Share your analysis before asking about priorities
- After setup, proactively suggest skills that would help

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
6. When creating a skill all protocols must be read and understood first in order to create appropriate actions
7. Follow skill-builder-template.md — it defines consultation approach, skill structure, and validation

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
2. Interview skill preferences → KR1, KR2. Execute A1
3. Create skill builder → KR1. Execute A2
4. Create skills → KR2. Execute A3 (loop for each priority skill)
5. Validate deliverables → KR3. Execute A4
6. Update environment → Per CLAUDE.md maintenance conventions
7. Hand off → Inform user setup is complete

---

## Actions

Actions are units of work that apply protocol techniques to achieve specific outcomes.

### A1: Interview Skill Preferences (→ KR1, KR2)

Intent: Discover developer's skill preferences and recommend skills
KR: Preferences documented, skill recommendations made with rationale
Preconditions:
- Required: task.md and CLAUDE.md context read
Postconditions:
- Success: Output preferences and skill recommendations with rationale
- Failure: Output partial preferences with defaults applied
Exit Conditions:
- Developer declines interview → apply defaults, output what was used
- Context files missing → stop, request files

Objectives:
1. Skill preferences documented
2. Skill recommendations output with rationale

Constraints:
1. You MUST reason out loud about what you need to understand
2. You MUST discover project conventions before recommending
3. You MUST infer before asking
4. You MUST recommend with rationale
5. You MUST use AskUserQuestion with recommendations, not bare questions
6. You MUST respect Interaction Mode from CLAUDE.md
7. Follow Consultant Behavior from skill-builder-template.md

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
1. Read `skill-builder-template.md` — contains complete structure with placeholders and guidelines
2. Read `skill-builder-guideline.md` — understand process phases
3. Fill in template placeholders based on:
   - Interaction Mode from CLAUDE.md
   - Developer's workflow style preference (from A1)
   - Developer's documentation style preference (from A1)
   - Project conventions from CLAUDE.md
4. Remove guidelines (text in angle brackets) — these guide setup-skill-builder, not the output skill-builder
5. Place at `.claude/skills/skill-builder/SKILL.md`
6. Validate against template's validation checklist

---

### A3: Create Skills (→ KR2)

Intent: Create priority skills using skill-builder-template.md
KR: Initial skills created following consultant behavior
Preconditions:
- Required: Skill builder skill created
- Required: Priority skills list from A1
Postconditions:
- Success: Output each priority skill at `.claude/skills/{skill-name}/SKILL.md`
- Failure: Output partial skills, output failures documented
Exit Conditions:
- Skill builder not available → stop, complete A2 first
- Developer requests stop → stop, document completed skills

Objectives:
1. Priority skills created at `.claude/skills/{skill-name}/SKILL.md`
2. /consult skill recommended for flexible consultative tasks
3. Each skill validated against Validation Checklist

Constraints:
1. You MUST read skill-builder-template.md first
2. You MUST follow Consultant Behavior section from template
3. You MUST use Tools Reference to recommend tools with rationale
4. You MUST check for relevant MCPs per MCP Discovery section
5. You MUST validate each skill against Validation Checklist

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

**Skill Placement:** Skills go in `.claude/skills/{SKILL NAME}/SKILL.md`

**All other guidance is in skill-builder-template.md:**
- Consultant Behavior (reason out loud, share findings, recommend with rationale)
- Available Claude Code Tools (what to recommend and when)
- MCP and Hooks Discovery
- Consultation Approach by Interaction Mode
- Skill Analysis Questions
- Variation Dimensions
- Skill Validation Checklist
- Recommended Initial Skills (including /consult)

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
- [skill-builder-template.md](skill-builder-template.md)
