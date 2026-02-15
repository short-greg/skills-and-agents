Purpose: This document defines how to create a guideline for setting up a particular type of skill.

**IMPORTANT: Guidelines are setup guides, NOT pre-filled skills.**
- Do NOT write complete SKILL.md content in the guideline
- Do NOT pre-fill all sections from skill-template.md
- DO describe procedures that map to skills in a suite
- DO define the full skill suite (orchestrator + atomic skills)
- DO reference skill-template.md for users to fill during interview
- See feature-development.md as the canonical example

[TEMPLATE]

# [Guideline Name]

---

# Overview

## Name
[Skill/workflow name - e.g., "feature-development", "bugfixing"]

## Background
[Context and motivation. Why does this guideline exist? What problems led to its creation?]

## Skill Intent
[What problem does this skill solve? What workflow does it enable through a suite of coordinated skills?]

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `[name]-workflow` | [End-to-end orchestration description] | Orchestrator |
| `[name]-[action1]` | [What this atomic skill does] | Atomic |
| `[name]-[action2]` | [What this atomic skill does] | Atomic |
| `[name]-[action3]` | [What this atomic skill does] | Atomic |

### When to Use Each Skill

**Use `[name]-workflow` when:**
- [Use case for orchestrator]

**Use `[name]-[action1]` when:**
- [Use case for this atomic skill]

**Use `[name]-[action2]` when:**
- [Use case for this atomic skill]

## When to Use This Guideline
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
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/[name]-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/[name]-[action1]/SKILL.md`
  - `${TOOL_CONFIG}/skills/[name]-[action2]/SKILL.md`
  - `${TOOL_CONFIG}/skills/[name]-[action3]/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read top-level coding agent documentation (e.g., CLAUDE.md, AGENTS.md) to determine repo type and conventions
2. Read existing artifacts to detect current state
3. Based on repo type, include/exclude quality gate items
4. For each phase, add corresponding checklist items
5. Add loop gates for validation failures (tests, coverage, etc.)
6. End with documentation update item

**Orchestrator (`[name]-workflow`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md and confirm conventions"
  - "Check for existing artifacts"
  - "Determine starting point based on existing artifacts"

phase_items:
  [phase1]:
    - "Run [name]-[action1] skill"
  [phase2]:
    - "Run [name]-[action2] skill"
  [phase3]:
    - "Run [name]-[action3] skill"

loop_gates:
  - trigger: "[failure condition]"
    action: "go to [previous skill]"

always_last:
  - "Update CLAUDE.md if new patterns established"
```

**Atomic skill (`[name]-[action1]`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md for conventions"
  - "[Skill-specific base items]"

conditional_items:
  - condition: "repo_type == 'production'"
    items:
      - "[Production-specific items]"

always_last:
  - "Write output to ${SPEC_DIR}/[output].md"
```

---

# Procedures

[Each procedure maps to an atomic skill in the suite]

## Procedure: [Name] ([skill-name])
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

**Override Points:**
- Additional risk checks for specific domains

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

Use [skill-template.md](skill-template.md) as the base for creating each SKILL.md file.

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
- `[name]-workflow` orchestrator calls atomic skills
- Each atomic skill is standalone but designed to work in sequence
- [Describe any loops or conditional flows]

**Output locations:**
- `${TOOL_CONFIG}/skills/[name]-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/[name]-[action1]/SKILL.md`
- `${TOOL_CONFIG}/skills/[name]-[action2]/SKILL.md`
- `${TOOL_CONFIG}/skills/[name]-[action3]/SKILL.md`
