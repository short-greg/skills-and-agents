# Workflows

This directory contains workflow definitions that compose primitives into reliable multi-step processes.

---

## What is a Workflow?

A workflow is a sequence of primitives orchestrated to accomplish a larger goal. All workflows in this directory:

- **Compose primitives** — Each step invokes exactly one primitive
- **Follow protocols** — Required: tracking, recovery, checklist_management
- **Are recoverable** — Can resume from interruption
- **Have clear goals** — Defined outcomes with verifiable success criteria

**Workflows are NOT:**
- Ready-to-use skills (these are templates to customize per project)
- Tool-specific (adapt to your AI coding assistant)
- Static procedures (adaptive workflows handle uncertainty)

---

## Available Workflows

### Development Workflows

**[feature_workflow.md](feature_workflow.md)**
- Complete feature development from idea to validated implementation
- **Primitives:** define → validate → design → validate → implement → validate
- **Type:** Adaptive (handles design uncertainty, validation failures)
- **Use when:** Building new features end-to-end

**[bugfix_workflow.md](bugfix_workflow.md)**
- Systematic bug investigation and fixing with hypothesis-driven approach
- **Primitives:** orient → investigate → define → implement → validate
- **Type:** Adaptive (hypothesis iteration, root cause discovery)
- **Use when:** Fixing bugs systematically with verified root causes

**[refactor_workflow.md](refactor_workflow.md)**
- Safe refactoring without changing behavior
- **Primitives:** orient → define → design → implement → validate
- **Type:** Deterministic (behavior preservation is predictable)
- **Use when:** Improving code structure without adding features

### Coordination Workflows

**[worktree_orchestrate_workflow.md](worktree_orchestrate_workflow.md)**
- Break large plans/PRDs into parallelizable tasks across git worktrees
- **Primitives:** orient → brainstorm → define → validate → implement (setup/merge)
- **Type:** Adaptive (task decomposition, dependency management)
- **Use when:** Large features can be split into independent parallel tasks

**[worktree_task_workflow.md](worktree_task_workflow.md)**
- Execute a single task in a git worktree based on a task specification
- **Primitives:** orient → validate → (delegates to feature/bugfix/refactor workflow)
- **Type:** Adaptive (routes to appropriate workflow based on task type)
- **Use when:** Working on an individual task within a worktree (created by orchestrate workflow)

---

## Workflow Structure

All workflows follow the `protocols/create_workflow.md` structure:

```markdown
---
name: workflow-name
description: When to use this workflow
argument-hint: What argument is expected
disable-model-invocation: true
user-invocable: true
allowed-tools: [tools needed]
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Workflow Name

**Goal:** [What this achieves]
**Intent:** [Why this exists]
**Scope:** [Complete process description]

## Workflow Type
[Deterministic/Adaptive, Imperative/Declarative/Hybrid]

## Steps
[Each step maps to one primitive]

## Preconditions
[What's needed to start]

## Postconditions
[Success/failure states]

## Recovery
[How to resume from interruption]
```

---

## Customizing Workflows

These workflows are **templates** — customize them for your project:

1. **Read the workflow** — Understand goal, scope, steps
2. **Identify customization points:**
   - Tool configuration paths (`${TOOL_CONFIG}`)
   - Spec directory location (`${SPEC_DIR}`)
   - Testing conventions (test frameworks, coverage targets)
   - Quality gates (when to validate/critique)
   - Iteration bounds (max validation failures)
3. **Adapt to project conventions:**
   - Read your project docs (CLAUDE.md, .cursorrules, etc.)
   - Match existing patterns (file naming, directory structure)
   - Configure quality standards (test coverage, documentation requirements)
4. **Test the workflow** — Run through a simple case to verify

---

## Required Protocols

Every workflow MUST follow these protocols:

- **[tracking.md](../protocols/tracking.md)** — Maintain trace file during execution
- **[recovery.md](../protocols/recovery.md)** — Resume from interruption
- **[checklist_management.md](../protocols/checklist_management.md)** — Dynamic progress tracking

---

## Optional Protocols

Use these protocols as needed for specific workflow challenges:

- **[manage_complexity_uncertainty_risk.md](../protocols/manage_complexity_uncertainty_risk.md)** — Handle complexity, uncertainty, risk (adaptive workflows)
- **[project_quality.md](../protocols/project_quality.md)** — Quality assessment across 6 dimensions
- **[modularity.md](../protocols/modularity.md)** — Modular design principles
- **[doc_maintenance.md](../protocols/doc_maintenance.md)** — When to update documentation

---

## Creating New Workflows

Follow the `protocols/create_workflow.md` protocol:

1. **Identify the goal** — What complete process needs orchestration?
2. **Determine workflow type:**
   - Deterministic or Adaptive?
   - Imperative, Declarative, or Hybrid?
3. **Map steps to primitives** — Each step = exactly one primitive
4. **Define data flow** — Inputs/outputs between steps
5. **Add quality gates** — Validate/critique each deliverable
6. **Plan recovery** — How to resume from interruption
7. **Handle failures** — What happens when steps or validation fail

---

## Workflow Patterns

Common patterns (from `protocols/create_workflow.md`):

**Validation Loop:**
```
primitive → validate → [on failure: investigate → loop back → refine]
```

**Quality Gate:**
```
Step N: produce deliverable
Step N+1: validate deliverable
Step N+2: use validated deliverable as input
```

**User Approval Gate:**
```
Step N: complete design
**Gate:** User confirms design before implementation
Step N+1: implement based on design
```

---

## Contributing

When adding a new workflow:

1. Use `protocols/create_workflow.md` as the template
2. Add entry to this README
3. Include all required protocols
4. Provide clear customization guidance
5. Test with a real example

---

## References

- **Protocol:** [create_workflow.md](../protocols/create_workflow.md)
- **Primitives:** [primitives/](../primitives/)
- **Protocols:** [protocols/](../protocols/)
