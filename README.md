# Skills and Agents Framework

A tool-agnostic framework for organizing AI coding assistant skills and agents into composable, discoverable workflows.

---

## What Do You Want To Do?

| Intent | Directory | Description |
|--------|-----------|-------------|
| **Build a new feature** | [building-features/](building-features/) | PRD → Plan → Implement workflow |
| **Fix a bug** | [fixing-bugs/](fixing-bugs/) | Hypothesis-based debugging |
| **Refactor code** | [refactoring/](refactoring/) | Safe code improvement |
| **Create examples/tutorials** | [creating-examples/](creating-examples/) | Educational content |
| **Use AI agents** | [agents/](agents/) | Specialized delegated tasks |

---

## Quick Decision Tree

```
What are you trying to do?
│
├─► Build something new
│   │
│   ├─► Have a feature idea → building-features/full-workflow-setup.md
│   ├─► Already have a PRD → building-features/plan-development-setup.md
│   ├─► Already have a plan → building-features/implement-feature-setup.md
│   └─► Large feature, want parallel work → building-features/parallelize-work-setup.md
│
├─► Fix a bug
│   │
│   ├─► Don't know root cause → fixing-bugs/full-workflow-setup.md
│   └─► Know what's wrong → fixing-bugs/implement-fix-setup.md
│
├─► Improve existing code
│   │
│   ├─► Need analysis first → refactoring/full-workflow-setup.md
│   └─► Know what to refactor → refactoring/implement-refactor-setup.md
│
├─► Create educational content
│   │
│   └─► Examples or tutorials → creating-examples/examples-and-tutorials-setup.md
│
└─► Delegate specialized task
    │
    ├─► Code review → agents/code-review-agent-setup.md
    └─► Test design → agents/test-architect-agent-setup.md
```

---

## How This Framework Works

### Skills vs Agents

| Aspect | Skills | Agents |
|--------|--------|--------|
| **Invoked by** | User directly (`/feature-define`) | AI delegates to agent |
| **Interaction** | Multi-phase with user gates | Autonomous, returns result |
| **Scope** | Complete workflows | Focused, specialized tasks |
| **Output** | Artifacts (PRDs, plans, code) | Analysis, recommendations |

### Three-Layer Architecture

```
Layer 1: Workflow Orchestrators
├── feature-workflow, bugfix-workflow, refactor-workflow
├── Detect state, compose skills, include review gates
│
Layer 2: Task Routers
├── parallel-orchestrate, parallel-task-workflow
├── Coordinate work across sessions/worktrees
│
Layer 3: Atomic Skills
├── feature-define, feature-plan, feature-impl
├── bugfix-impl, refactor-impl, create-example
└── Single-purpose, can be used standalone or composed
```

### Key Patterns

**State Detection:** Workflows detect what already exists (PRD? Plan? Code?) and skip completed phases.

**Review Gates:** User approval required at key decision points before proceeding.

**Composition:** Higher-level skills call lower-level skills.

---

## Directory Structure

```
skills-and-agents/
│
├── building-features/          # Feature development skills
│   ├── index.md
│   ├── full-workflow-setup.md
│   ├── define-requirements-setup.md
│   ├── plan-development-setup.md
│   ├── implement-feature-setup.md
│   └── parallelize-work-setup.md
│
├── fixing-bugs/                # Bug fixing skills
│   ├── index.md
│   ├── full-workflow-setup.md
│   └── implement-fix-setup.md
│
├── refactoring/                # Refactoring skills
│   ├── index.md
│   ├── full-workflow-setup.md
│   └── implement-refactor-setup.md
│
├── creating-examples/          # Educational content skills
│   ├── index.md
│   └── examples-and-tutorials-setup.md
│
├── agents/                     # Specialized agents
│   ├── index.md
│   ├── code-review-agent-setup.md
│   └── test-architect-agent-setup.md
│
├── shared/                     # Common resources
│   ├── repo-detection.md       # Phase 0 template
│   ├── prerequisites.md        # Phase 1 template
│   ├── interview-protocol.md   # Customization process
│   ├── code-review-checklist.md
│   ├── shell-functions.md      # Git worktree helpers
│   └── skill-template.md       # SKILL.md structure
│
└── reference/                  # Reference documentation
    ├── conventions.md          # Naming, quality standards
    ├── architecture.md         # Framework design
    └── worktree-guide.md       # Git worktree usage
```

---

## Getting Started

### 1. Choose Your Intent

Look at the [decision tree](#quick-decision-tree) above and pick the directory that matches what you're trying to do.

### 2. Read the Index

Each directory has an `index.md` that explains the available skills and when to use each.

### 3. Run Setup

Each setup guide follows the same structure:
- **Phase 0:** Repository Detection (analyze your project)
- **Phase 1:** Prerequisites (validate requirements)
- **Phase 2:** Define Skills (what will be created)
- **Phase 3:** SKILL.md Templates (copy-paste ready)
- **Phase 4:** Integration (how skills work together)
- **Phase 5:** Testing (verify it works)

### 4. Customize

Use the [Interview Protocol](shared/interview-protocol.md) to customize skills for your specific project conventions.

---

## Shared Resources

All setup guides reference these common documents:

| Document | Purpose |
|----------|---------|
| [repo-detection.md](shared/repo-detection.md) | Analyze project structure before setup |
| [prerequisites.md](shared/prerequisites.md) | Validate requirements are met |
| [interview-protocol.md](shared/interview-protocol.md) | Customize skills for your project |
| [code-review-checklist.md](shared/code-review-checklist.md) | Universal code quality standards |
| [shell-functions.md](shared/shell-functions.md) | Git worktree helper functions |
| [skill-template.md](shared/skill-template.md) | Base SKILL.md structure |

---

## Tool Adaptation

This framework is tool-agnostic. Adapt for your AI coding assistant:

| Tool | Skills Directory | Invocation |
|------|------------------|------------|
| Claude Code | `.claude/skills/` | `/skill-name` |
| Cursor | `.cursor/skills/` | Adapt to prompts |
| Codex | `.codex/skills/` | Adapt to format |
| Other | Your tool's config | Your tool's syntax |

---

## Core Principles

1. **Intent-based organization** - Find skills by what you want to accomplish
2. **Composable skills** - Use individually or let workflows compose them
3. **State detection** - Workflows skip completed phases
4. **Review gates** - User approval at key decision points
5. **Tool-agnostic** - Adapt to any AI coding assistant
6. **Customizable** - Interview protocol ensures project-specific adaptation

---

## Contributing

When adding new skills or agents:

1. Follow the naming convention: `{task}-{action}` for skills, `{role}-{domain}` for agents
2. Use the shared Phase 0/1 templates
3. Include complete SKILL.md or agent templates
4. Add to the appropriate intent directory
5. Update the index.md in that directory

---

## Legacy Files

The original suite setup files are in the `skills/` directory. These are being migrated to the intent-based structure. During the transition, both locations may contain content.
