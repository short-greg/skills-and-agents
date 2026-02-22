# Bugfixing

---

# Overview

## Name
bugfixing

## Background
Bug fixing is often approached haphazardly - developers jump to conclusions, fix symptoms rather than causes, and introduce regressions. This guideline provides a systematic, hypothesis-driven approach to debugging that ensures root causes are identified and minimal fixes are applied.

## Skill Intent
Enable systematic bug investigation through a suite of coordinated skills.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `bugfix-workflow` | End-to-end orchestration from bug report to verified fix | Orchestrator |
| `bugfix-reproduce` | Reproduce bug and create failing test | Atomic |
| `bugfix-hypothesize` | Generate hypotheses about root causes | Atomic |
| `bugfix-investigate` | Test hypotheses with instrumentation | Atomic |
| `bugfix-impl` | Implement minimal fix with regression test | Atomic |

### When to Use Each Skill

**Use `bugfix-workflow` when:**
- Starting bug investigation from scratch
- Want complete orchestration with review gates
- Unsure what debugging has been done (it detects and resumes)

**Use `bugfix-reproduce` when:**
- Have a bug report that needs reproduction
- Need to create failing test before investigation

**Use `bugfix-hypothesize` when:**
- Bug is reproduced
- Need to generate potential root causes

**Use `bugfix-investigate` when:**
- Have hypotheses to test
- Need to collect evidence with instrumentation

**Use `bugfix-impl` when:**
- Root cause is confirmed
- Ready to implement minimal fix

## When to Use This Guideline
**Use this guideline when:**
- Setting up bugfixing workflow for a project
- Creating the full suite of debugging skills
- Customizing hypothesis-driven debugging for specific conventions

**Do NOT use when:**
- Building new features (use feature-development guideline)
- Refactoring without fixing bugs (use refactoring guideline)

## Guideline OKRs
[See [protocols/goals_and_objectives.md](../protocols/goals_and_objectives.md)]

**Objective:** Bugs are fixed systematically with verified root causes and no regressions

**Key Results:**
1. 100% of fixes have documented root cause analysis
2. 100% of fixes include regression test
3. 0 regressions introduced by bug fixes

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/bugfix-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/bugfix-reproduce/SKILL.md`
  - `${TOOL_CONFIG}/skills/bugfix-hypothesize/SKILL.md`
  - `${TOOL_CONFIG}/skills/bugfix-investigate/SKILL.md`
  - `${TOOL_CONFIG}/skills/bugfix-impl/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [protocols/skills_and_agents.md](../protocols/skills_and_agents.md) for tool-specific locations) to determine repo type and testing conventions
2. Check for existing debugging artifacts (hypotheses docs, root cause docs)
3. Based on repo type, include/exclude quality gates
4. For each phase, add corresponding checklist items
5. Add loop gates for incomplete evidence or failing tests
6. End with documentation update item

**Orchestrator (`bugfix-workflow`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing hypotheses/root cause docs"
  - "Determine starting point based on existing artifacts"

phase_items:
  reproduce:
    - "Run bugfix-reproduce skill"
  hypothesize:
    - "Run bugfix-hypothesize skill"
    - "(If production) Review hypotheses with user"
  investigate:
    - "Run bugfix-investigate skill"
  implement:
    - "Run bugfix-impl skill"

loop_gates:
  - trigger: "all_hypotheses_refuted"
    action: "go to bugfix-hypothesize for more hypotheses"
  - trigger: "tests_fail_after_fix"
    action: "go to bugfix-investigate to re-examine"

always_last:
  - "Update project documentation if new debugging patterns established"
```

**Atomic skill (`bugfix-hypothesize`) checklist generation:**
```yaml
base_items:
  - "Read project documentation for conventions"
  - "Analyze bug symptoms and error messages"
  - "Identify potential cause categories"

conditional_items:
  - condition: "repo_type == 'production'"
    items:
      - "Generate 3+ hypotheses"
  - condition: "repo_type == 'prototype'"
    items:
      - "Generate 2+ hypotheses"

always_last:
  - "Write hypotheses to ${SPEC_DIR}/hypotheses.md"
```

---

# Procedures

## Procedure: Bug Reproduction (bugfix-reproduce)
**Purpose:** Confirm bug exists and create failing test

**Steps:**
1. Gather bug description from user or issue
2. Reproduce the bug locally
3. Write failing test that captures the bug
4. Save reproduction details to ${SPEC_DIR}/bug-report.md

**Override Points:**
- Test framework
- Evidence collection requirements

## Procedure: Hypothesis Generation (bugfix-hypothesize)
**Purpose:** Generate distinct, testable hypotheses about root causes

**Steps:**
1. Analyze bug symptoms and error messages
2. Identify categories of potential causes (logic, concurrency, validation, etc.)
3. Generate hypotheses with theory and test plan
4. Ensure hypotheses are distinct (not variations of same idea)
5. Save to ${SPEC_DIR}/hypotheses.md

**Override Points:**
- Minimum number of hypotheses (prototype: 2, production: 3+)
- Required categories to cover

## Procedure: Hypothesis Testing (bugfix-investigate)
**Purpose:** Systematically test each hypothesis with instrumentation

**Steps:**
1. For each hypothesis: add debug instrumentation
2. Run code and collect evidence
3. Mark hypothesis as CONFIRMED, REFUTED, or INCONCLUSIVE
4. Document findings in ${SPEC_DIR}/investigation.md

**Override Points:**
- Types of instrumentation allowed
- Evidence collection depth

## Procedure: Minimal Fix Implementation (bugfix-impl)
**Purpose:** Fix the root cause with smallest possible change

**Steps:**
1. Implement fix targeting only the confirmed root cause
2. Verify original reproduction test passes
3. Add regression test
4. Run full test suite
5. Update documentation

**Override Points:**
- Definition of "minimal"
- Regression test requirements

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes
2. Verify test suite exists and passes
3. Assess impact on existing code

**Override Points:**
- Additional checks for security bugs

---

# Customization by Repo Type

## Prototype
- 2 hypotheses minimum
- Evidence documentation optional
- Regression test recommended but not required
- Review gates optional

## Production
- 3+ hypotheses required
- Full evidence documentation required
- Regression test mandatory
- Security scan for security-related bugs
- Review gates recommended

## Library
- 3+ hypotheses required
- API compatibility verification
- Backward compatibility check
- Documentation update if API behavior changes

---

# Framework References

- [protocols/goals_and_objectives.md](../protocols/goals_and_objectives.md) - OKR patterns
- [protocols/checklist_management.md](../protocols/checklist_management.md) - Checklist syntax
- [protocols/project_quality.md](../protocols/project_quality.md) - Quality criteria
- [protocols/tracking.md](../protocols/tracking.md) - Progress tracking

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
- `bugfix-workflow` orchestrator calls atomic skills
- Each atomic skill is standalone but designed to work in sequence
- Investigation may loop back to hypothesis generation

**Output locations:**
- `${TOOL_CONFIG}/skills/bugfix-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/bugfix-reproduce/SKILL.md`
- `${TOOL_CONFIG}/skills/bugfix-hypothesize/SKILL.md`
- `${TOOL_CONFIG}/skills/bugfix-investigate/SKILL.md`
- `${TOOL_CONFIG}/skills/bugfix-impl/SKILL.md`
