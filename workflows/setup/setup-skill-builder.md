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
6. When creating a skill all protocols and modes be read and understood first in order to create appropriate tasks.
7. The skill-builder-skill that gets output MUST
   1. Act as a consultant. It must understand the intent, it must clarify uncertainties, it must provide guidance based on the developers nees.
   2. Create a proposal for the skill that gets approved by the user before.
   3. Plan out the tasks that the skill will need.
   4. Include preferences about whether the developer prefers declarative or imperative. If it is imperative it expresses the order of the tasks. If declarative the skill-builder will aim to satisfy the goal.
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
1. Read context → Read task.md and CLAUDE.md to understand conventions and Interaction Mode
2. Interview skill preferences → KR1, KR2. Execute "Interview Skill Preferences" task
3. Create skill builder → KR1. Execute "Create Skill Builder Skill" task
4. Create skills → KR2. Execute "Create Skill" task (loop for each priority skill)
5. Validate deliverables → KR3. Execute "Validate Deliverables" task
6. Update CLAUDE.md → Add available skills section
7. Hand off → Inform user setup is complete

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.

### Interview Skill Preferences (→ KR1, KR2)

**Goal:** Discover developer's skill preferences and priorities

**When:** After reading context

**Mode:** [interviewing](../../modes/interviewing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Discover workflow style preference (declarative/imperative/hybrid)
- Discover documentation style preference (prescriptive/descriptive)
- Discover which skills they need first

**Outputs:**
- Workflow style preference (declarative/imperative/hybrid)
- Documentation style preference (prescriptive/descriptive)
- List of priority skills to create

---

### Create Skill Builder Skill (→ KR1)

**Goal:** Create a consultative skill builder with developer preferences baked in

**When:** After preferences gathered

**Mode:** [implementing](../../modes/implementing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Read [skill-builder-guideline.md](../../guidelines/skill-builder-guideline.md) first
- Follow all requirements for the skill-builder deliverable in REQ section
- See Example Skill Builder Output in Additional Notes

**Inputs:**
- Skill preferences from interview (required)
- task.md and CLAUDE.md (required)
- skill-builder-guideline.md (required)

**Outputs:**
- Skill builder skill at `.claude/skills/`

---

### Create Skill (→ KR2)

**Goal:** Create each priority skill using the skill builder

**When:** Skill builder created

**Mode:** Use the skill builder skill

**Instructions:**
For each priority skill:
1. Use skill builder to create the skill
2. Place in `.claude/skills/`

**Inputs:**
- Skill builder skill (required)
- Priority skills list (required)

**Outputs:**
- Initial skills customized to project

---

### Validate Deliverables (→ KR3)

**Goal:** Verify setup outputs exist and are correct

**When:** All skills created

**Mode:** [evaluating](../../modes/evaluating.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Find evidence of success AND evidence of failure
- Output validation summary

**Outputs:**
- Validation summary with evidence

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

## Example Skill Builder Output

The skill builder skill should produce output similar to this annotated example:

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
**Scope:** Create individual skills. Does not create modes, protocols, or workflows.

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
4. Each task must have one mode
5. Validate with positive AND negative evidence

---

## Steps

<!-- COMMENT: Pure declarative pattern - only Step 1 plans the rest -->
<!-- For pure declarative: -->
0. Read CLAUDE.md → Understand project conventions
1. Plan skill creation → Execute "Plan Skill" task. Output plan for all subsequent steps.

<!-- COMMENT: Hybrid pattern - imperative bookends, declarative middle -->
<!-- For hybrid: -->
0. Read CLAUDE.md → Understand project conventions
1. Understand intent → Execute "Clarify Intent" task
2. Plan and design skill → Plan the design and implementation based on intent
3. Validate skill → Execute "Validate Skill" task

<!-- COMMENT: Pure imperative pattern - all steps defined -->
<!-- For pure imperative: -->
0. Read CLAUDE.md → Understand project conventions
1. Understand intent → Execute "Clarify Intent" task
2. Propose design → Execute "Propose Design" task
3. Create skill → Execute "Create Skill" task
4. Validate skill → Execute "Validate Skill" task

---

## Tasks

<!-- COMMENT: Tasks reference modes, let executor choose actions -->

### Clarify Intent (→ KR1)

**Goal:** Understand what the developer actually needs

**Mode:** [interviewing](../../modes/interviewing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Infer from context before asking
- Discover constraints and success criteria

**Outputs:**
- Clarified skill intent
- Constraints and requirements

---

### Propose Design (→ KR2)

**Goal:** Create skill design for approval

**Mode:** [designing](../../modes/designing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Follow project conventions from CLAUDE.md
- Output design as code block for review

**Outputs:**
- Proposed skill structure
- Task breakdown with modes

---

### Create Skill (→ KR3)

**Goal:** Implement the approved skill design

**Mode:** [implementing](../../modes/implementing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Place skill in `.claude/skills/`
- Follow approved design exactly

**Outputs:**
- Skill file at `.claude/skills/{skill-name}.md`

---

### Validate Skill (→ KR4)

**Goal:** Verify skill follows conventions and will work

**Mode:** [evaluating](../../modes/evaluating.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Find evidence of success AND evidence of failure
- Check against CLAUDE.md conventions

**Outputs:**
- Validation summary with evidence
```

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [skill-builder-guideline.md](../../guidelines/skill-builder-guideline.md)
- [workflow-creation-guideline.md](../../guidelines/workflow-creation-guideline.md)
