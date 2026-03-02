# Skills and Agents Framework

**A framework for building reliable AI coding assistant skills with primitives, workflows, and protocols.**

---

## Quick Start

### Option 1: Submodule (Recommended)

```bash
cd your-project
git submodule add https://github.com/short-greg/skills-and-agents.git .skills-framework
```

Then create skill pointers (see [Skill Structure](#skill-structure) below).

### Option 2: Use the Setup Skill

If you have Claude Code, run:
```
/setup-skill-env current
```

This will guide you through installation.

---

## How It Works

### Skill Structure

Skills are **lightweight pointers** in `.claude/skills/[name]/SKILL.md` that reference workflow files:

```
your-project/
├── .claude/
│   └── skills/
│       └── dev-planning/
│           └── SKILL.md          # Points to workflow
├── .skills-framework/            # Framework (submodule or copy)
│   ├── primitives/
│   ├── protocols/
│   └── workflows/
│       └── dev_planning_workflow.md
└── CLAUDE.md
```

**SKILL.md example:**
```markdown
---
name: dev-planning
description: Create a development plan before implementation
argument-hint: "[project or feature to plan]"
---

# Dev Planning

Follow [dev_planning_workflow.md](../../../.skills-framework/workflows/dev_planning_workflow.md).
```

### Framework Components

| Component | Purpose | Location |
|-----------|---------|----------|
| **Primitives** | Atomic cognitive actions | `.skills-framework/primitives/` |
| **Protocols** | Reusable reasoning patterns | `.skills-framework/protocols/` |
| **Workflows** | Multi-step processes | `.skills-framework/workflows/` |

---

## Available Components

### Primitives (10)

| File | Purpose |
|------|---------|
| base.md | Base steps all primitives follow |
| orienting.md | Understand current state |
| defining.md | Establish requirements |
| planning.md | Sequence actions |
| designing.md | Plan technical approach |
| implementing.md | Write code |
| evaluating.md | Assess against criteria |
| investigating.md | Research and diagnose |
| brainstorming.md | Generate options |
| maintaining.md | Keep system healthy |

### Protocols (13)

| File | Purpose |
|------|---------|
| tracking_and_recovery.md | Progress tracking and recovery |
| thinking.md | Reasoning techniques |
| discipline.md | Systematic enumeration |
| pragmatics.md | Communication calibration |
| interviewing.md | Eliciting information |
| instruction_giving.md | Clear instructions |
| criteria_setting.md | Defining success criteria |
| goal_setting.md | Setting objectives |
| risk_management.md | Risk assessment |
| transparency.md | Documentation practices |
| software_quality.md | Quality dimensions |
| system_modularity.md | Modular design |
| frame_of_mind.md | Expert reasoning |

### Workflows (2 core)

| File | Purpose |
|------|---------|
| dynamic_workflow_base.md | Base for dynamic workflows |
| dev_planning_workflow.md | Development planning |

---

## Creating Skills

1. **Create skill directory:** `.claude/skills/my-skill/`
2. **Create SKILL.md** with frontmatter and workflow reference
3. **Invoke with:** `/my-skill [args]`

---

## Updating Framework

If using submodule:
```bash
git submodule update --remote .skills-framework
```

---

## Tool Compatibility

- Claude Code (`.claude/skills/`)
- Cursor (adapt paths)
- Windsurf (adapt paths)
- Others (adapt to your tool's format)

---

## License

MIT
