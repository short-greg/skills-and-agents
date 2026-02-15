# Create Skill

---

# Overview

## Name
create-skill

## Background
Creating effective AI coding assistant skills requires understanding skill structure, checklist generation, and progress tracking. Skill creation is fundamentally similar to feature development - it requires defining requirements, planning structure, and implementing content. The difference is the domain-specific context: SKILL.md format, checklist generation rules, and framework conventions.

## Skill Intent
Provide a workflow that composes existing feature development skills with skill-specific context, producing complete SKILL.md files that follow framework conventions.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `create-skill` | End-to-end workflow from idea to working skill | Workflow |

### Skills Composed (from feature-development)

This workflow **reuses** existing skills:
- `feature-define` - Define requirements (with skill-specific context)
- `feature-plan` - Plan structure (with skill-specific structure)
- `feature-impl` - Implement content (writing SKILL.md)

## When to Use This Guideline
**Use this guideline when:**
- Setting up the skill creation workflow for a project
- Need to understand the skill creation process

**Do NOT use when:**
- Modifying existing skills (edit directly)
- Creating skill suites from a guideline template (use that guideline directly)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** New skills are complete, consistent, and effective on first use

**Key Results:**
1. 100% of created skills have all required sections
2. 100% of created skills include progress tracking
3. Skills execute without missing steps or phases

## Guideline Checklist
**Process to create this skill from this guideline:**

- [ ] 1. Read this guideline
- [ ] 2. Ensure feature-development skills exist (these are composed)
- [ ] 3. Interview user for skill-specific conventions
- [ ] 4. Write SKILL.md file:
  - `${TOOL_CONFIG}/skills/create-skill/SKILL.md`
- [ ] 5. Test skill execution

---

# Process

## Workflow: create-skill

This workflow composes existing feature-development skills with skill-specific context.

**Checklist:**
```markdown
- [ ] 1. Read project documentation and skill template
- [ ] 2. Run feature-define with skill context (add sub-checklist below)
- [ ] 3. Run feature-plan with skill context (add sub-checklist below)
- [ ] 4. Run feature-impl with skill context (add sub-checklist below)
- [ ] 5. Verify skill against framework conventions
- [ ] 6. Test skill execution
```

**Skill context passed to composed skills:**

For `feature-define`:
- Skill name (kebab-case)
- Skill type (atomic/orchestrator/workflow)
- What problem skill solves
- When to use / when NOT to use
- Inputs and outputs
- Prerequisites and related skills

For `feature-plan`:
- Phase structure
- Checklist generation rules (base, conditional, loop gates, final)
- Progress tracking format
- Error recovery approach

For `feature-impl`:
- SKILL.md format (frontmatter + sections)
- Required sections from skill-template.md
- Progress tracking display example

**Verification step (unique to skills):**
```markdown
- [ ] 5a. Verify frontmatter present (name, description, argument-hint)
- [ ] 5b. Verify all required sections present
- [ ] 5c. Verify checklist generation rules are complete
- [ ] 5d. Verify progress tracking format defined
- [ ] 5e. Verify OKRs are measurable
```

---

# Interview Questions

**Questions to ask when creating a new skill:**

## Core Identity
1. What is the skill name? (kebab-case, e.g., `feature-impl`)
2. What does this skill do in one sentence?
3. What type of skill is it? (atomic/orchestrator/workflow)

## Boundaries
4. When should someone use this skill?
5. When should they NOT use it? What should they use instead?
6. What are the prerequisites?

## Inputs and Outputs
7. What input does the skill receive?
8. What output does the skill produce?
9. Where are outputs saved?

## Process
10. What are the major phases?
11. What must be true before moving to the next phase?
12. What should happen if something fails?

---

# Skill Types Reference

## Atomic Skills
- Do one specific thing
- Standalone execution
- Clear single purpose
- Examples: `feature-define`, `bugfix-impl`

## Orchestrator Skills
- Coordinate multiple atomic skills
- Manage flow between skills
- Handle state across phases
- Examples: `feature-workflow`, `bugfix-workflow`

## Workflow Skills
- Like orchestrators but compose existing skills rather than defining new ones
- Pass domain-specific context to composed skills
- Examples: `create-example`, `create-skill`

---

# Customization by Repo Type

## Prototype
- Skills can skip comprehensive error recovery
- OKRs can be simplified

## Production
- Full error recovery required
- Complete OKRs with measurable thresholds

## Library
- Skills must consider API stability
- Document backward compatibility concerns

---

# Framework References

- [feature-development.md](feature-development.md) - Skills that are composed
- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist syntax
- [templates/skill-template.md](../templates/skill-template.md) - SKILL.md format

---

# Skill Output Template

Use [skill-template.md](../templates/skill-template.md) as the base.

**Output location:**
- `${TOOL_CONFIG}/skills/create-skill/SKILL.md`

**Key point:** This workflow composes existing skills. It doesn't create new atomic skills - it passes skill-specific context to `feature-define`, `feature-plan`, and `feature-impl`.

---

# Validation Checklist

**Before marking skill creation complete, verify:**

## Structure
- [ ] Frontmatter present (name, description, argument-hint)
- [ ] All sections from skill-template.md included

## Content
- [ ] Background explains why skill exists
- [ ] Intent is clear and specific
- [ ] When to Use has clear boundaries
- [ ] When NOT to Use lists alternatives
- [ ] Prerequisites are complete

## Process
- [ ] Phases are logical and sequential
- [ ] Checklist generation rules are complete
- [ ] Error recovery documented

## Progress Tracking
- [ ] Progress display format defined
- [ ] Checklist markers documented

## Quality
- [ ] OKRs are measurable
- [ ] Deliverables clearly specified
