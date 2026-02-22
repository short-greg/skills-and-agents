---
name: setup-skill-env
description: Set up skill environment for a new or existing project. Run this first when using the framework.
argument-hint: "[project path or 'current']"
---

# Setup Skill Environment

Set up the skill environment for a project by following the workflow defined in [workflows/setup_skill_env_workflow.md](../../../workflows/setup_skill_env_workflow.md).

## When to Use

- **First time using this framework** — Run this to configure your project
- **Adding new workflows** — Re-run to install additional workflows
- **Changing settings** — Re-run to modify customizations

## What It Does

1. **Detects project state** — Existing repo vs new repo
2. **Understands repository** — Structure, conventions, existing config
3. **Creates analysis report** — Uncertainties, risks, complexity, modularity
4. **Configures tool** — Sets up `.claude/skills/`, `.cursor/`, etc.
5. **Offers worktree setup** — For parallel work (optional)
6. **Installs workflows** — Copies selected workflows as skills
7. **Creates documentation** — CLAUDE.md or equivalent for AI coding
8. **Validates setup** — Confirms everything works

## Quick Start

```
/setup-skill-env current
```

Or specify a project path:

```
/setup-skill-env /path/to/project
```

## Full Workflow

See [workflows/setup_skill_env_workflow.md](../../../workflows/setup_skill_env_workflow.md) for the complete process.
