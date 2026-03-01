---
name: setup-skill-env
description: >
  Install the skills-and-agents framework to make a repository AI-friendly.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup skills", "install framework", "make repo AI-friendly".
argument-hint: "[project path or 'current']"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, AskUserQuestion, TodoWrite
---

# Setup Skill Environment Workflow

**Goal:** Install the skills-and-agents framework components into a project.

**Intent:** Projects need primitives, workflows, and protocols installed for effective AI coding. This workflow handles the installation and basic configuration.

**Scope:** Framework installation: copy primitives, protocols, and workflows to the project's AI tool config directory. Create basic CLAUDE.md if missing.

---

## Key Results - KR

1. Framework components installed — primitives, protocols, and workflows copied to config directory
2. Project has AI guidance — CLAUDE.md exists with project conventions
3. Setup validated — installed components are accessible

## Requirements and Constraints - REQ

1. Progress tracked per `tracking_and_recovery.md`
2. Ask user for AI tool type (Claude Code, Cursor, etc.) to determine config directory
3. Do not overwrite existing files without user confirmation

---

## Preconditions

**Required:** Project path or current directory, framework source location

**Elicit if not provided:**
- AI tool type (Claude Code → `.claude/`, Cursor → `.cursor/`, etc.)
- Whether to overwrite existing files

**Optional:** Custom config directory path

## Postconditions

**Success:** All framework components installed, CLAUDE.md exists, setup validated

**Failure:** Framework source not accessible, cannot write to config directory, user aborts

---

## Steps

MUST read and follow steps in `dynamic_workflow_base.md`

---

## Deliverables

| Deliverable | Format | Location | Validation |
|-------------|--------|----------|------------|
| Primitives | Markdown files | `${CONFIG}/primitives/` | All 10 files present |
| Protocols | Markdown files | `${CONFIG}/protocols/` | All 13 files present |
| Workflows | Markdown files | `${CONFIG}/workflows/` | Base + dev_planning present |
| CLAUDE.md | Markdown | Project root | File exists, has project info |

---

## Possible Tasks

### Determine Config Directory (→ KR1)

Execute config detection when starting setup. Ask user which AI tool they use. Map to config directory:
- Claude Code → `.claude/`
- Cursor → `.cursor/`
- Other → ask for path

Create subdirectories: `primitives/`, `protocols/`, `workflows/`

**Fallback:** If directory creation fails, report error and ask user to create manually.

### Install Primitives (→ KR1)

Execute primitive installation when config directory exists. Copy all primitives from framework:

```
primitives/base.md
primitives/orienting.md
primitives/defining.md
primitives/planning.md
primitives/designing.md
primitives/implementing.md
primitives/evaluating.md
primitives/investigating.md
primitives/brainstorming.md
primitives/maintaining.md
```

**Fallback:** If file exists, ask user whether to overwrite.

### Install Protocols (→ KR1)

Execute protocol installation when config directory exists. Copy all protocols from framework:

```
protocols/tracking_and_recovery.md
protocols/thinking.md
protocols/discipline.md
protocols/pragmatics.md
protocols/interviewing.md
protocols/instruction_giving.md
protocols/criteria_setting.md
protocols/goal_setting.md
protocols/risk_management.md
protocols/transparency.md
protocols/software_quality.md
protocols/system_modularity.md
protocols/frame_of_mind.md
```

**Fallback:** If file exists, ask user whether to overwrite.

### Install Workflows (→ KR1)

Execute workflow installation when config directory exists. Copy workflow files:

```
workflows/dynamic_workflow_base.md
workflows/dev_planning_workflow.md
```

**Fallback:** If file exists, ask user whether to overwrite.

### Create CLAUDE.md (→ KR2)

Execute documentation creation when CLAUDE.md does not exist or user wants to update. Create basic CLAUDE.md with:

- Project name and description
- Key commands (build, test, lint)
- Directory structure overview
- Conventions (naming, patterns)
- Reference to installed skills

**Fallback:** If CLAUDE.md exists, ask user if they want to update or skip.

### Validate Setup (→ KR3)

Execute validation using `evaluating` primitive when installation complete. Check:

1. Config directory exists with subdirectories
2. All primitive files present (10 files)
3. All protocol files present (13 files)
4. Workflow files present (2 files)
5. CLAUDE.md exists

Report any missing components.

**Fallback:** If validation fails, offer to re-run failed installation tasks.

---

## Components to Install

### Primitives (10 files)
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

### Protocols (13 files)
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

### Workflows (2 files)
| File | Purpose |
|------|---------|
| dynamic_workflow_base.md | Base for dynamic workflows |
| dev_planning_workflow.md | Development planning |

---

## Additional Notes

**Extending:** After setup, users can add more workflows by copying from the framework's `workflows/` directory.

**Customization:** Users should edit CLAUDE.md to add project-specific conventions and commands.
