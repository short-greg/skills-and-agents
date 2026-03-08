# Skill Builder

[COMMENT: also must successfully capture the intent of the developer, currently it is not doing that. I don't think it has even looked at the protocols or modes that are available with the current state it would be better not to use this at all.]

**Goal:** Create new skills that follow project conventions and integrate with the existing mode/protocol infrastructure.

[COMM]
**Intent:** Consistent skill creation prevents fragmentation and ensures all skills work together. A skill builder encodes project patterns so new skills maintain quality and coherence.

**Scope:** Creating new skills for this project. Handles skill definition, file structure, mode/protocol integration, and registration in CLAUDE.md.

---

[COMMENT: I don't think this successfully capture the considerations and other things that were supposed to be made. it just looks liek a skill AI created without thinking very deeply]

## Key Results - KR

1. Skill specification gathered - purpose, triggers, key results defined
2. Skill follows project patterns - uses existing modes and protocols appropriately
3. Skill is complete and documented - SKILL.md created with all required sections
4. Skill is discoverable - registered in CLAUDE.md skills section

## Requirements and Constraints - REQ

1. Consultative approach - propose structure, let developer decide
2. Reuse existing infrastructure - leverage modes and protocols, don't duplicate
3. Follow naming conventions - skill folder matches skill name, kebab-case
4. Include recovery capability - skills with multi-step workflows need trace support

---

## Preconditions

**Required:** Description of what the skill should do

**Optional:**
- Example triggers or invocations
- Specific modes to use
- Output format preferences
- Integration requirements with other skills

## Postconditions

**Success:** New skill folder created with SKILL.md, registered in CLAUDE.md

**Failure:** Requirements unclear or conflicts with existing skills. Output what's needed to proceed.

---

## Steps

1. **Gather skill requirements** - Interview developer about purpose, triggers, expected behavior
2. **Analyze existing infrastructure** - Identify relevant modes and protocols to reuse
3. **Design skill structure** - Propose key results, steps, and actions
4. **Confirm with developer** - Present design for approval
5. **Create skill files** - Write SKILL.md following project patterns
6. **Register skill** - Add to CLAUDE.md skills section
7. **Validate** - Confirm skill is discoverable and documented

---

[COMMENT: this should be in a separate file that gets referenced]
## Skill Structure Template

```markdown
# [Skill Name]

**Goal:** [One sentence - what does this skill accomplish?]

**Intent:** [Why is this skill needed? What problem does it solve?]

**Scope:** [What's included and excluded from this skill's responsibility?]

---

## Key Results - KR

1. [Measurable outcome 1]
2. [Measurable outcome 2]
...

## Requirements and Constraints - REQ

1. [Rule or constraint 1]
2. [Rule or constraint 2]
...

---

## Preconditions

**Required:** [What must exist before skill can run?]

**Optional:** [What additional context helps?]

## Postconditions

**Success:** [What exists when skill completes successfully?]

**Failure:** [What happens on failure? What output is provided?]

---

## Steps

1. [Step 1] - uses [mode/protocol]
2. [Step 2] - uses [mode/protocol]
...

---

## Tasks

[For complex skills, break into discrete tasks with their own instructions]

### Task Name (-> KR#)

**Goal:** [Task-specific goal]

**When:** [Condition for executing this task]

**Mode:** [Which mode(s) to use]

**Instructions:** [How to execute]

**Inputs:** [Required and optional inputs]

**Outputs:** [What this task produces]
```

---

## Available Modes

Reference these modes in skill steps:

| Mode | Use For |
|------|---------|
| `orienting` | Understanding current state before acting |
| `interviewing` | Gathering requirements from developer |
| `defining` | Specifying requirements and acceptance criteria |
| `planning` | Sequencing actions to achieve goal |
| `designing` | Creating solutions and architectures |
| `implementing` | Writing code and creating artifacts |
| `evaluating` | Validating outcomes against criteria |
| `brainstorming` | Generating ideas and alternatives |
| `investigating` | Deep research and analysis |
| `positioning` | Configuring approach for task |
| `maintaining` | Updating and improving existing work |

---

## Available Protocols

Reference these protocols for specific techniques:

| Protocol | Provides |
|----------|----------|
| `tracking_and_recovery` | Progress tracking, trace files, recovery |
| `elicitation` | Questioning techniques, inference |
| `goal_setting` | Goal decomposition, success criteria |
| `criteria_setting` | Measurable criteria, thresholds |
| `thinking` | Analytical, strategic, creative thinking |
| `discipline` | MECE enumeration, coverage tracking |
| `pragmatics` | Communication framing, option presentation |
| `transparency` | Output formatting, reasoning visibility |
| `risk_management` | Risk identification, mitigation |
| `instruction_giving` | Clear action instructions |

---

## Naming Conventions

- **Skill folder**: kebab-case matching skill name (e.g., `skill-builder`, `dev-planning`)
- **Skill file**: Always `SKILL.md`
- **Task folders**: `YYYYMMDD-<task-name>` when skill creates task artifacts

---

## Integration Points

Skills can:
- Reference modes: `uses modes/planning.md`
- Reference protocols: `per protocols/tracking_and_recovery.md`
- Create task folders: Following `./tasks/YYYYMMDD-<name>/` convention
- Use trace files: For recovery capability in multi-step workflows
