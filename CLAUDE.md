# Skills and Agents Framework - Project Conventions

## About This Repo

This is a **tool/framework repository**. It defines primitives, workflows, and protocols that other projects use to build reliable AI coding assistant skills.

**To use this framework in another project:** See [README.md](README.md) and follow [workflows/setup_skill_env_workflow.md](workflows/setup_skill_env_workflow.md).

**To develop this framework:** Use the skills in `.claude/skills/`:
- `/create-workflow` — Create new workflows
- `/create-primitive` — Create new primitives
- `/setup-skill-env` — Test the setup workflow

---

## Project Intent

This repository provides **primitives, workflows, and protocols** for creating reliable AI coding assistant skills.

**What this is:**
- Primitives (atomic cognitive actions)
- Workflows (multi-step processes composing primitives)
- Protocols (reusable patterns for tracking, recovery, quality)
- Templates for creating new skills

**What this is NOT:**
- Ready-to-use skills (users must adapt to their project)
- Tool-specific (works with Claude Code, Cursor, Windsurf, etc.)
- A complete solution (requires user customization)

**Problem solved:**
- AI skills often skip steps or don't complete workflows
- Skills need project-specific customization but users don't know how
- Documentation drift causes convention violations and code duplication
- No standard patterns for reliable skill execution

**Solution provided:**
- Progress tracking pattern (skills report what they're doing as they work)
- Interview protocol (skills analyze repo and ask user for customization)
- Documentation maintenance (skills read/update conventions to prevent drift)
- Workflow templates (proven patterns for feature dev, bugfixing, refactoring)

---

## Repository Structure

```
skills-and-agents/
├── CLAUDE.md                    # This file - project conventions
├── README.md                    # User-facing overview and quick start
│
├── primitives/                  # Core cognitive actions (atomic building blocks)
│   ├── README.md                # Primitives overview
│   ├── orient.md                # Understand context and constraints
│   ├── define.md                # Establish requirements and scope
│   ├── design.md                # Plan technical approach
│   ├── implement.md             # Execute plans into artifacts
│   ├── validate.md              # Verify against criteria
│   ├── investigate.md           # Diagnose root causes
│   ├── brainstorm.md            # Generate options
│   └── critique.md              # Evaluate quality
│
├── workflows/                   # Multi-step processes composing primitives
│   ├── README.md                # Workflows overview
│   ├── feature_workflow.md      # Complete feature development
│   ├── bugfix_workflow.md       # Hypothesis-driven bug fixing
│   ├── refactor_workflow.md     # Safe behavior-preserving refactoring
│   ├── worktree_orchestrate_workflow.md  # Parallel task coordination
│   ├── worktree_task_workflow.md         # Single task execution in worktree
│   ├── code_review_workflow.md  # Systematic code review
│   └── test_strategy_workflow.md # Test design and planning
│
├── protocols/                   # Reusable patterns and conventions
│   ├── README.md                # Protocols overview
│   ├── tracking.md              # Progress documentation
│   ├── recovery.md              # Resume from interruption
│   ├── checklist_management.md  # Dynamic checklist patterns
│   ├── skills_and_agents.md     # When to use skills vs agents
│   ├── goals_and_objectives.md  # OKR patterns
│   ├── project_quality.md       # Quality criteria and assessment
│   ├── modularity.md            # Modular design principles
│   └── ...                      # Other protocols
│
└── templates/                   # Base templates for skills
    ├── skill-template.md        # Individual skill template
    └── skill-setup-guideline-template.md  # Skill suite setup template
```

---

## Document Naming Conventions

### Primitives
- `[action].md` - One primitive per file, named after the action
- Examples: `orient.md`, `define.md`, `validate.md`

### Workflows
- `[name]_workflow.md` - One workflow per file
- Examples: `feature_workflow.md`, `bugfix_workflow.md`, `worktree_orchestrate_workflow.md`
- Each workflow composes primitives into a complete process

### Protocols
- `[topic].md` - One protocol per file
- Examples: `tracking.md`, `recovery.md`, `checklist_management.md`
- Protocols are referenced by workflows and primitives

---

## Content Organization

### Workflow Files

Each workflow file (e.g., `feature_workflow.md`) contains:

1. **Frontmatter** - name, description, argument-hint, protocols
2. **Goal/Intent/Scope** - What it achieves and why
3. **Workflow Type** - Deterministic/Adaptive, Imperative/Declarative/Hybrid
4. **Steps** - Each step maps to exactly one primitive
5. **Preconditions/Postconditions** - What's needed, what's produced
6. **Recovery** - How to resume from interruption
7. **Quality Gates** - When to validate deliverables
8. **Customization Points** - How to adapt to different projects/repo types

### Workflow Structure

All workflows follow this structure:

```markdown
---
name: workflow-name
description: When to use this workflow
argument-hint: What argument is expected
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

## Customization Points
[Repository type variations, tool configuration, etc.]
```

---

## Key Patterns

### Progress Tracking Pattern

**ALL workflow skills must include progress tracking.**

Purpose:
- Makes Claude report what it's doing (less likely to skip steps)
- User can see progress
- Provides audit trail

Components:
1. **Progress display** - Shows current phase and what's done/pending
2. **Phase checklist** - Strict list of required actions
3. **Status reporting** - Claude reports completion as it works

**Location:** Defined in `protocols/tracking.md`

**User approval gates are OPTIONAL** - Claude should ask user if they want to review at each gate, not force it.

### Documentation Maintenance Pattern

**ALL implementation skills must read and update documentation.**

Purpose:
- Prevents convention drift
- Prevents code duplication
- Ensures patterns are followed

Required phases:
- **Phase 0: Read Documentation** - Read project documentation, package docs, confirm understanding
- **Final Phase: Update Documentation** - Update project docs if new patterns, update package docs if API changed

**Location:** Defined in `protocols/doc_maintenance.md`

### Interview Protocol

**ALL setup guides include customization via interview.**

Purpose:
- Skills get customized to project conventions
- Users don't have to manually adapt templates
- Ensures skills fit the project

Process:
1. Analyze repository structure
2. Detect conventions from existing code/docs
3. Ask user targeted questions
4. Generate customized templates

**Location:** Defined in `templates/skill-setup-guideline-template.md`

---

## Writing Style

### Be Specific About Structure
- Use exact file paths
- Provide complete templates
- Include concrete examples
- Show exactly what goes where

### Be General About Tools
- Don't assume Claude Code - say "your AI tool"
- Use `${TOOL_CONFIG}` not `.claude/`
- Use `${SPEC_DIR}` not `specs/`
- Adapt examples to multiple tools

### Be Clear About Purpose
- State what problem is solved
- State when to use vs. not use
- State what output is produced
- State what prerequisites are needed

---

## Quality Standards

### Documentation Must Be:
- **Accurate** - Matches current implementation
- **Complete** - No missing critical information
- **Clear** - Easy to understand
- **Concise** - No unnecessary verbosity
- **Consistent** - Follows established patterns

### Templates Must Be:
- **Copy-paste ready** - Users can use as-is
- **Customizable** - Clear placeholders like `${TOOL_CONFIG}`
- **Complete** - All phases included
- **Tested** - Validated to work

### Guidelines Must Include:
- Purpose and when to use
- Prerequisites
- Complete templates
- Integration with other skills
- Testing/validation steps

---

## Anti-Patterns to Avoid

### Don't:
- ❌ Create nested folder hierarchies (keep flat)
- ❌ Split related skills across multiple files
- ❌ Create "shared" or "common" directories (ended up too complex)
- ❌ Write tool-specific instructions
- ❌ Assume project structure
- ❌ Provide incomplete templates
- ❌ Skip progress tracking in workflows
- ❌ Skip documentation maintenance in implementation skills
- ❌ Create too many separate documentation files

### Do:
- ✅ Keep structure simple and flat
- ✅ Group related skills in one file
- ✅ Reference common patterns (don't duplicate)
- ✅ Use placeholders for project-specific values
- ✅ Detect project structure, don't assume
- ✅ Provide complete, working templates
- ✅ Include progress tracking
- ✅ Include documentation maintenance
- ✅ Consolidate related information

---

## Summary

**This repository is:**
- Setup guides and templates (not ready-to-use skills)
- Tool-agnostic (works with any AI coding assistant)
- Customization-focused (interview protocol ensures fit)
- Reliability-focused (progress tracking prevents skipping)
- Convention-focused (documentation maintenance prevents drift)

**Key innovations:**
- Progress tracking (makes skills self-reporting)
- Documentation maintenance (prevents convention drift)
- Interview protocol (ensures customization)
- Proven workflow templates (feature dev, bugfixing, refactoring)

**Target user:**
- Developers using AI coding assistants
- Teams wanting consistent skill patterns
- Projects needing convention enforcement
- Anyone building complex workflows
