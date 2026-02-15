# Parallel Orchestration

---

# Overview

## Name
parallel-orchestration

## Background
Large features often have natural boundaries that enable parallel development. Without proper coordination, parallel work leads to merge conflicts, duplicated effort, or incorrect dependency sequencing. Git worktrees enable multiple working directories from the same repository, but orchestrating task breakdown, dependency tracking, and safe integration requires structured workflow management.

## Skill Intent
Enable parallel development by breaking large PRDs into independent tasks with dependency tracking, executing them in separate Git worktrees, and safely integrating work in correct dependency order.

## Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `parallel-orchestrate` | Plan, setup, status, review, merge workflows | Orchestrator |
| `parallel-task-workflow` | Dependency checking and routing to implementation skills | Router |
| `parallel-help` | Quick reference for worktree commands | Reference |

### When to Use Each Skill

**Use `parallel-orchestrate` when:**
- Breaking large PRDs into parallelizable tasks
- Managing task dependencies and execution order
- Coordinating integration of parallel work
- Need visibility into task progress and blockers

**Use `parallel-task-workflow` when:**
- Running inside a worktree session to implement a task
- Need dependency checking before starting work
- Want automatic routing to the correct implementation skill (feature-impl, bugfix-impl, refactor-impl)

**Use `parallel-help` when:**
- Need quick reference for worktree commands
- Troubleshooting worktree issues
- Learning the parallel workflow

## When to Use This Guideline
**Use this guideline when:**
- Setting up parallel development workflow for a project
- Creating coordinated parallel task execution skills
- Customizing worktree-based workflows for specific conventions

**Do NOT use when:**
- Tasks are small and can be completed quickly in sequence
- Changes affect the same files (merge conflicts likely)
- Single developer, single session preference
- Codebase area is tightly coupled

## Guideline OKRs
[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** Large features complete faster through safe parallel execution

**Key Results:**
1. Tasks execute in correct dependency order (no blocked task starts early)
2. All merges pass tests before proceeding
3. Worktree cleanup after merge is automatic

## Guideline Checklist
**Process to create skills from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for each skill in suite (see Skill Output Template)
- [ ] 3. Verify prerequisites (Git 2.5+, shell functions, directory structure)
- [ ] 4. Confirm skill designs with user
- [ ] 5. Write SKILL.md files:
  - `${TOOL_CONFIG}/skills/parallel-orchestrate/SKILL.md`
  - `${TOOL_CONFIG}/skills/parallel-task-workflow/SKILL.md`
  - `${TOOL_CONFIG}/skills/parallel-help/SKILL.md`
- [ ] 6. Test skill execution

---

# Process

**How each skill creates its checklist on execution:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to determine repo type and conventions
2. Read existing parallel plans to detect current state
3. Check dependency status for all tasks
4. For each phase, add corresponding checklist items
5. Add loop gates for merge failures
6. End with cleanup and documentation update

**Orchestrator (`parallel-orchestrate`) checklist generation:**
```yaml
base_items:
  - "Read project documentation and confirm conventions"
  - "Check for existing parallel plans"
  - "Determine current wave status"

phase_items:
  plan:
    - "Read PRD file"
    - "Identify natural task boundaries"
    - "Analyze dependencies between tasks"
    - "Create dependency graph"
    - "Generate task specs"
  setup:
    - "Read parallel plan"
    - "Identify ready tasks (dependencies satisfied)"
    - "Create worktrees for ready tasks"
    - "Copy and commit specs to worktrees"
  status:
    - "Read plan and check all task states"
    - "Show blocked/ready/completed tasks"
  review:
    - "Identify completed branches"
    - "Review against conventions"
    - "Check acceptance criteria"
  merge:
    - "Merge in dependency order"
    - "Test after each merge"
    - "Identify unblocked tasks"
    - "Clean up worktrees"

loop_gates:
  - trigger: "tests_fail_after_merge"
    action: "stop and report, do not continue merging"

always_last:
  - "Update project documentation if new patterns established"
```

**Router skill (`parallel-task-workflow`) checklist generation:**
```yaml
base_items:
  - "Read task spec from worktree"
  - "Check all dependencies are merged"

conditional_items:
  - condition: "dependencies_not_merged"
    items:
      - "STOP immediately with clear error"
      - "List unmerged dependencies"
      - "Exit without proceeding"

phase_items:
  routing:
    - "Detect task type (feature/bugfix/refactor)"
    - "Route to appropriate implementation skill"
  completion:
    - "Verify all changes committed"
    - "Verify tests passing"
    - "Report ready for merge"

always_last:
  - "Output next steps for orchestrator"
```

---

# Procedures

## Procedure: Task Planning (parallel-orchestrate plan)
**Purpose:** Break PRD into parallelizable tasks with dependency tracking

**Steps:**
1. Read PRD file and extract requirements
2. Identify natural task boundaries (features, components, layers)
3. Analyze dependencies between tasks
4. Create dependency graph with execution waves
5. Generate task spec files
6. Save plan to ${SPEC_DIR}/YYYY-MM-DD-parallel-plan.md

**Override Points:**
- Task granularity (aim for 2-6 tasks)
- Dependency detection rules
- Task spec template

## Procedure: Worktree Setup (parallel-orchestrate setup)
**Purpose:** Create worktrees for tasks with satisfied dependencies

**Steps:**
1. Read parallel plan
2. Check dependency status (which are merged)
3. Identify ready tasks
4. Create worktree for each ready task
5. Copy and force-add specs to worktrees
6. Output session instructions

**Override Points:**
- Branch naming convention
- Worktree directory location

## Procedure: Dependency Check (parallel-task-workflow)
**Purpose:** Verify dependencies before starting implementation

**Steps:**
1. Read task spec and extract dependencies
2. For each dependency, check if branch is merged
3. If ANY not merged: STOP and report
4. If ALL merged: detect task type and route

**Override Points:**
- How to check merge status
- What to do if blocked

## Procedure: Safe Merge (parallel-orchestrate merge)
**Purpose:** Integrate work in correct dependency order

**Steps:**
1. Read dependency graph from plan
2. Create merge sequence respecting dependencies
3. For each task: merge, test, continue or stop
4. Identify newly unblocked tasks
5. Clean up merged worktrees

**Override Points:**
- Test command
- Conflict resolution approach

## Procedure: Evaluate Risks
**Purpose:** Identify potential issues before execution

**Steps:**
1. Check for uncommitted changes
2. Verify worktree directory exists
3. Verify Git version supports worktrees (2.5+)
4. Verify shell functions installed

**Override Points:**
- Additional prerequisite checks

---

# Customization by Repo Type

## Prototype
- Simplified task specs (goal + acceptance criteria only)
- Fewer dependency checks
- Optional code review before merge

## Production
- Full task specs with implementation notes
- Strict dependency enforcement
- Required code review before merge
- Tests must pass after each merge

## Library
- Task specs include API compatibility notes
- Extra validation for public API changes
- Changelog updates tracked per task

---

# Framework References

- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist syntax
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria
- [references/spec-maintenance.md](../references/spec-maintenance.md) - Progress tracking
- [setup-guidelines/worktree-guide.md](../setup-guidelines/worktree-guide.md) - Git worktree setup

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
- `parallel-orchestrate` handles planning, setup, status, review, merge
- `parallel-task-workflow` runs in worktrees, routes to implementation skills
- `parallel-help` provides quick reference
- Implementation skills (feature-impl, bugfix-impl, refactor-impl) do actual work

**Output locations:**
- `${TOOL_CONFIG}/skills/parallel-orchestrate/SKILL.md`
- `${TOOL_CONFIG}/skills/parallel-task-workflow/SKILL.md`
- `${TOOL_CONFIG}/skills/parallel-help/SKILL.md`
