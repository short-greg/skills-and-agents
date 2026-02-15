# Feature Development

---

# Overview

## Name
feature-development

## Background
Feature development requires structured progression through requirements, planning, and implementation. Without clear phases, AI assistants skip steps, miss requirements, or produce code that doesn't match expectations.

## Skill Intent
Enable consistent feature delivery from idea to tested code through a suite of coordinated skills.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `feature-workflow` | End-to-end orchestration from idea to code | Orchestrator |
| `feature-define` | Create PRD from feature ideas | Atomic |
| `feature-define-review` | Review and validate PRD | Review |
| `feature-plan` | Transform PRD into development plan | Atomic |
| `feature-plan-review` | Review and validate development plan | Review |
| `feature-impl` | Implement feature using TDD | Atomic |

### When to Use Each Skill

**Use `feature-workflow` when:**
- Starting a new feature from scratch
- Want complete orchestration with review gates
- Unsure what already exists (it detects and resumes)

**Use `feature-define` when:**
- Have a feature idea that needs documentation
- Need to create PRD before planning
- Want to align stakeholders on requirements

**Use `feature-plan` when:**
- Have an approved PRD
- Need technical design and task breakdown

**Use `feature-impl` when:**
- Have an approved development plan
- Ready to write code using TDD

**Use review skills when:**
- Need validation before proceeding
- Want feedback on document quality

## When to Use This Guideline
**Use this guideline when:**
- Setting up feature development workflow for a project
- Creating the full suite of feature skills
- Customizing feature workflow for specific conventions

**Do NOT use when:**
- Fixing bugs (use bugfixing guideline)
- Refactoring without new behavior (use refactoring guideline)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** Features ship with clear requirements, solid plans, and tested code

**Key Results:**
1. 100% of implementations have corresponding PRD
2. 100% of implementations follow approved plan
3. 90%+ test coverage on new code

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/feature-define/SKILL.md`
  - `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`
  - `${TOOL_CONFIG}/skills/feature-impl/SKILL.md`
  - (Optional) Review skill files
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type and conventions
2. Read existing specs (PRD, plan) to detect current state
3. Based on repo type, include/exclude quality gates
4. For each phase, add corresponding checklist items
5. Add loop gates for validation failures
6. End with documentation update item

**Orchestrator (`feature-workflow`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing PRD/plan documents"
  - "Determine starting point based on existing artifacts"

phase_items:
  define:
    - "Run feature-define skill"
    - "(If production) Run feature-define-review"
  plan:
    - "Run feature-plan skill"
    - "(If production) Run feature-plan-review"
  implement:
    - "Run feature-impl skill"

loop_gates:
  - trigger: "review_rejected"
    action: "go to previous skill for revision"

always_last:
  - "Update project documentation if new patterns established"
```

**Atomic skill (`feature-define`) checklist generation:**
```yaml
base_items:
  - "Read project documentation for conventions"
  - "Gather feature description from user"
  - "Research existing code/patterns"

conditional_items:
  - condition: "repo_type == 'library'"
    items:
      - "Define public API changes"

always_last:
  - "Write PRD to ${SPEC_DIR}/prd.md"
```

---

# Procedures

## Procedure: Requirements Definition (feature-define)
**Purpose:** Transform feature idea into structured PRD

**Steps:**
1. Gather feature description from user
2. Ask clarifying questions about scope
3. Research existing code/patterns
4. Write PRD with sections: overview, requirements, acceptance criteria
5. Save to ${SPEC_DIR}/prd.md

**Override Points:**
- PRD template format
- Required sections

## Procedure: Development Planning (feature-plan)
**Purpose:** Transform PRD into implementation plan

**Steps:**
1. Read approved PRD
2. Identify affected files and components
3. Break down into sequential tasks
4. Identify test strategy
5. Save to ${SPEC_DIR}/plan.md

**Override Points:**
- Task granularity
- Test requirements

## Procedure: TDD Implementation (feature-impl)
**Purpose:** Implement feature following test-driven development

**Steps:**
1. Read approved plan
2. For each task: write test → implement → verify
3. Run full test suite
4. Update documentation

**Override Points:**
- Test framework
- Coverage requirements

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes
2. Verify prerequisite documents exist
3. Assess impact on existing code

**Override Points:**
- Risk thresholds

---

# Customization by Repo Type

## Prototype
- PRD can be minimal (requirements list only)
- Plan can skip detailed architecture
- Tests optional
- Review skills optional

## Production
- Full PRD required
- Plan must include architecture decisions
- Tests required with coverage thresholds
- Review skills recommended

## Library
- PRD must include API design
- Plan must address backward compatibility
- Tests required with examples
- Changelog updates required

---

# Framework References

- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist syntax
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria
- [references/spec-maintenance.md](../references/spec-maintenance.md) - Progress tracking

---

# Skill Output Template

Use [skill-template.md](../templates/skill-template.md) as the base for creating each SKILL.md file.

**During the interview process, fill in these sections for each skill:**

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
14. **Related Skills** - connections to other skills in suite

**Suite structure:**
- `feature-workflow` orchestrator calls atomic skills
- Each atomic skill is standalone but designed to work in sequence
- Review skills gate progression between phases

**Output locations:**
- `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/feature-define/SKILL.md`
- `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`
- `${TOOL_CONFIG}/skills/feature-impl/SKILL.md`
