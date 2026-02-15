# Creating Examples

---

# Overview

## Name
creating-examples

## Background
Educational content and framework demonstrations require structured creation to ensure quality, consistency, and effective learning outcomes. Without clear processes, examples become inconsistent, tutorials skip steps, and learners struggle to follow along.

## Skill Intent
Enable consistent creation of educational content through coordinated skills for building working code examples and step-by-step tutorials.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `create-example` | Create working code examples with implementation, tests, docs, and integration | Atomic |
| `create-tutorial` | Create step-by-step tutorial content with progressive learning | Atomic |

### When to Use Each Skill

**Use `create-example` when:**
- Building framework examples
- Demonstrating specific concepts or patterns
- Creating working code demonstrations
- Need runnable, self-contained examples
- Want to showcase framework features

**Use `create-tutorial` when:**
- Creating step-by-step educational content
- Teaching concepts progressively
- Building learning paths
- Need detailed explanations with code
- Want to guide learners through a process

## When to Use This Guideline
**Use this guideline when:**
- Setting up educational content creation workflow
- Creating example or tutorial skills for a project
- Customizing example conventions for specific frameworks

**Do NOT use when:**
- Implementing production features (use feature-development)
- Fixing bugs in existing examples (use bugfixing)
- Refactoring example code (use refactoring)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** Educational content enables effective learning with working, well-documented examples

**Key Results:**
1. 100% of examples are runnable and self-contained
2. 100% of tutorials have clear learning objectives
3. All code samples tested and verified working

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/create-example/SKILL.md`
  - `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type, examples directory, and conventions
2. Read existing examples to detect naming patterns and structure
3. Based on repo type, include/exclude quality gates (tests, grievances)
4. For each phase, add corresponding checklist items
5. Add loop gates for test failures
6. End with integration and documentation update items

**Atomic skill (`create-example`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Read example spec or gather description"
  - "Identify next example number"
  - "Research framework and existing examples"

phase_items:
  design:
    - "Plan file structure and components"
    - "Design example flow"
  implement:
    - "Create directory structure"
    - "Implement main code"
    - "Implement demo script"
  test:
    - "Write tests (if applicable)"
    - "Run example manually"
    - "Verify output"
  document:
    - "Write README.md"
    - "Add inline documentation"
  integrate:
    - "Register with main application"
    - "Test integration"

conditional_items:
  - condition: "has_grievances_process"
    items:
      - "Document friction points in GRIEVANCES.md"

always_last:
  - "Verify example runs successfully"
```

**Atomic skill (`create-tutorial`) checklist generation:**
```yaml
base_items:
  - "Read project documentation for conventions"
  - "Gather tutorial topic from user"
  - "Research related documentation"

phase_items:
  objectives:
    - "Define learning goals"
    - "Define prerequisites"
    - "Determine scope"
  outline:
    - "Design step sequence"
    - "Define milestones"
    - "Plan code evolution"
  content:
    - "Write introduction"
    - "Write each step with explanations"
    - "Add checkpoints"
  code:
    - "Create code samples for each step"
    - "Test all code samples"
  visuals:
    - "Identify visual opportunities"
    - "Create diagrams/screenshots"
  review:
    - "Test code samples end-to-end"
    - "Verify learning objectives achieved"

always_last:
  - "Proofread and verify completeness"
```

---

# Procedures

## Procedure: Example Creation (create-example)
**Purpose:** Create complete framework example with implementation, tests, documentation, and integration

**Steps:**
1. **Intake** - Read spec or gather description, identify example number and name
2. **Research** - Study framework docs and existing examples
3. **Design** - Plan file structure, components, and demo flow
4. **Implementation** - Build working code following conventions
5. **Testing** - Write tests, verify example runs correctly
6. **Documentation** - Create README with concept explanation and usage
7. **Integration** - Register example with main application
8. **Grievances** (optional) - Document framework friction points

**Override Points:**
- Example naming convention (e.g., `example{N}_{name}`)
- Example directory structure
- Registration process with main app
- Whether grievances are collected

## Procedure: Tutorial Creation (create-tutorial)
**Purpose:** Create step-by-step tutorial with progressive learning

**Steps:**
1. **Define Objectives** - Identify learning goals, prerequisites, scope
2. **Outline Steps** - Design progressive step sequence with milestones
3. **Write Content** - Create explanations focusing on WHY not just WHAT
4. **Create Code** - Build working, tested code samples for each step
5. **Add Visuals** - Create diagrams, screenshots where helpful
6. **Review** - Test end-to-end, verify objectives achieved

**Override Points:**
- Tutorial location (docs/, tutorials/, etc.)
- Code sample format
- Visual requirements

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes in examples directory
2. Verify framework documentation is accessible
3. Check for naming conflicts with existing examples
4. Assess impact on main app registration

**Override Points:**
- Additional domain-specific risk checks

---

# Customization by Repo Type

## Prototype
- Examples can skip tests
- Documentation can be minimal
- Integration optional
- No grievances collection

## Production
- Tests required for examples
- Full README documentation
- Integration with main app required
- Grievances optional but recommended

## Library
- Examples must demonstrate public API
- Examples serve as documentation
- Tests required showing usage patterns
- Multiple examples per feature encouraged

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
- Both skills are atomic (no orchestrator needed)
- `create-example` produces working code with documentation
- `create-tutorial` produces educational content with code samples
- Skills can reference each other's outputs

**Output locations:**
- `${TOOL_CONFIG}/skills/create-example/SKILL.md`
- `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`

---

# Interview Questions

**Questions to ask when customizing these skills:**

## For create-example:
1. Where are examples stored? (`examples/`, `src/examples/`, etc.)
2. What is the example naming convention? (e.g., `example{N}_{name}`)
3. What structure do examples follow? (files, directories)
4. How do examples integrate with the main application?
5. Is there a framework being demonstrated?
6. Are tests required for examples?
7. Is there a grievances/feedback collection process?

## For create-tutorial:
1. Where are tutorials stored? (`docs/tutorials/`, etc.)
2. What format are tutorials in? (Markdown, notebook, etc.)
3. What is the target audience skill level?
4. Are visual aids required or optional?
5. Should tutorials reference working examples?

---

# Example Directory Detection

**How to detect example conventions in a repository:**

```bash
# Find examples directory
for dir in examples src/examples lib/examples docs/examples samples; do
  if [ -d "$dir" ]; then
    echo "Found examples directory: $dir"
    EXAMPLES_DIR="$dir"
    break
  fi
done

# Review existing examples to understand pattern
ls -la ${EXAMPLES_DIR}/

# Typical structure to detect:
# examples/
# ├── example1_name/
# │   ├── __init__.py or main.py
# │   ├── README.md
# │   └── tests/ (optional)

# Find main application entry point
for file in app.py main.py __main__.py src/main.py; do
  if [ -f "$file" ]; then
    echo "Found main app: $file"
    MAIN_APP="$file"
    break
  fi
done

# Check for framework directory
ls -la framework/ 2>/dev/null && echo "Framework: framework/"
```

---

# Key Differences: Example vs Tutorial

| Aspect | create-example | create-tutorial |
|--------|----------------|-----------------|
| Primary output | Working code | Educational content |
| Code role | The deliverable | Illustrates concepts |
| Documentation | Explains the code | Is the main content |
| Structure | Files in directory | Sequential steps |
| Testing | Code must run | Steps must be followable |
| Integration | With main app | With docs/examples |

**Decision guide:**
- Need runnable demonstration? → `create-example`
- Need to teach step-by-step? → `create-tutorial`
- Need both? → Create example first, then tutorial that references it
