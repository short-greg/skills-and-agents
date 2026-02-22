# Skills and Agents Framework

**A framework of primitives, workflows, and protocols for building reliable AI coding assistant skills.**

---

## Getting Started

**First time using this framework?** Run the setup workflow to configure your environment.

Follow [workflows/setup_skill_env_workflow.md](workflows/setup_skill_env_workflow.md) to:
- Detect your project structure and conventions
- Configure your AI tool (`.claude/skills/`, `.cursor/`, etc.)
- Install selected workflows as skills
- Create project documentation (CLAUDE.md)
- Validate the setup works

> **For AI assistants:** If skills aren't set up yet, follow [workflows/setup_skill_env_workflow.md](workflows/setup_skill_env_workflow.md) first.

---

## What This Is

This repository provides a **complete framework** for creating AI coding assistant skills that actually work:

- **Primitives** — Atomic cognitive actions (orient, define, design, implement, validate, investigate, brainstorm, critique)
- **Workflows** — Multi-step processes that compose primitives (feature development, bugfixing, refactoring, code review)
- **Protocols** — Reusable patterns for tracking, recovery, quality, and more
- **Templates** — Starting points for creating your own skills

### Why This Exists

AI coding assistants often:
- Skip steps in complex workflows
- Violate project conventions
- Don't complete what they start

This framework solves these problems with:
- **Progress tracking** — Skills report what they're doing (less skipping)
- **Documentation maintenance** — Skills read/update conventions (less drift)
- **Structured workflows** — Clear steps with validation gates

### Tool Compatibility

Works with any AI coding assistant:
- Claude Code (`.claude/skills/`)
- Cursor
- Windsurf
- Others (adapt to your tool's format)

---

## Quick Start

### 1. Understand the Building Blocks

| Component | Purpose | Examples |
|-----------|---------|----------|
| **Primitives** | Atomic cognitive actions | orient, define, design, implement, validate |
| **Workflows** | Multi-step processes | feature_workflow, bugfix_workflow, refactor_workflow |
| **Protocols** | Reusable patterns | tracking, recovery, checklist_management |
| **Templates** | Starting points | skill-template, skill-setup-guideline-template |

### 2. Choose a Workflow

| I Want To... | Use This Workflow |
|--------------|-------------------|
| Build a complete feature | [feature_workflow.md](workflows/feature_workflow.md) |
| Fix a bug systematically | [bugfix_workflow.md](workflows/bugfix_workflow.md) |
| Refactor code safely | [refactor_workflow.md](workflows/refactor_workflow.md) |
| Work on tasks in parallel | [worktree_orchestrate_workflow.md](workflows/worktree_orchestrate_workflow.md) |
| Review code | [code_review_workflow.md](workflows/code_review_workflow.md) |
| Design test strategy | [test_strategy_workflow.md](workflows/test_strategy_workflow.md) |

### 3. Customize for Your Project

Templates use placeholders:
- `${TOOL_CONFIG}` — Your AI tool's config directory (`.claude/`, `.cursor/`, etc.)
- `${SPEC_DIR}` — Where you store PRDs/plans (`specs/`, `dev-docs/`, etc.)

---

## Repository Structure

```
skills-and-agents/
├── README.md                    # This file
├── CLAUDE.md                    # Project conventions
│
├── primitives/                  # Atomic cognitive actions
│   ├── orient.md                # Understand context and constraints
│   ├── define.md                # Establish requirements and scope
│   ├── design.md                # Plan technical approach
│   ├── implement.md             # Execute plans into artifacts
│   ├── validate.md              # Verify against criteria
│   ├── investigate.md           # Diagnose root causes
│   ├── brainstorm.md            # Generate options
│   └── critique.md              # Evaluate quality
│
├── workflows/                   # Multi-step processes
│   ├── feature_workflow.md      # Complete feature development
│   ├── bugfix_workflow.md       # Hypothesis-driven bug fixing
│   ├── refactor_workflow.md     # Safe behavior-preserving refactoring
│   ├── worktree_orchestrate_workflow.md  # Parallel task coordination
│   ├── worktree_task_workflow.md         # Single task in worktree
│   ├── code_review_workflow.md  # Systematic code review
│   └── test_strategy_workflow.md # Test design and planning
│
├── protocols/                   # Reusable patterns
│   ├── tracking.md              # Progress documentation
│   ├── recovery.md              # Resume from interruption
│   ├── checklist_management.md  # Dynamic checklist patterns
│   ├── skills_and_agents.md     # When to use skills vs agents
│   ├── goals_and_objectives.md  # OKR patterns
│   ├── project_quality.md       # Quality criteria
│   ├── modularity.md            # Modular design principles
│   └── ...                      # Other protocols
│
├── templates/                   # Base templates
│   ├── skill-template.md        # Individual skill template
│   └── skill-setup-guideline-template.md  # Skill suite setup
│
└── docs/                        # Additional documentation
    ├── SETUP.md                 # Detailed setup guide
    ├── SETUP_WORKTREE_ENV.md    # Git worktree setup
    ├── PRIMITIVES.md            # Primitives overview
    ├── IMPLEMENTATION-STATUS.md # Current status
    └── FUTURE_WORK.md           # Planned improvements
```

---

## Key Concepts

### Primitives

Primitives are atomic cognitive actions — the building blocks of workflows. Each primitive:
- Has a single, clear purpose
- Produces a well-defined output
- Can be composed into workflows

See [primitives/](primitives/) for all available primitives.

### Workflows

Workflows compose primitives into complete processes. Each workflow:
- Maps each step to exactly one primitive
- Follows required protocols (tracking, recovery, checklist_management)
- Can be customized per project

See [workflows/](workflows/) for all available workflows.

### Protocols

Protocols are reusable patterns that ensure consistency. Key protocols:

- **[tracking.md](protocols/tracking.md)** — Progress documentation during execution
- **[recovery.md](protocols/recovery.md)** — Resume from interruption
- **[checklist_management.md](protocols/checklist_management.md)** — Dynamic progress tracking

See [protocols/](protocols/) for all available protocols.

---

## Key Patterns

### Progress Tracking

**Problem:** AI skills often skip steps or don't complete workflows.

**Solution:** Skills report their progress as they work:
- Shows current phase
- Reports completed/pending tasks
- Creates audit trail

**Result:** Skills are less likely to skip steps because they're actively reporting what they're doing.

### Documentation Maintenance

**Problem:** Documentation drift causes convention violations and code duplication.

**Solution:** Implementation skills must:
- **Read** documentation before starting
- **Follow** conventions during implementation
- **Update** documentation when done

### Interview Protocol

**Problem:** Skills need project-specific customization but users don't know how.

**Solution:** Skills analyze your repository and ask targeted questions to customize themselves.

---

## Documentation

| Document | Purpose |
|----------|---------|
| [SETUP_WORKTREE_ENV.md](docs/SETUP_WORKTREE_ENV.md) | Git worktree configuration |
| [IMPLEMENTATION-STATUS.md](docs/IMPLEMENTATION-STATUS.md) | Current implementation status |
| [FUTURE_WORK.md](docs/FUTURE_WORK.md) | Planned improvements |

---

## Tool Compatibility

This framework works with any AI coding assistant that supports:
- Custom skills/prompts
- Multi-phase workflows
- File reading/writing

**Tested with:**
- Claude Code (`.claude/skills/`)
- Cursor (adapt to prompt system)
- Windsurf
- Other tools (adapt to your tool's format)

---

## FAQ

**Q: Are these ready-to-use skills?**
A: No. These are templates you must customize for your project.

**Q: Does this only work with Claude Code?**
A: No. It works with any AI coding assistant. Templates use placeholders you adapt to your tool.

**Q: Do I need to use everything?**
A: No. Pick what you need. Start with one workflow and expand from there.

**Q: Why progress tracking?**
A: AI skills often skip steps. Progress tracking makes the AI report what it's doing, which significantly reduces skipping.

**Q: Why documentation maintenance?**
A: Documentation drift causes convention violations and code duplication. Making skills read/update docs prevents this.

---

## Contributing

See [CLAUDE.md](CLAUDE.md) for:
- Project intent and conventions
- Content organization
- Quality standards
- Anti-patterns to avoid

---

## License

MIT
