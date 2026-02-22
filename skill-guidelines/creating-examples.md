# Creating Examples

---

# Overview

## Name
creating-examples

## Background
Educational content and framework demonstrations require structured creation. Creating examples is fundamentally a feature development task - it requires defining requirements, planning structure, and implementing working code. The difference is the additional context: learning objectives, educational value, and documentation that explains WHY.

## Skill Intent
Provide a workflow that composes existing feature development skills with example-specific context, producing working examples with educational documentation.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `create-example` | End-to-end workflow from idea to working example | Workflow |
| `create-tutorial` | End-to-end workflow for tutorial creation | Workflow |

### Skills Composed (from feature-development)

These workflows **reuse** existing skills:
- `feature-define` - Define requirements (with example-specific context)
- `feature-plan` - Plan structure (with example-specific structure)
- `feature-impl` - Implement code (with example-specific documentation)

### When to Use Each Skill

**Use `create-example` when:**
- Building framework examples from scratch
- Need working, runnable code that demonstrates a concept

**Use `create-tutorial` when:**
- Creating step-by-step educational content
- Primary output is documentation, not code

## When to Use This Guideline
**Use this guideline when:**
- Setting up example/tutorial creation workflows
- Customizing example conventions for specific frameworks

**Do NOT use when:**
- Implementing production features (use feature-development)
- Fixing bugs in existing examples (use bugfixing)

## Guideline OKRs
[See [protocols/goals_and_objectives.md](../protocols/goals_and_objectives.md)]

**Objective:** Educational content enables effective learning with working examples

**Key Results:**
1. 100% of examples are runnable and self-contained
2. All examples have clear learning objectives documented
3. All code tested and verified working

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline
- [ ] 2. Ensure feature-development skills exist (these are composed)
- [ ] 3. Interview user for example-specific conventions
- [ ] 4. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/create-example/SKILL.md`
  - `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`
- [ ] 5. Test skill execution

---

# Process

## Workflow: create-example

This workflow composes existing feature-development skills with example-specific context.

**Checklist:**
```markdown
- [ ] 1. Read project documentation and detect example conventions
- [ ] 2. Run feature-define with example context (add sub-checklist below)
- [ ] 3. Run feature-plan with example context (add sub-checklist below)
- [ ] 4. Run feature-impl with example context (add sub-checklist below)
- [ ] 5. Assess educational value
- [ ] 6. Register example and update documentation
```

**Example context passed to composed skills:**

For `feature-define`:
- Learning objectives (what reader will understand)
- Prerequisites (what reader should know)
- Complexity level (beginner/intermediate/advanced)
- What concept/pattern the example demonstrates

For `feature-plan`:
- Example directory structure conventions
- Example naming convention (e.g., `example{N}_{name}`)
- Demo script design
- Expected output

For `feature-impl`:
- Inline documentation explaining WHY (not just what)
- README.md with concept explanation
- Self-contained verification
- Tests that demonstrate usage

**Assessment step (unique to examples):**
```markdown
- [ ] 5a. Verify learning objectives are achievable
- [ ] 5b. Verify example demonstrates stated concepts
- [ ] 5c. Verify documentation explains WHY not just HOW
- [ ] 5d. Verify example is runnable and self-contained
- [ ] 5e. Verify example is appropriately scoped
```

## Workflow: create-tutorial

This workflow is simpler - tutorials are primarily documentation with code samples.

**Checklist:**
```markdown
- [ ] 1. Read project documentation and detect tutorial conventions
- [ ] 2. Define tutorial scope
  - [ ] 2a. Learning goals
  - [ ] 2b. Prerequisites
  - [ ] 2c. Step sequence with milestones
- [ ] 3. Write tutorial content
  - [ ] 3a. Introduction with learning objectives
  - [ ] 3b. Each step with explanations (WHY not just WHAT)
  - [ ] 3c. Code samples for each step
  - [ ] 3d. Checkpoints for reader verification
- [ ] 4. Test all code samples
- [ ] 5. Update documentation index
```

---

# Interview Questions

**Questions to ask when setting up these workflows:**

## For create-example:
1. Where are examples stored? (`examples/`, `src/examples/`, etc.)
2. What is the example naming convention? (e.g., `example{N}_{name}`)
3. What structure do examples follow? (files, directories)
4. How do examples integrate with the main application?
5. Is there a framework being demonstrated?
6. Are tests required for examples?

## For create-tutorial:
1. Where are tutorials stored? (`docs/tutorials/`, etc.)
2. What format are tutorials in? (Markdown, notebook, etc.)
3. What is the target audience skill level?
4. Should tutorials reference working examples?

---

# Customization by Repo Type

## Prototype
- Examples can skip tests
- Documentation can be minimal
- Integration optional

## Production
- Tests required for examples
- Full README documentation
- Integration with main app required

## Library
- Examples must demonstrate public API
- Examples serve as documentation
- Tests required showing usage patterns

---

# Framework References

- [feature-development.md](feature-development.md) - Skills that are composed
- [protocols/goals_and_objectives.md](../protocols/goals_and_objectives.md) - OKR patterns
- [protocols/checklist_management.md](../protocols/checklist_management.md) - Checklist syntax

---

# Key Differences: Example vs Tutorial

| Aspect | create-example | create-tutorial |
|--------|----------------|-----------------|
| Primary output | Working code | Educational content |
| Code role | The deliverable | Illustrates concepts |
| Documentation | Explains the code | Is the main content |
| Testing | Code must run | Steps must be followable |
| Composes | feature-define → feature-plan → feature-impl | (simpler, no composition) |

**Decision guide:**
- Need runnable demonstration? → `create-example`
- Need to teach step-by-step? → `create-tutorial`
- Need both? → Create example first, then tutorial that references it

---

# Skill Output Template

Use [skill-template.md](../templates/skill-template.md) as the base.

**Output locations:**
- `${TOOL_CONFIG}/skills/create-example/SKILL.md`
- `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`

**Key point:** These workflows compose existing skills. They don't create new atomic skills - they pass example-specific context to `feature-define`, `feature-plan`, and `feature-impl`.
