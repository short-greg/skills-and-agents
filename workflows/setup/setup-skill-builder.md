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
   a. Gather context per skill-builder-template.md (CLAUDE.md, existing skills, protocols, tools)
   b. Ask skill analysis questions until intent is clear (adapt to Interaction Mode)
   c. Propose high-level design for approval:
      - Goal, Intent, Scope, Role
      - Proposed KRs (3-4)
      - Proposed Actions (outline with names and purposes)
      - Workflow style recommendation with rationale
   d. After design approval, create initial draft following skill template
   e. Perform risk analysis — identify scope, execution, integration, autonomy, artifact risks
   f. Output draft with risk analysis for developer review
   g. Ask: "What would you change?" — iterate up to 3 times
   h. Validate final skill against checklist
   i. Place in `.claude/skills/{skill-name}/SKILL.md`
   j. Mark complete in progress tracker
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

**Skill Placement:** Skills go in `.claude/skills/`.
- The path must be `.claude/skills/{SKILL NAME}/SKILL.md`
- Put reference files for that skill and code for the skill in the skill directory
- Put common reference files or code in `.claude/`

**Skill-Builder Output Structure:**

The skill-builder skill created by A2 is itself a skill that follows the same template. It must include:
- Goal: Create well-structured skills following project conventions
- Intent: Skills should be consistent with project patterns
- Role: Consultant + skill creator
- Actions that implement the skill creation process:
  - A1: Gather context and ask analysis questions
  - A2: Propose design for approval
  - A3: Create draft, perform risk analysis, iterate
  - A4: Validate and place skill
- Additional Notes containing: Consultation approach table, skill analysis questions, variation dimensions, validation checklist (copied from setup-skill-builder.md)

**Skill Recommendations:**

The skill-builder should proactively recommend skills based on project analysis:
1. After reading CLAUDE.md and existing skills, identify gaps
2. Propose skills that would help based on:
   - Common patterns in the codebase (testing, deployment, documentation)
   - Project type (web API → endpoint skill, data science → notebook skill)
   - Developer pain points mentioned in task.md
3. Output recommendations with rationale before asking what skills to create
4. Let developer accept, modify, or reject recommendations

**Skill Dependencies:**

When a skill depends on another skill:
1. Check if dependency exists in `.claude/skills/`
2. If not, either:
   - Create dependency first (if simple)
   - Document dependency in skill's Preconditions
   - Recommend creating dependency skill

**Sub-agent Recommendations:**

Recommend sub-agents when:
- Skill involves parallel independent tasks (e.g., checking multiple files)
- Skill needs specialized roles (e.g., reviewer + implementer)
- Task complexity would benefit from divide-and-conquer

Recommend single skill when:
- Tasks are sequential and interdependent
- Simple scope with clear linear flow
- Sub-agent overhead not justified

**Consultation Approach by Interaction Mode:**

| Mode | Approach |
|------|----------|
| Lead | Output proposal with rationale, confirm only essential points, proceed efficiently |
| Senior | Share intermediate findings, recommend approach, get approval at gates |
| Peer | Brainstorm options together, collaborate on design, discuss trade-offs |
| Junior | Present all options with trade-offs, await direction at each decision |

**Context to Gather Before Creating Skills:**
1. CLAUDE.md — project conventions, Interaction Mode
2. Existing skills in `.claude/skills/` — check for overlap, composition opportunities
3. Available protocols in `protocols/` — what techniques can be referenced
4. Available tools — what the skill can use

**Skill Analysis Questions (ask until intent is clear):**

*Understanding the Problem:*
1. What problem does this skill solve?
2. Why is this skill necessary? (vs manual, vs existing skill)
3. Who will use this skill?
4. What triggers this skill?

*Defining Scope:*
5. What are the deliverables?
6. Should this be one skill or multiple?
7. Does it compose with other skills?
8. What's the granularity?

*Determining Role:*
9. What domain role? (software engineer, UI designer, data analyst, reviewer, etc.)
10. What behavioral stance? (executor, advisor, pair programmer, auditor)
11. Should it spawn sub-agents with narrower roles?

*Deciding Behavior:*
12. How autonomous? (proceed vs confirm at each step)
13. Echo reasoning? (thinking aloud vs silent)
14. How handle errors? (fail, retry, ask, rollback)

*Planning Artifacts:*
15. Just SKILL.md or additional files?
16. Reference materials needed? (guidelines, templates, examples)
17. Code/scripts needed? (shell functions, validators)
18. Hooks needed? (pre-commit, post-action)
19. What tools will it use?

**Variation Dimensions:**

| Dimension | Options | Determined by |
|-----------|---------|---------------|
| Workflow Style | Declarative, Imperative, Hybrid | Developer preference |
| Action Style | Instructions, Objectives+Constraints | Developer preference or per-action |
| Role | Domain (engineer, designer, analyst) + Behavioral (executor, advisor) | Skill intent |
| Autonomy | Proceed, confirm at gates, confirm each step | Interaction Mode |
| Reasoning | Echo thoughts, silent execution | Developer preference |
| Composition | Standalone, spawns sub-agents, calls other skills | Skill complexity |
| Artifacts | SKILL.md only, +references, +code, +hooks | Skill needs |
| Error handling | Fail fast, retry, ask user, rollback | Developer preference |
| Tracking | Required, recommended, not required | Developer preference |
| Validation | Required, recommended, not required | Developer preference |

**Artifact Types:**

| Type | When to include | Location |
|------|-----------------|----------|
| Reference files | Guidelines, templates, examples | `.claude/skills/{skill-name}/` |
| Code/scripts | Shell functions, validators | `.claude/skills/{skill-name}/` |
| Hooks | Git events, file changes | `.claude/hooks/` or skill folder |

**Workflow Styles:**
- **Declarative (Goal-Oriented):** "Achieve X outcome" — adapts approach to situation. Use for creative tasks, context-dependent decisions.
- **Imperative (Process-Oriented):** "Follow these exact steps" — consistent execution. Use for compliance, reproducibility.
- **Hybrid:** Imperative bookends, declarative middle. Use when need consistency at boundaries but flexibility in execution.

**Documentation Styles:**
- **Prescriptive:** Tells you exactly what to do. Less ambiguity, less flexibility.
- **Descriptive:** Explains concepts and options. More flexibility, requires more judgment.

**Risk Analysis Categories:**

When creating any skill, analyze risks in these categories:
1. **Scope risks** — Is the skill too broad? Too narrow? Overlapping with existing skills?
2. **Execution risks** — What could go wrong during skill execution? Data loss? Incorrect outputs?
3. **Integration risks** — How might this skill conflict with other skills or project conventions?
4. **Autonomy risks** — Is the skill too autonomous for its impact? Should it confirm more?
5. **Artifact risks** — Could generated files overwrite important content? Create clutter?

Output risk analysis with severity (high/medium/low) and proposed mitigations. Consult developer before finalizing.

**Iterative Improvement:**

Skills rarely perfect on first draft. After creating initial draft:
1. Output the draft skill for developer review
2. Ask: "What would you change about this skill?"
3. Iterate based on feedback (up to 3 iterations before escalating)
4. Check: Does it match developer's mental model? Right granularity? Appropriate workflow style?

**Skill Validation Checklist:**

*Structure:*
- [ ] All sections present (Frontmatter, Goal, Intent, Scope, Role, KRs, REQs, Pre/Postconditions, Steps, Actions, Notes, Risks, References)
- [ ] 150-200 lines (200-250 with permission)
- [ ] 3-4 Key Results
- [ ] 2+ Actions
- [ ] Each action has complete structure

*Content:*
- [ ] Coherent — no contradictions between sections
- [ ] Complete — all KRs achievable from Actions
- [ ] Concise — no duplication, minimum words
- [ ] Precise — unambiguous language, no hedging

*Skill-specific:*
- [ ] Role defined (domain + behavioral)
- [ ] Referenced protocols exist in `protocols/`
- [ ] Tools in allowed-tools are available
- [ ] Artifacts appropriate for skill needs

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
