# Skills and Agents Framework - Project Conventions

This document defines conventions for maintaining and extending this framework.

---

## Project Intent

This repository provides **setup guides and templates** for creating reliable AI coding assistant skills and agents.

**What this is:**
- Setup guides that help users configure skills in their AI tool
- Templates that get customized per project
- Patterns for progress tracking, documentation maintenance, and customization

**What this is NOT:**
- Ready-to-use skills (users must adapt templates to their project)
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
├── templates/                   # Base templates for skills and agents
│   ├── skill-template.md        # Individual skill template
│   ├── skill-setup-guideline-template.md  # Skill suite setup template
│   ├── agent-template.md        # Individual agent template
│   └── agent-setup-guideline-template.md  # Agent suite setup template
│
├── references/                  # Reusable patterns (referenced by guidelines)
│   ├── skills-vs-agents.md      # When to use skills vs agents
│   ├── checklist-management.md  # Checklist syntax
│   ├── goals-and-objectives.md  # OKR patterns
│   ├── assessment-approaches.md # Quality criteria
│   └── spec-maintenance.md      # Progress documentation
│
├── skill-guidelines/            # Skill workflow setup guides
│   ├── INTENT.md                # Directory purpose
│   ├── feature-development.md   # Complete feature workflow
│   ├── bugfixing.md             # Hypothesis-based debugging
│   ├── refactoring.md           # Safe refactoring
│   ├── parallel-orchestration.md # Parallel task coordination
│   ├── creating-examples.md     # Educational content creation
│   └── create-skill.md          # Meta-skill for creating skills
│
└── agent-guidelines/            # Specialized agent setup guides
    ├── INTENT.md                # Directory purpose
    ├── code-review.md           # Code review agent setup
    └── test-architect.md        # Test architect agent setup
```

---

## Document Naming Conventions

### Setup Guidelines
- `core-conventions.md` - Core patterns and standards
- `[topic]-[purpose].md` - Descriptive, hyphenated
- Examples: `repo-detection.md`, `interview-protocol.md`

### Skill Guidelines
- `[workflow-name].md` - One workflow per file
- Examples: `feature-development.md`, `bugfixing.md`
- Each file contains ALL related skills (orchestrator + atomic skills)

### Agent Guidelines
- `INTENT.md` - Directory purpose
- `[domain].md` - One agent setup guide per file
- Examples: `code-review.md`, `test-architect.md`
- See `references/skills-vs-agents.md` for when to use agents vs skills

---

## Content Organization

### Skill Guideline Files

Each skill guideline file (e.g., `feature-development.md`) contains:

1. **Overview** - What this workflow does, when to use
2. **Phase 0: Repository Detection** - How to detect project structure
3. **Phase 1: Prerequisites** - What must exist before setup
4. **Skill Definitions** - All skills in this workflow
5. **SKILL.md Templates** - Complete, copy-paste ready templates
6. **Integration** - How skills compose together
7. **Testing** - How to validate the skills work

**All related skills in ONE file:**
- Orchestrator (e.g., `feature-workflow`)
- Atomic skills (e.g., `feature-define`, `feature-plan`, `feature-impl`)
- Review skills (e.g., `feature-define-review`, `feature-plan-review`)

### Template Structure

All SKILL.md templates follow this structure:

```markdown
---
name: [skill-name]
description: [one-line description]
disable-model-invocation: true
---

# [Skill Display Name]

[Purpose and when to use]

## Progress Tracking

[Progress tracker and checklist pattern]

## Phase 0: [First Phase]

[Phase checklist and instructions]

## Phase N: [Next Phase]

[Continue...]

## Best Practices

[Guidelines for this skill]
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

**Location:** Defined in `setup-guidelines/core-conventions.md`

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

**Location:** Defined in `setup-guidelines/core-conventions.md`

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

**Location:** Defined in `setup-guidelines/interview-protocol.md`

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
