# Create Skill

---

# Overview

## Name
create-skill

## Background
Creating effective AI coding assistant skills requires understanding skill structure, checklist generation, progress tracking, and documentation maintenance patterns. Without a structured process, skills end up incomplete, skip steps during execution, or fail to integrate with project conventions. Skill creation is fundamentally similar to feature development - it requires defining requirements, planning structure, implementing content, and verifying quality.

## Skill Intent
Enable consistent skill creation through a workflow that reuses existing feature development patterns, adding skill-specific concerns like checklist generation rules, progress tracking format, and framework compliance verification.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `skill-workflow` | End-to-end orchestration from idea to working skill | Orchestrator |
| `skill-define` | Define skill requirements and boundaries (uses feature-define pattern) | Atomic |
| `skill-plan` | Plan skill phases and checklist rules (uses feature-plan pattern) | Atomic |
| `skill-impl` | Write SKILL.md content (uses feature-impl pattern) | Atomic |
| `skill-verify` | Verify skill follows framework conventions | Atomic |

### When to Use Each Skill

**Use `skill-workflow` when:**
- Creating a new skill from scratch
- Want complete orchestration with verification gates
- Unsure what already exists (it detects and resumes)

**Use `skill-define` when:**
- Have a skill idea that needs documentation
- Need to clarify skill boundaries and use cases
- Want to define inputs, outputs, and prerequisites

**Use `skill-plan` when:**
- Have approved skill requirements
- Need to design phase structure and checklist rules

**Use `skill-impl` when:**
- Have approved skill plan
- Ready to write SKILL.md content

**Use `skill-verify` when:**
- Skill is implemented
- Need to verify framework compliance

## When to Use This Guideline
**Use this guideline when:**
- Setting up the skill creation workflow for a project
- Need to understand the skill creation process
- Want to create skills that follow framework conventions

**Do NOT use when:**
- Modifying existing skills (edit directly)
- Creating skill suites from a guideline template (use that guideline directly)
- Setting up repository structure (follow tool documentation)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** New skills are complete, consistent, and effective on first use

**Key Results:**
1. 100% of created skills have all required sections
2. 100% of created skills include progress tracking
3. Skills execute without missing steps or phases
4. Skills verified against framework conventions

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Ensure feature-development skills exist (reused by this workflow)
- [ ] 3. Interview user for skill-specific requirements
- [ ] 4. Customize for repo type (prototype/production/library)
- [ ] 5. Confirm skill designs with user
- [ ] 6. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/skill-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/skill-define/SKILL.md`
  - `${TOOL_CONFIG}/skills/skill-plan/SKILL.md`
  - `${TOOL_CONFIG}/skills/skill-impl/SKILL.md`
  - `${TOOL_CONFIG}/skills/skill-verify/SKILL.md`
- [ ] 7. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type and conventions
2. Check for existing skill specs to determine starting point
3. Based on skill complexity, include/exclude sections
4. For each phase, add corresponding checklist items
5. Add loop gates for verification failures
6. End with testing and documentation items

**Orchestrator (`skill-workflow`) checklist generation:**

The orchestrator's checklist is composed from child skill checklists.

```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing skill specs"
  - "Determine starting point based on existing artifacts"

phase_items:
  define:
    - "Run skill-define skill"
    - "(If production) Review skill requirements"
  plan:
    - "Run skill-plan skill"
    - "(If production) Review skill design"
  implement:
    - "Run skill-impl skill"
  verify:
    - "Run skill-verify skill"

loop_gates:
  - trigger: "review_rejected"
    action: "go to previous skill for revision"
  - trigger: "verification_failed"
    action: "go to skill-impl to address issues"

always_last:
  - "Update project documentation if new patterns established"
```

**Atomic skill (`skill-define`) checklist generation:**

This skill extends `feature-define` with skill-specific concerns.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Gather skill name and purpose from user"
  - "Determine skill type (atomic/orchestrator/review)"

phase_items:
  requirements:
    - "Define what problem skill solves"
    - "Define when to use (specific scenarios)"
    - "Define when NOT to use (and what to use instead)"
    - "Identify inputs (files, descriptions, specs)"
    - "Identify outputs (code, docs, specs)"
  boundaries:
    - "Define prerequisites (what must exist)"
    - "Define related skills (what composes with this)"
    - "Identify risks and mitigations"

conditional_items:
  - condition: "skill_type == 'orchestrator'"
    items:
      - "Identify atomic skills to compose"
      - "Define composition flow"

always_last:
  - "Write skill spec to ${SPEC_DIR}/skill-spec.md"
```

**Atomic skill (`skill-plan`) checklist generation:**

This skill extends `feature-plan` with skill-specific structure.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Read approved skill spec"

phase_items:
  structure:
    - "Design phase structure"
    - "Define phase purposes and boundaries"
    - "Plan phase transitions and gates"
  checklist:
    - "Define base items (always included)"
    - "Define conditional items (based on repo type, inputs)"
    - "Define loop gates (when to retry)"
    - "Define final items (always last)"
  tracking:
    - "Design progress display format"
    - "Define status reporting content"

conditional_items:
  - condition: "skill_type == 'orchestrator'"
    items:
      - "Define how child skill checklists compose"
      - "Define orchestrator-level gates"
  - condition: "repo_type == 'production'"
    items:
      - "Plan comprehensive error recovery"
      - "Plan rollback strategies"

always_last:
  - "Write skill plan to ${SPEC_DIR}/skill-plan.md"
```

**Atomic skill (`skill-impl`) checklist generation:**

This skill extends `feature-impl` for SKILL.md content.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Read approved skill plan"

phase_items:
  frontmatter:
    - "Write YAML frontmatter (name, description, argument-hint)"
  sections:
    - "Write Background section"
    - "Write Intent section"
    - "Write Role section"
    - "Write When to Use section"
    - "Write Command section"
    - "Write OKRs section"
    - "Write Prerequisites section"
    - "Write Risks section"
    - "Write Process section (phases, checklist rules, error recovery)"
    - "Write Deliverables section"
    - "Write Reporting/Transparency section"
    - "Write Fault Tolerance section"
    - "Write Related Skills section"
  examples:
    - "Add examples and edge cases"
    - "Add progress tracking display example"

always_last:
  - "Write SKILL.md to ${TOOL_CONFIG}/skills/[skill-name]/SKILL.md"
```

**Atomic skill (`skill-verify`) checklist generation:**

This skill is unique to skill creation - verifies framework compliance.

```yaml
base_items:
  - "Read skill-template.md for required sections"
  - "Read created SKILL.md"

verification_items:
  structure:
    - "Verify frontmatter present (name, description, argument-hint)"
    - "Verify all required sections present"
    - "Verify sections in correct order"
  content:
    - "Verify Background explains why skill exists"
    - "Verify Intent is clear and specific"
    - "Verify When to Use has clear boundaries"
    - "Verify When NOT to Use lists alternatives"
    - "Verify Prerequisites are complete"
    - "Verify Risks identified with mitigations"
  process:
    - "Verify phases are logical and sequential"
    - "Verify checklist generation rules are complete"
    - "Verify gates between phases defined"
    - "Verify error recovery documented"
  tracking:
    - "Verify progress display format defined"
    - "Verify checklist markers documented"
  quality:
    - "Verify OKRs are measurable"
    - "Verify deliverables clearly specified"
    - "Verify fault tolerance addressed"

always_last:
  - "Document verification results in ${SPEC_DIR}/skill-verification.md"
  - "Test skill execution"
```

---

# Procedures

## Procedure: Skill Requirements (skill-define)
**Purpose:** Define skill requirements and boundaries

**Steps:**
1. **Get skill name** - Short, descriptive, kebab-case (e.g., `feature-impl`)
2. **Get skill purpose** - One sentence describing what it does
3. **Determine skill type:**
   - **Atomic** - Does one thing, standalone
   - **Orchestrator** - Coordinates multiple atomic skills
   - **Review** - Validates output of another skill
4. **Define boundaries:**
   - When to use this skill (specific scenarios)
   - When NOT to use (and what to use instead)
5. **Identify inputs** - What does skill receive? (files, descriptions, specs)
6. **Identify outputs** - What does skill produce? (code, docs, specs)
7. **Identify prerequisites** - What must exist before skill runs?
8. **Identify related skills** - What composes with this skill?
9. **Identify risks and mitigations**
10. Save to ${SPEC_DIR}/skill-spec.md

**Override Points:**
- Skill spec template format
- Required sections

## Procedure: Skill Planning (skill-plan)
**Purpose:** Design skill phases and checklist generation rules

**Steps:**
1. **Break work into phases** - Each phase is a logical unit
2. **Define phase purpose** - What each phase accomplishes
3. **Define phase steps** - Concrete actions within each phase
4. **Define gates** - What must pass before next phase
5. **Design checklist generation:**
   - Base items (always included)
   - Conditional items (based on repo type, inputs)
   - Loop gates (when to retry)
   - Final items (always last)
6. **Design progress tracking:**
   - Progress display format
   - Checklist markers (`[ ]`, `[x]`, `[-]`)
   - Status reporting content
7. Save to ${SPEC_DIR}/skill-plan.md

**Override Points:**
- Phase granularity
- Progress tracking format
- Checklist marker style

## Procedure: Skill Implementation (skill-impl)
**Purpose:** Write SKILL.md content

**Steps:**
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
3. **Add examples** - Usage examples and edge cases
4. **Add progress tracking example** - Show what progress display looks like
5. Save to ${TOOL_CONFIG}/skills/[skill-name]/SKILL.md

**Override Points:**
- Section requirements (some may be optional for simple skills)

## Procedure: Skill Verification (skill-verify)
**Purpose:** Verify skill follows framework conventions

**Steps:**
1. **Review against skill-template.md** - All sections present?
2. **Check structure** - Frontmatter, section order
3. **Check content quality:**
   - Background explains why skill exists
   - Intent is clear and specific
   - Boundaries clearly defined
   - Prerequisites complete
   - Risks identified with mitigations
4. **Check process quality:**
   - Phases are logical and sequential
   - Checklist generation rules complete
   - Gates between phases defined
   - Error recovery documented
5. **Check progress tracking** - Format correct?
6. **Test execution** - Run skill, verify it works
7. Document verification results

**Override Points:**
- Verification criteria
- Quality thresholds

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before skill creation

**Steps:**
1. Check if similar skill already exists
2. Verify skill doesn't overlap with existing skills
3. Confirm skill scope is appropriate (not too broad/narrow)
4. Verify skill name follows conventions

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

**Suite structure:**
- `skill-workflow` orchestrator composes atomic skills
- Orchestrator's checklist is composed from child skill checklists
- Each atomic skill has its own progress tracking
- Verification skill (`skill-verify`) is unique to skill creation

**Composition pattern:**
The orchestrator's checklist is dynamically composed by:
1. Including base orchestrator items (read docs, detect state)
2. For each child skill phase, including that skill's checklist
3. Adding orchestrator-level gates and transitions
4. Including final orchestrator items (testing, documentation)

**Output locations:**
- `${TOOL_CONFIG}/skills/skill-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/skill-define/SKILL.md`
- `${TOOL_CONFIG}/skills/skill-plan/SKILL.md`
- `${TOOL_CONFIG}/skills/skill-impl/SKILL.md`
- `${TOOL_CONFIG}/skills/skill-verify/SKILL.md`

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

---

# Relationship to Feature Development

This guideline composes skills from [feature-development.md](feature-development.md):

| Skill Creation Skill | Based On | Additional Concerns |
|---------------------|----------|---------------------|
| `skill-define` | `feature-define` | Skill type, checklist inputs, related skills |
| `skill-plan` | `feature-plan` | Checklist generation rules, progress tracking design |
| `skill-impl` | `feature-impl` | SKILL.md sections, framework conventions |
| `skill-verify` | (new) | Framework compliance verification |

**Prerequisites:**
Before using this guideline, ensure the feature-development skills are set up. The skill creation skills extend those patterns with skill-specific concerns.
