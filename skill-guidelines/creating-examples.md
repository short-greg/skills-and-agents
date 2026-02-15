# Creating Examples

---

# Overview

## Name
creating-examples

## Background
Educational content and framework demonstrations require structured creation to ensure quality, consistency, and effective learning outcomes. Without clear processes, examples become inconsistent, tutorials skip steps, and learners struggle to follow along. Creating examples is fundamentally a feature development task - it requires defining what the example teaches (requirements), planning the structure (design), and implementing working code with tests.

## Skill Intent
Enable consistent creation of educational content through a workflow that reuses existing feature development skills, adding example-specific concerns like learning objectives, educational value assessment, and integration with documentation.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `example-workflow` | End-to-end orchestration from idea to working example | Orchestrator |
| `example-define` | Define what the example teaches (uses feature-define pattern) | Atomic |
| `example-plan` | Plan example structure (uses feature-plan pattern) | Atomic |
| `example-impl` | Implement example with tests (uses feature-impl pattern) | Atomic |
| `example-assess` | Assess educational value and completeness | Atomic |
| `tutorial-workflow` | End-to-end orchestration for tutorial creation | Orchestrator |
| `tutorial-define` | Define learning objectives and audience | Atomic |
| `tutorial-impl` | Write tutorial content with code samples | Atomic |

### When to Use Each Skill

**Use `example-workflow` when:**
- Building framework examples from scratch
- Want complete orchestration with quality gates
- Unsure what already exists (it detects and resumes)

**Use `example-define` when:**
- Have an example idea that needs documentation
- Need to clarify what concept the example demonstrates
- Want to define learning objectives before building

**Use `example-plan` when:**
- Have approved example requirements
- Need to design file structure and components

**Use `example-impl` when:**
- Have approved example plan
- Ready to write code using TDD

**Use `example-assess` when:**
- Example is implemented
- Need to verify educational value and completeness

**Use `tutorial-workflow` when:**
- Creating step-by-step educational content
- Want complete orchestration for tutorial creation

**Use `tutorial-define` when:**
- Need to define tutorial scope and prerequisites
- Want to plan learning progression

**Use `tutorial-impl` when:**
- Have approved tutorial plan
- Ready to write content with code samples

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
4. Educational value assessed and documented

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Ensure feature-development skills exist (reused by this workflow)
- [ ] 3. Interview user for example-specific skills (see Skill Output Template)
- [ ] 4. Customize for repo type (prototype/production/library)
- [ ] 5. Confirm skill designs with user
- [ ] 6. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/example-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/example-define/SKILL.md`
  - `${TOOL_CONFIG}/skills/example-plan/SKILL.md`
  - `${TOOL_CONFIG}/skills/example-impl/SKILL.md`
  - `${TOOL_CONFIG}/skills/example-assess/SKILL.md`
  - `${TOOL_CONFIG}/skills/tutorial-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/tutorial-define/SKILL.md`
  - `${TOOL_CONFIG}/skills/tutorial-impl/SKILL.md`
- [ ] 7. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type, examples directory, and conventions
2. Read existing examples to detect naming patterns and structure
3. Check for existing example specs to determine starting point
4. Based on repo type, include/exclude quality gates (tests, assessment)
5. For each phase, add corresponding checklist items
6. Add loop gates for test failures or assessment issues
7. End with integration and documentation update items

**Orchestrator (`example-workflow`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing example specs"
  - "Determine starting point based on existing artifacts"

phase_items:
  define:
    - "Run example-define skill"
    - "(If production) Review example requirements"
  plan:
    - "Run example-plan skill"
    - "(If production) Review example design"
  implement:
    - "Run example-impl skill"
  assess:
    - "Run example-assess skill"
  integrate:
    - "Register example with main application"
    - "Update examples index/listing"

loop_gates:
  - trigger: "review_rejected"
    action: "go to previous skill for revision"
  - trigger: "assessment_failed"
    action: "go to example-impl to address issues"

always_last:
  - "Update project documentation if new patterns established"
```

**Atomic skill (`example-define`) checklist generation:**

This skill extends `feature-define` with example-specific concerns.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Gather example description from user"
  - "Research existing examples for patterns"

phase_items:
  requirements:
    - "Define what concept the example demonstrates"
    - "Define learning objectives"
    - "Define prerequisites (what reader should know)"
    - "Define expected outcome (what reader will learn)"
  scope:
    - "Determine complexity level (beginner/intermediate/advanced)"
    - "Identify related examples to reference"
    - "Define example boundaries (what's NOT covered)"

conditional_items:
  - condition: "repo_type == 'library'"
    items:
      - "Define which public APIs are demonstrated"

always_last:
  - "Write example spec to ${SPEC_DIR}/example-spec.md"
```

**Atomic skill (`example-plan`) checklist generation:**

This skill extends `feature-plan` with example-specific structure.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Read approved example spec"
  - "Identify next example number"

phase_items:
  structure:
    - "Plan directory structure"
    - "Plan file organization"
    - "Design component layout"
  flow:
    - "Design example execution flow"
    - "Plan demonstration script"
    - "Define expected output"
  tests:
    - "Plan test strategy for example"
    - "Identify edge cases to demonstrate"

always_last:
  - "Write example plan to ${SPEC_DIR}/example-plan.md"
```

**Atomic skill (`example-impl`) checklist generation:**

This skill extends `feature-impl` with example-specific implementation.

```yaml
base_items:
  - "Read project documentation for conventions"
  - "Read approved example plan"
  - "Create example directory"

phase_items:
  implement:
    - "Implement main example code"
    - "Implement demo/run script"
    - "Add inline documentation explaining WHY"
  test:
    - "Write tests for example"
    - "Run example manually and verify output"
    - "Verify example is self-contained"
  document:
    - "Write README.md with concept explanation"
    - "Add usage instructions"
    - "Document expected output"

conditional_items:
  - condition: "has_grievances_process"
    items:
      - "Document friction points in GRIEVANCES.md"

always_last:
  - "Verify example runs successfully end-to-end"
```

**Atomic skill (`example-assess`) checklist generation:**

This skill is unique to examples - assesses educational value.

```yaml
base_items:
  - "Read example spec and plan"
  - "Run example to understand behavior"

assessment_items:
  educational_value:
    - "Verify learning objectives are achievable"
    - "Verify example demonstrates stated concepts"
    - "Verify documentation explains WHY not just HOW"
  completeness:
    - "Verify example is runnable and self-contained"
    - "Verify tests pass"
    - "Verify README is complete"
  quality:
    - "Verify code follows project conventions"
    - "Verify example is appropriately scoped (not too complex)"
    - "Verify example builds on prerequisites correctly"

always_last:
  - "Document assessment results in ${SPEC_DIR}/example-assessment.md"
```

**Orchestrator (`tutorial-workflow`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing tutorial specs"
  - "Determine starting point based on existing artifacts"

phase_items:
  define:
    - "Run tutorial-define skill"
    - "(If production) Review tutorial plan"
  implement:
    - "Run tutorial-impl skill"
  assess:
    - "Verify learning objectives achieved"
    - "Test all code samples end-to-end"

loop_gates:
  - trigger: "review_rejected"
    action: "go to tutorial-define for revision"
  - trigger: "code_samples_fail"
    action: "go to tutorial-impl to fix"

always_last:
  - "Update documentation index"
```

**Atomic skill (`tutorial-define`) checklist generation:**
```yaml
base_items:
  - "Read project documentation for conventions"
  - "Gather tutorial topic from user"
  - "Research related documentation"

phase_items:
  objectives:
    - "Define learning goals"
    - "Define prerequisites"
    - "Determine scope and complexity level"
  outline:
    - "Design step sequence"
    - "Define milestones/checkpoints"
    - "Plan code evolution through tutorial"
  references:
    - "Identify related examples to reference"
    - "Identify documentation to link"

always_last:
  - "Write tutorial spec to ${SPEC_DIR}/tutorial-spec.md"
```

**Atomic skill (`tutorial-impl`) checklist generation:**
```yaml
base_items:
  - "Read project documentation for conventions"
  - "Read approved tutorial spec"

phase_items:
  content:
    - "Write introduction with learning objectives"
    - "Write each step with explanations (WHY not just WHAT)"
    - "Add checkpoints for reader verification"
  code:
    - "Create code samples for each step"
    - "Test all code samples"
    - "Verify code progression is logical"
  visuals:
    - "Identify visual opportunities"
    - "(If needed) Create diagrams/screenshots"
  review:
    - "Test code samples end-to-end"
    - "Verify learning objectives achieved"
    - "Proofread and verify completeness"

always_last:
  - "Write tutorial to ${TUTORIAL_DIR}/[tutorial-name].md"
```

---

# Procedures

## Procedure: Example Requirements (example-define)
**Purpose:** Define what the example teaches and its learning objectives

**Steps:**
1. Gather example description from user
2. Identify what concept/pattern the example demonstrates
3. Define learning objectives (what reader will understand after)
4. Define prerequisites (what reader should know before)
5. Determine complexity level (beginner/intermediate/advanced)
6. Identify related examples for context
7. Save to ${SPEC_DIR}/example-spec.md

**Override Points:**
- Example spec template format
- Required sections

## Procedure: Example Planning (example-plan)
**Purpose:** Design example structure and execution flow

**Steps:**
1. Read approved example spec
2. Determine example number and naming
3. Design directory structure and file organization
4. Design execution flow and demonstration script
5. Define expected output
6. Plan test strategy
7. Save to ${SPEC_DIR}/example-plan.md

**Override Points:**
- Example naming convention (e.g., `example{N}_{name}`)
- Example directory structure
- Registration process with main app

## Procedure: Example Implementation (example-impl)
**Purpose:** Implement example with tests and documentation

**Steps:**
1. Read approved example plan
2. Create directory structure
3. Implement main example code with inline documentation
4. Implement demo/run script
5. Write tests
6. Run example manually and verify output
7. Write README.md with concept explanation and usage
8. (Optional) Document framework friction in GRIEVANCES.md

**Override Points:**
- Test framework
- Documentation style
- Whether grievances are collected

## Procedure: Educational Assessment (example-assess)
**Purpose:** Verify example achieves educational goals

**Steps:**
1. Review learning objectives from spec
2. Run example and verify it demonstrates stated concepts
3. Review documentation for WHY explanations (not just HOW)
4. Verify example is appropriately scoped
5. Verify example builds on prerequisites correctly
6. Document assessment results

**Override Points:**
- Assessment criteria
- Quality thresholds

## Procedure: Tutorial Definition (tutorial-define)
**Purpose:** Define learning objectives and content structure

**Steps:**
1. Gather tutorial topic from user
2. Define learning goals
3. Define prerequisites
4. Determine scope and complexity level
5. Design step sequence with milestones
6. Plan code evolution through tutorial
7. Identify related examples to reference
8. Save to ${SPEC_DIR}/tutorial-spec.md

**Override Points:**
- Tutorial spec template
- Step granularity

## Procedure: Tutorial Implementation (tutorial-impl)
**Purpose:** Write tutorial content with tested code samples

**Steps:**
1. Read approved tutorial spec
2. Write introduction with learning objectives
3. Write each step with explanations (focus on WHY)
4. Create code samples for each step
5. Test all code samples
6. Add checkpoints for reader verification
7. Create diagrams/screenshots where helpful
8. Proofread and verify completeness
9. Save to ${TUTORIAL_DIR}/[tutorial-name].md

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
- `example-workflow` orchestrator composes atomic skills
- Orchestrator's checklist is composed from child skill checklists
- Each atomic skill has its own progress tracking
- Assessment skill (`example-assess`) is unique to examples
- `tutorial-workflow` follows similar composition pattern

**Composition pattern:**
The orchestrator's checklist is dynamically composed by:
1. Including base orchestrator items (read docs, detect state)
2. For each child skill phase, including that skill's checklist
3. Adding orchestrator-level gates and transitions
4. Including final orchestrator items (integration, documentation)

**Output locations:**
- `${TOOL_CONFIG}/skills/example-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/example-define/SKILL.md`
- `${TOOL_CONFIG}/skills/example-plan/SKILL.md`
- `${TOOL_CONFIG}/skills/example-impl/SKILL.md`
- `${TOOL_CONFIG}/skills/example-assess/SKILL.md`
- `${TOOL_CONFIG}/skills/tutorial-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/tutorial-define/SKILL.md`
- `${TOOL_CONFIG}/skills/tutorial-impl/SKILL.md`

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

| Aspect | example-workflow | tutorial-workflow |
|--------|------------------|-------------------|
| Primary output | Working code | Educational content |
| Code role | The deliverable | Illustrates concepts |
| Documentation | Explains the code | Is the main content |
| Structure | Files in directory | Sequential steps |
| Testing | Code must run | Steps must be followable |
| Integration | With main app | With docs/examples |
| Composed from | define → plan → impl → assess | define → impl |
| Unique skill | example-assess | (none - simpler flow) |

**Decision guide:**
- Need runnable demonstration? → `example-workflow`
- Need to teach step-by-step? → `tutorial-workflow`
- Need both? → Create example first, then tutorial that references it

---

# Relationship to Feature Development

This guideline composes skills from [feature-development.md](feature-development.md):

| Example Skill | Based On | Additional Concerns |
|---------------|----------|---------------------|
| `example-define` | `feature-define` | Learning objectives, prerequisites, complexity level |
| `example-plan` | `feature-plan` | Example numbering, demo script design |
| `example-impl` | `feature-impl` | Inline documentation explaining WHY, self-contained verification |
| `example-assess` | (new) | Educational value assessment |

**Prerequisites:**
Before using this guideline, ensure the feature-development skills are set up. The example skills extend those patterns with educational concerns.
