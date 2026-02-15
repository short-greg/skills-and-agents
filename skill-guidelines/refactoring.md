# Refactoring

---

# Overview

## Name
refactoring

## Background
Refactoring requires preserving behavior while improving structure. Without safety guarantees, AI assistants make big-bang changes that break functionality, skip test verification, or change tests to make failing code pass. Safe refactoring requires incremental changes with continuous testing and measurable modularity improvements.

## Skill Intent
Enable safe code structure improvements through a suite of coordinated skills that guarantee behavior preservation via incremental changes and continuous test verification.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `refactor-workflow` | Orchestrates the full refactoring process | Orchestrator |
| `refactor-plan` | Analyze code for modularity issues, create refactoring plan | Atomic |
| `refactor-plan-review` | Review and validate refactoring plan | Review |
| `refactor-impl` | Execute incremental refactoring with continuous testing | Atomic |
| `refactor-assess` | Measure modularity improvement (before/after comparison) | Atomic |

### When to Use Each Skill

**Use `refactor-workflow` when:**
- Starting a refactoring effort from scratch
- Want complete orchestration with review gates
- Unsure what already exists (it detects and resumes)

**Use `refactor-plan` when:**
- Need to analyze code for modularity issues
- Want to create a structured refactoring plan
- Preparing for a refactoring effort

**Use `refactor-plan-review` when:**
- Have a refactoring plan that needs validation
- Want feedback before proceeding with changes

**Use `refactor-impl` when:**
- Have an approved refactoring plan
- Ready to make incremental structural changes
- Want safety guarantees (tests before and after each step)

**Use `refactor-assess` when:**
- Need to measure modularity metrics (complexity, coupling, cohesion)
- Want before/after comparison with concrete evidence
- Validating that refactoring achieved improvement goals

## When to Use This Guideline
**Use this guideline when:**
- Setting up refactoring workflow for a project
- Creating the refactoring skill suite
- Customizing refactoring workflow for specific conventions

**Do NOT use when:**
- Building new features (use feature-development guideline)
- Fixing bugs (use bugfixing guideline)
- Tests don't exist for target code (write tests first)

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** Code structure improves while behavior is provably preserved

**Key Results:**
1. 100% of original tests pass after refactoring (no tests modified)
2. Measurable modularity improvement (cohesion, coupling, complexity)
3. Zero behavior changes introduced (verified by unchanged test suite)

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/refactor-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/refactor-plan/SKILL.md`
  - `${TOOL_CONFIG}/skills/refactor-impl/SKILL.md`
  - `${TOOL_CONFIG}/skills/refactor-assess/SKILL.md`
  - (Optional) `${TOOL_CONFIG}/skills/refactor-plan-review/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read CLAUDE.md to determine repo type and conventions
2. Read existing artifacts (refactoring plans) to detect current state
3. Based on repo type, include/exclude quality gates
4. For each phase, add corresponding checklist items
5. Add loop gates for test failures (rollback and fix)
6. End with documentation update item

**Orchestrator (`refactor-workflow`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md and confirm conventions"
  - "Check for existing refactoring plans"
  - "Determine starting point based on existing artifacts"

phase_items:
  assess_before:
    - "Run refactor-assess skill (capture BEFORE metrics)"
  plan:
    - "Run refactor-plan skill"
  review:
    - "(If production) Run refactor-plan-review skill"
  implement:
    - "Run refactor-impl skill"
  assess_after:
    - "Run refactor-assess skill (capture AFTER metrics)"
  document:
    - "Update architecture docs if needed"

loop_gates:
  - trigger: "review_rejected"
    action: "go to refactor-plan for revision"
  - trigger: "tests_fail"
    action: "rollback in refactor-impl, fix issue"

always_last:
  - "Update CLAUDE.md if new patterns established"
```

**Atomic skill (`refactor-plan`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md for conventions"
  - "Read BEFORE metrics from refactor-assess"
  - "Analyze target code for modularity issues"

phase_items:
  analyze:
    - "Identify cohesion problems (multiple responsibilities)"
    - "Identify coupling problems (too many dependencies)"
    - "Identify complexity issues (high cyclomatic complexity)"
  define:
    - "Define improvement goals with measurable targets"
    - "Break down into incremental steps (5-10 min each)"
    - "Identify rollback strategy"

always_last:
  - "Write plan to ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md"
```

**Atomic skill (`refactor-impl`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md for conventions"
  - "Read approved refactoring plan"
  - "Establish test baseline (all tests must pass)"

incremental_loop:
  for_each_step:
    - "Make small change"
    - "Run tests immediately"
    - "If pass: document and continue"
    - "If fail: rollback, fix, retry"

safety_rules:
  - "Never modify tests (they define correct behavior)"
  - "Never combine multiple refactorings in one step"
  - "Never proceed with failing tests"

always_last:
  - "Verify all tests still pass"
```

**Atomic skill (`refactor-assess`) checklist generation:**
```yaml
base_items:
  - "Read CLAUDE.md for conventions"
  - "Identify target code to assess"

assessment_items:
  - "Run complexity analysis (radon cc)"
  - "Count dependencies (imports)"
  - "Check for circular dependencies"
  - "Assess cohesion (responsibilities per module)"
  - "Assess coupling (dependency count)"
  - "Assess coherence (logical boundaries)"

conditional_items:
  - condition: "is_after_assessment"
    items:
      - "Compare to BEFORE metrics"
      - "Calculate improvement percentages"
      - "Verify improvement meets targets"

always_last:
  - "Document metrics in ${SPEC_DIR}/refactor-assessment.md"
```

---

# Procedures

## Procedure: Modularity Assessment (refactor-assess)
**Purpose:** Measure modularity metrics with concrete, tool-based evidence

**Steps:**
1. Run complexity analysis tools (radon cc, eslint complexity)
2. Count dependencies (import count, dependency graph)
3. Check for circular dependencies
4. Assess cohesion (responsibilities per module, public method count)
5. Assess coupling (import count, dependency depth)
6. Assess coherence (logical module boundaries)
7. Document all metrics with tool output

**Override Points:**
- Assessment tools (radon, pylint, eslint, etc.)
- Metric thresholds
- Output format

## Procedure: Refactoring Planning (refactor-plan)
**Purpose:** Analyze code and create structured refactoring plan

**Steps:**
1. Read BEFORE metrics from refactor-assess
2. Analyze target code for modularity issues
3. Identify cohesion problems (multiple responsibilities in one module)
4. Identify coupling problems (too many dependencies, circular deps)
5. Identify complexity issues (high cyclomatic complexity)
6. Define improvement goals with measurable targets
7. Break down into incremental steps (5-10 min each)
8. Save to ${SPEC_DIR}/YYYY-MM-DD-refactor-plan.md

**Override Points:**
- Plan template format
- Step granularity
- Improvement targets

## Procedure: Plan Review (refactor-plan-review)
**Purpose:** Validate refactoring plan before execution

**Steps:**
1. Review plan completeness (all issues addressed)
2. Verify incremental steps are small enough
3. Verify improvement targets are measurable
4. Confirm rollback strategy exists
5. Get user approval

**Override Points:**
- Review criteria
- Approval process

## Procedure: Incremental Refactoring (refactor-impl)
**Purpose:** Execute small, safe structural changes with continuous testing

**Steps:**
1. Establish test baseline (all tests must pass before starting)
2. For each step in plan:
   - Make small change (5-10 min max)
   - Run tests immediately
   - If pass: document and continue
   - If fail: rollback, fix, retry
3. Never modify tests (they define correct behavior)
4. Never combine multiple refactorings in one step
5. Never proceed with failing tests

**Override Points:**
- Step size
- Test command

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes
2. Verify tests exist for target code
3. Verify all tests currently pass
4. Assess rollback strategy

**Override Points:**
- Risk thresholds

---

# Customization by Repo Type

## Prototype
- Modularity assessment optional
- Informal refactoring plan acceptable
- Tests strongly recommended but not required
- Documentation updates optional

## Production
- Full modularity assessment required (before/after)
- Formal refactoring plan with review
- Tests required with coverage verification
- Documentation updates required

## Library
- Modularity assessment required
- Must verify no API changes
- Tests required
- Changelog NOT updated (no behavior change)

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
- `refactor-workflow` orchestrator manages full workflow
- `refactor-impl` handles incremental changes with continuous testing
- Tests run after EACH incremental step (not just at the end)
- Any test failure triggers rollback, not continuation

**Output locations:**
- `${TOOL_CONFIG}/skills/refactor-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/refactor-impl/SKILL.md`
