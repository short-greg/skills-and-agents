# Skills and Agents Framework

**Setup guides and templates for creating reliable AI coding assistant skills and agents.**

---

## What This Is

This repository provides **setup guides** (not ready-to-use skills) that help you:

1. **Create skills** for your AI coding assistant (Claude Code, Cursor, Windsurf, etc.)
2. **Customize templates** to match your project's conventions
3. **Build reliable workflows** that actually complete (using progress tracking)
4. **Prevent convention drift** (using documentation maintenance)

### What You Get

- **Workflow templates** - Feature development, bugfixing, refactoring, parallel orchestration
- **Progress tracking pattern** - Makes skills report what they're doing (less likely to skip steps)
- **Documentation maintenance** - Skills read/update conventions to prevent drift
- **Interview protocol** - Skills get customized to your project automatically
- **Agent templates** - Code review, test architecture, and more

### What This Is NOT

- ❌ Ready-to-use skills (you must customize templates for your project)
- ❌ Tool-specific (works with any AI coding assistant)
- ❌ A complete solution (requires your customization and setup)

---

## Quick Start

### 1. Choose What You Need

| I Want To... | Use This Guide |
|--------------|----------------|
| Build a complete feature (PRD → Plan → Code) | [skill-guidelines/feature-development.md](skill-guidelines/feature-development.md) |
| Fix a bug systematically | [skill-guidelines/bugfixing.md](skill-guidelines/bugfixing.md) |
| Refactor code safely | [skill-guidelines/refactoring.md](skill-guidelines/refactoring.md) |
| Work on tasks in parallel | [skill-guidelines/parallel-orchestration.md](skill-guidelines/parallel-orchestration.md) |
| Create examples/tutorials | [skill-guidelines/creating-examples.md](skill-guidelines/creating-examples.md) |
| Set up code review agent | [agent-guidelines/code-review-agent.md](agent-guidelines/code-review-agent.md) |
| Set up test architect agent | [agent-guidelines/test-architect-agent.md](agent-guidelines/test-architect-agent.md) |

### 2. Follow the Setup Guide

Each guide includes:
- **Phase 0:** Repository Detection (analyze your project)
- **Phase 1:** Prerequisites (verify requirements)
- **Skill Templates:** Complete SKILL.md files ready to customize
- **Integration:** How skills work together
- **Testing:** Validate the skills work

### 3. Customize the Templates

Templates use placeholders:
- `${TOOL_CONFIG}` - Your AI tool's config directory (`.claude/`, `.cursor/`, etc.)
- `${SPEC_DIR}` - Where you store PRDs/plans (`specs/`, `dev-docs/`, etc.)

The interview protocol helps your AI assistant detect these automatically.

---

## Key Innovations

### 1. Progress Tracking

**Problem:** AI skills often skip steps or don't complete workflows.

**Solution:** Skills report their progress as they work:
- Shows current phase
- Reports completed/pending tasks
- Creates audit trail

**Result:** Skills are less likely to skip steps because they're actively reporting what they're doing.

### 2. Documentation Maintenance

**Problem:** Documentation drift causes convention violations, code duplication, tool misuse.

**Solution:** Implementation skills must:
- **Read** documentation before starting (Phase 0)
- **Follow** conventions during implementation
- **Update** documentation when done (Final Phase)

**Result:** Conventions stay current, preventing drift over time.

### 3. Interview Protocol

**Problem:** Skills need project-specific customization but users don't know how.

**Solution:** Skills analyze your repository and ask targeted questions to customize themselves.

**Result:** Skills fit your project's conventions automatically.

---

## Repository Structure

```
skills-and-agents/
├── README.md                    # This file - quick start
├── CLAUDE.md                    # Project conventions
│
├── setup-guidelines/            # How to set up and customize
│   ├── core-conventions.md      # Progress tracking, doc maintenance, naming
│   ├── repo-detection.md        # How to analyze project structure
│   ├── interview-protocol.md    # How to customize per project
│   └── worktree-guide.md        # Git worktree usage for parallel work
│
├── skill-guidelines/            # Workflow setup guides
│   ├── feature-development.md   # feature-workflow, feature-define, feature-plan, feature-impl
│   ├── bugfixing.md            # bugfix-workflow, bugfix-impl
│   ├── refactoring.md          # refactor-workflow, refactor-impl
│   ├── parallel-orchestration.md # parallel-orchestrate, parallel-task-workflow
│   └── creating-examples.md    # create-example, create-tutorial
│
└── agent-guidelines/            # Agent setup guides
    ├── overview.md              # Skills vs agents, when to use each
    ├── code-review-agent.md     # Automated code review
    └── test-architect-agent.md  # Test strategy design
```

---

## How Skills Work

### Workflow Skills (Orchestrators)

Example: `feature-workflow`

```
feature-workflow
├── Detects current state (PRD exists? Plan exists? Code exists?)
├── Skips completed phases
├── Calls: feature-define (if no PRD)
├── Calls: feature-plan (if no plan)
├── Calls: feature-impl (to implement)
└── Reviews modularity
```

### Atomic Skills

Example: `feature-define`

```
feature-define
├── Phase 0: Read documentation, confirm conventions
├── Phase 1: Capture feature idea
├── Phase 2: Write background
├── Phase 3: Create user stories
├── Phase 4: Define OKRs
├── Phase 5: UI design (if applicable)
├── Phase 6: Identify risks
├── Phase 7: Define scope
├── Phase 8: Write PRD document
└── Final Phase: Update documentation
```

### Skills with Progress Tracking

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-DEFINE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Capture idea
✅ Phase 2: Write background
🔄 Phase 3: User stories [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: OKRs
⏸️ Phase 5: UI design
⏸️ Phase 6: Risks
⏸️ Phase 7: Scope
⏸️ Phase 8: Write PRD

CURRENT TASK:
Phase 3: Creating user stories
Status: Writing stories for each persona
Started: 10:30

CHECKLIST:
✅ Identify personas
✅ Write stories for persona 1
🔲 Write stories for persona 2
🔲 Prioritize stories
🔲 Present to user
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Common Workflows

### Build a Feature (End-to-End)

```bash
# 1. Start workflow
/feature-workflow

# It detects current state and skips completed phases
# It composes: feature-define → feature-plan → feature-impl
# User approval gates between major phases
# Progress tracking shows what's done/pending
```

### Fix a Bug (Hypothesis-Based)

```bash
# 1. Start workflow
/bugfix-workflow

# It uses hypothesis-based debugging:
# - Generate 3+ potential root causes
# - Test each hypothesis systematically
# - Implement minimal fix
# - Verify resolution
```

### Refactor Code (Safely)

```bash
# 1. Start workflow
/refactor-workflow

# It ensures safety:
# - Preserve existing tests
# - Refactor in small steps
# - Run tests after each change
# - Measure modularity improvement
```

### Work in Parallel (Large Features)

```bash
# 1. Create plan with task decomposition
/feature-plan specs/feature-prd.md

# 2. Set up parallel orchestration
/parallel-orchestrate plan specs/feature-plan.md
/parallel-orchestrate setup

# 3. Work on tasks in separate worktrees
# (Each worktree runs /parallel-task-workflow)

# 4. Review and merge
/parallel-orchestrate review
/parallel-orchestrate merge
```

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

## Documentation Maintenance Example

**Problem:** New developer adds code that duplicates existing functionality because they didn't know about the existing pattern.

**With this framework:**

```markdown
## Phase 0: Read Documentation

**READ CLAUDE.md:**
- Found: "Use BatchProcessor for bulk operations"
- Found: "Batch sizes must not exceed 100"
- Found: "All batch operations must be atomic"

✅ I understand the conventions
✅ I will use existing BatchProcessor
✅ I will not create duplicate batch logic

## [Implementation phases...]

## Final Phase: Update Documentation

**CLAUDE.md UPDATED:**
- Added: Example usage of BatchProcessor for moderation
- No new patterns needed - used existing ones

**moderation/README.md UPDATED:**
- Added: approve_batch function documentation
- Added: Dependency on utils.batch
```

**Result:** No duplication, conventions followed, documentation stays current.

---

## Getting Help

### Understanding Core Concepts

- Read [setup-guidelines/core-conventions.md](setup-guidelines/core-conventions.md) for:
  - Progress tracking pattern
  - Documentation maintenance pattern
  - Naming conventions

### Setting Up Your First Skill

1. Start with [skill-guidelines/feature-development.md](skill-guidelines/feature-development.md)
2. Follow Phase 0-1 to detect your project structure
3. Copy the SKILL.md templates
4. Customize with your `${TOOL_CONFIG}` and `${SPEC_DIR}`
5. Test with a simple feature

### Contributing

See [CLAUDE.md](CLAUDE.md) for:
- Project intent and conventions
- Content organization
- Quality standards
- Anti-patterns to avoid

---

## FAQ

**Q: Are these ready-to-use skills?**
A: No. These are setup guides and templates you must customize for your project.

**Q: Does this only work with Claude Code?**
A: No. It works with any AI coding assistant (Claude Code, Cursor, Windsurf, etc.). Templates use placeholders you adapt to your tool.

**Q: Do I need to use all the skills?**
A: No. Pick what you need. Start with one workflow (e.g., feature-development) and expand from there.

**Q: What's the learning curve?**
A: Start simple:
1. Read core-conventions.md (15 min)
2. Pick one workflow guide (30 min)
3. Customize and test (1 hour)
4. Total: ~2 hours to get first skill working

**Q: Why progress tracking?**
A: AI skills often skip steps. Progress tracking makes the AI report what it's doing as it works, which significantly reduces skipping.

**Q: Why documentation maintenance?**
A: Documentation drift is a primary cause of convention violations, code duplication, and architectural inconsistency. Making skills read/update docs prevents this.

**Q: Can I modify the templates?**
A: Yes! These are starting points. Adapt to your team's needs.

---

## License

[Add your license here]

---

## Credits

Developed to solve real problems with AI coding assistants:
- Skills that don't complete
- Documentation that drifts
- Conventions that aren't followed
- Customization that's too manual
