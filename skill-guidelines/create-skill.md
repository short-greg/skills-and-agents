# Create Skill

---

# Overview

## Name
create-skill

## Background
Creating effective AI coding assistant skills requires understanding skill structure, checklist generation, progress tracking, and documentation maintenance patterns. Without a structured process, skills end up incomplete, skip steps during execution, or fail to integrate with project conventions.

## Skill Intent
Enable consistent skill creation by guiding users through the complete process: from understanding requirements to producing a working SKILL.md file that follows framework conventions.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `create-skill` | Create a new skill following framework conventions | Atomic |

### When to Use Each Skill

**Use `create-skill` when:**
- Creating a new skill from scratch
- Converting an ad-hoc workflow into a reusable skill
- Need structured guidance through skill creation
- Want to ensure skill follows framework patterns

## When to Use This Guideline
**Use this guideline when:**
- Setting up the create-skill skill for a project
- Need to understand the skill creation process
- Want to create skills that follow framework conventions

**Do NOT use when:**
- Modifying existing skills (edit directly)
- Creating skill suites (use appropriate skill-guideline template)
- Setting up repository structure (use setup-guidelines)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** New skills are complete, consistent, and effective on first use

**Key Results:**
1. 100% of created skills have all required sections
2. 100% of created skills include progress tracking
3. Skills execute without missing steps or phases

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for skill requirements
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill design with user
- [ ] 5. Write SKILL.md file:
  - `${TOOL_CONFIG}/skills/create-skill/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How the create-skill skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type and conventions
2. Gather skill requirements from user
3. Based on skill complexity, include/exclude sections
4. For each phase, add corresponding checklist items
5. End with validation and testing items

**Atomic skill (`create-skill`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Gather skill name and purpose from user"
  - "Determine skill type (atomic/orchestrator/review)"

phase_items:
  requirements:
    - "Define what problem skill solves"
    - "Define when to use / when NOT to use"
    - "Identify inputs and outputs"
  design:
    - "Design phase structure"
    - "Define checklist generation rules"
    - "Plan progress tracking format"
  documentation:
    - "Write all SKILL.md sections"
    - "Add examples and edge cases"
  validation:
    - "Review against skill-template.md"
    - "Test skill execution"

conditional_items:
  - condition: "skill_type == 'orchestrator'"
    items:
      - "Define atomic skills to call"
      - "Define loop gates between skills"
  - condition: "repo_type == 'production'"
    items:
      - "Add comprehensive error recovery"
      - "Add rollback strategies"

always_last:
  - "Verify skill follows framework conventions"
```

---

# Procedures

## Procedure: Skill Creation (create-skill)
**Purpose:** Create a complete SKILL.md file following framework conventions

**Steps:**

### Phase 1: Requirements Gathering
1. **Get skill name** - Short, descriptive, kebab-case (e.g., `feature-impl`)
2. **Get skill purpose** - One sentence describing what it does
3. **Determine skill type:**
   - **Atomic** - Does one thing, standalone
   - **Orchestrator** - Coordinates multiple atomic skills
   - **Review** - Validates output of another skill
4. **Define boundaries:**
   - When to use this skill
   - When NOT to use (and what to use instead)
5. **Identify inputs** - What does skill receive? (files, descriptions, specs)
6. **Identify outputs** - What does skill produce? (code, docs, specs)

### Phase 2: Phase Design
1. **Break work into phases** - Each phase is a logical unit
2. **Define phase purpose** - What each phase accomplishes
3. **Define phase steps** - Concrete actions within each phase
4. **Define gates** - What must pass before next phase
5. **Design checklist generation:**
   - Base items (always included)
   - Conditional items (based on repo type, inputs)
   - Loop gates (when to retry)
   - Final items (always last)

### Phase 3: Progress Tracking Design
1. **Define progress display format** - How phases and tasks are shown
2. **Define checklist markers:**
   - `[ ]` = pending
   - `[x]` = completed
   - `[-]` = skipped
3. **Define status reporting** - What context to show

### Phase 4: Write SKILL.md
1. **Write frontmatter** - name, description, argument-hint
2. **Write each section** from skill-template.md:
   - Background
   - Intent
   - Role
   - When to Use
   - Command
   - OKRs
   - Prerequisites
   - Risks
   - Process (phases, checklist rules, error recovery)
   - Deliverables
   - Reporting/Transparency
   - Fault Tolerance
   - Related Skills

### Phase 5: Validation
1. **Review against skill-template.md** - All sections present?
2. **Check progress tracking** - Format correct?
3. **Check checklist generation** - Rules complete?
4. **Test execution** - Run skill, verify it works

**Override Points:**
- Section requirements (some may be optional for simple skills)
- Progress tracking format
- Checklist marker style

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before skill creation

**Steps:**
1. Check if similar skill already exists
2. Verify skill doesn't overlap with existing skills
3. Confirm skill scope is appropriate (not too broad/narrow)

**Override Points:**
- Domain-specific risk checks

---

# Customization by Repo Type

## Prototype
- Skills can skip comprehensive error recovery
- Fault tolerance section can be minimal
- OKRs can be simplified

## Production
- Full error recovery required
- Comprehensive fault tolerance
- Complete OKRs with measurable thresholds

## Library
- Skills must consider API stability
- Document backward compatibility concerns
- Include versioning considerations

---

# Framework References

- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist syntax
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria
- [references/spec-maintenance.md](../references/spec-maintenance.md) - Progress tracking

---

# Skill Output Template

Use [skill-template.md](../templates/skill-template.md) as the base for creating the SKILL.md file.

**During the interview process, fill in these sections:**

1. **Frontmatter** - name, description, argument-hint
2. **Background** - why this skill exists
3. **Intent** - what it accomplishes
4. **Role** - persona and reasoning approach
5. **When to Use** - boundaries and use cases
6. **Command** - name, arguments, examples
7. **OKRs** - measurable outcomes
8. **Prerequisites** - what must exist
9. **Risks** - inherent risks and mitigations
10. **Process** - checklist generation rules, phases, input handling, error recovery
11. **Deliverables** - outputs produced
12. **Reporting/Transparency** - progress tracking format
13. **Fault Tolerance** - rollback, partial completion, recovery
14. **Related Skills** - connections to other skills

**Output location:**
- `${TOOL_CONFIG}/skills/create-skill/SKILL.md`

---

# Interview Questions

**Questions to ask when creating a new skill:**

## Core Identity
1. What is the skill name? (kebab-case, e.g., `feature-impl`)
2. What does this skill do in one sentence?
3. What type of skill is it? (atomic/orchestrator/review)

## Boundaries
4. When should someone use this skill?
5. When should they NOT use it? What should they use instead?
6. What are the prerequisites? (files, state, dependencies)

## Inputs and Outputs
7. What input does the skill receive? (spec file, description, etc.)
8. What output does the skill produce? (code, docs, etc.)
9. Where are outputs saved?

## Process
10. What are the major phases of this skill?
11. What must be true before moving to the next phase?
12. What should happen if something fails?

## Context
13. What role/persona should the skill adopt?
14. What documentation should the skill read first?
15. What patterns should it follow from existing code?

---

# Skill Types Reference

## Atomic Skills
- Do one specific thing
- Standalone execution
- Clear single purpose
- Examples: `feature-define`, `create-example`, `bugfix-impl`

**Characteristics:**
- Single responsibility
- Can be called by orchestrators
- Produces specific deliverable
- Has own progress tracking

## Orchestrator Skills
- Coordinate multiple atomic skills
- Manage flow between skills
- Handle state across phases
- Examples: `feature-workflow`, `bugfix-workflow`

**Characteristics:**
- Calls atomic skills in sequence
- Manages loop gates (retry on failure)
- Determines starting point from existing artifacts
- Higher-level progress tracking

## Review Skills
- Validate output of other skills
- Gate progression in workflows
- Provide feedback for revision
- Examples: `feature-define-review`, `feature-plan-review`

**Characteristics:**
- Takes output of another skill as input
- Returns pass/fail with feedback
- May suggest specific improvements
- Typically called by orchestrators

---

# Common Patterns

## Progress Tracking Pattern
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 [SKILL-NAME] PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ROLE: [Persona Name]

PHASES:
✅ Phase 1: [Name]
🔄 Phase 2: [Name] [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: [Name]

CHECKLIST (Phase 2):
[x] 1. [Completed item]
[x] 2. [Completed item]
[ ] 3. [Current item] ◀── CURRENT
[ ] 4. [Pending item]

CONTEXT:
- Repo type: ${repo_type}
- [Other relevant context]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Checklist Generation Pattern
```yaml
base_items:
  - "Items that always run"

conditional_items:
  - condition: "some_condition"
    items:
      - "Items when condition is true"

loop_gates:
  - trigger: "failure_condition"
    action: "go to step [step_name]"

always_last:
  - "Final items that always run"
```

## Documentation Maintenance Pattern
```yaml
# At start of skill
read_first:
  - "Project documentation (e.g., CLAUDE.md, .cursorrules)"
  - "Relevant package docs"

# At end of skill
update_if_needed:
  - condition: "new_pattern_established"
    update: "Project documentation"
  - condition: "api_changed"
    update: "Package documentation"
```

---

# Validation Checklist

**Before marking skill creation complete, verify:**

## Structure
- [ ] Frontmatter present (name, description, argument-hint)
- [ ] All sections from skill-template.md included
- [ ] Sections in correct order

## Content
- [ ] Background explains why skill exists
- [ ] Intent is clear and specific
- [ ] When to Use has clear boundaries
- [ ] When NOT to Use lists alternatives
- [ ] Prerequisites are complete
- [ ] Risks identified with mitigations

## Process
- [ ] Phases are logical and sequential
- [ ] Each phase has clear purpose
- [ ] Checklist generation rules are complete
- [ ] Gates between phases defined
- [ ] Error recovery documented

## Progress Tracking
- [ ] Progress display format defined
- [ ] Checklist markers documented
- [ ] Context information specified

## Quality
- [ ] OKRs are measurable
- [ ] Deliverables clearly specified
- [ ] Fault tolerance addressed
- [ ] Related skills identified
