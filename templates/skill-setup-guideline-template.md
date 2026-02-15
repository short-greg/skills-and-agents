Purpose: This document defines how to create a guideline for setting up a particular type of skill.

[TEMPLATE]

# [Guideline Name]

---

# Overview

## Name
[Skill/workflow name - e.g., "feature-development", "bugfixing"]

## Background
[Context and motivation. Why does this guideline exist? What problems led to its creation?]

## Skill Intent
[What problem does this skill solve? What workflow does it enable?]

## When to Use
**Use this guideline when:**
- [Use case 1]
- [Use case 2]

**Do NOT use when:**
- [Anti-use case 1]
- [Anti-use case 2]

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** [Aspirational outcome for skills created from this guideline]

**Key Results:**
1. [Measurable outcome 1]
2. [Measurable outcome 2]
3. [Measurable outcome 3]

## Guideline Checklist
**Process to create a skill from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Determine: single skill or workflow (multiple skills)?
- [ ] 3. Interview user until [skill-template.md](skill-template.md) is filled for each skill
  - Ask about: Role, OKRs, phases, checklist rules, etc.
  - (If workflow) Define orchestrator + atomic skills
  - See "Skill Output Template" section below for full list of sections to fill
- [ ] 4. Define command name(s) following naming conventions
- [ ] 5. Customize for repo type (prototype/production/library)
- [ ] 6. Confirm skill design with user
- [ ] 7. Write SKILL.md file(s) to ${TOOL_CONFIG}/skills/[name]/
- [ ] 8. Test skill execution

---

# Process

**How the skill creates its checklist on execution:**

[Describe the steps the skill takes to generate a dynamic checklist based on context. This is NOT a static checklist - it's instructions for HOW to build one.]

**Prose description:**
1. Read top-level coding agent documentation (e.g., CLAUDE.md, AGENTS.md) to determine repo type and conventions
2. (If working in a specific package) Read that package's coding agent documentation if it exists
3. Based on repo type, include/exclude quality gate items
4. For each entity in input (files, features, etc.), add corresponding checklist items
5. Add loop gates for validation failures (tests, coverage, etc.)
6. Always end with progress update item
7. (If skill adds, removes, or edits code) Include step to update coding agent documentation using templates from [coding-agent-doc-template.md](coding-agent-doc-template.md) and [coding-agent-doc-sub-level-template.md](coding-agent-doc-sub-level-template.md)

**YAML example (where helpful):**
```yaml
base_items:
  - "Read CLAUDE.md and understand conventions"
  - "Confirm understanding with user"

conditional_items:
  - condition: "repo_type == 'production'"
    items:
      - "Run security scans"
      - "Run modularity assessment"

loop_gates:
  - trigger: "tests_fail"
    action: "go to step [run_tests]"

always_last:
  - "Update ${SPEC_DIR}/plan.md with progress"
```

**Checklist Syntax (for generated checklists):**
```
- [ ] 1. Item description
- [ ] 2. (If condition) Conditional item
- [ ] 3! (If condition) Loop gate - go to step N
```

**Markers:** `[ ]` pending, `[x]` done, `[-]` skipped, `!` = loop gate

---

# Procedures

[Reusable logic blocks that can be overridden when creating the skill]

## Procedure: [Name]

**Purpose:** [What this procedure accomplishes]

**Steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Override Points:**
- [What can be customized]

## Procedure: Evaluate Risks

**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes
2. Verify prerequisites exist
3. Assess impact on existing code
4. [Domain-specific risk checks]

**Override Points:**
- Additional risk checks for specific domains
- Threshold adjustments

---

# Customization by Repo Type

## Prototype
- [Simplified requirements]
- [Lower thresholds]
- [What to skip]

## Production
- [Full requirements]
- [Standard thresholds]
- [All quality gates]

## Library
- [Extra API requirements]
- [Higher thresholds]
- [Compatibility checks]

---

# Framework References

- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist syntax
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria
- [references/spec-maintenance.md](../references/spec-maintenance.md) - Progress tracking

---

# Skill Output Template

Use [skill-template.md](skill-template.md) as the base for creating SKILL.md files.

**During the interview process, fill in these sections from skill-template.md:**

1. **Frontmatter** - name, description, argument-hint
2. **Background** - why this skill exists
3. **Intent** - what it accomplishes
4. **Role** - persona and reasoning approach
5. **When to Use** - boundaries and use cases
6. **Command** - name, arguments, examples
7. **OKRs** - measurable outcomes
8. **Prerequisites** - what must exist
9. **Risks** - inherent risks and mitigations
10. **Process** - checklist generation rules, phases, input handling, error recovery, documentation updates
11. **Deliverables** - outputs produced
12. **Reporting/Transparency** - progress tracking format
13. **Fault Tolerance** - rollback, partial completion, recovery
14. **Related Skills** - what it works with

**For workflows (multiple skills):**
- Create one SKILL.md per atomic skill
- Create one SKILL.md for the orchestrator
- Orchestrator references atomic skills in "Related Skills"

**Output location:** `${TOOL_CONFIG}/skills/[skill-name]/SKILL.md`
