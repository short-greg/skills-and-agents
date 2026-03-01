# Skills and Agents Framework

**A framework to set up a code repository to be AI-Friendly. It includes primitives, workflows, and protocols for building reliable AI coding assistant skills.**

---

## Getting Started

### For AI Assistants

Copy the following to your project's AI tool config directory (e.g., `.claude/skills/`):

**Workflow:**
- `workflows/dev_planning_workflow.md` — Development planning workflow
- `workflows/dynamic_workflow_base.md` — Base for dynamic workflows

**Primitives:**
- `primitives/base.md` — Base steps all primitives follow
- `primitives/orienting.md` — Understand current state
- `primitives/defining.md` — Establish requirements
- `primitives/planning.md` — Sequence actions
- `primitives/designing.md` — Plan technical approach
- `primitives/implementing.md` — Write code
- `primitives/evaluating.md` — Assess against criteria
- `primitives/investigating.md` — Research and diagnose
- `primitives/brainstorming.md` — Generate options
- `primitives/maintaining.md` — Keep system healthy

**Protocols:**
- `protocols/tracking_and_recovery.md` — Progress tracking and recovery
- `protocols/thinking.md` — Reasoning techniques
- `protocols/discipline.md` — Systematic enumeration
- `protocols/pragmatics.md` — Communication calibration
- `protocols/interviewing.md` — Eliciting information
- `protocols/instruction_giving.md` — Clear instructions
- `protocols/criteria_setting.md` — Defining success criteria
- `protocols/goal_setting.md` — Setting objectives
- `protocols/risk_management.md` — Risk assessment
- `protocols/transparency.md` — Documentation practices
- `protocols/software_quality.md` — Quality dimensions
- `protocols/system_modularity.md` — Modular design
- `protocols/frame_of_mind.md` — Expert reasoning

### Access Methods

1. **Clone temporarily** (recommended)
   ```bash
   git clone https://github.com/short-greg/skills-and-agents.git /tmp/skills-framework
   # Copy files to your project
   rm -rf /tmp/skills-framework  # cleanup
   ```

2. **Submodule** (for ongoing updates)
   ```bash
   git submodule add https://github.com/short-greg/skills-and-agents.git .skills-framework
   ```

3. **Manual download** — Download ZIP from GitHub

---

## What This Is

| Component | Purpose | Location |
|-----------|---------|----------|
| **Primitives** | Atomic cognitive actions | `primitives/` |
| **Workflows** | Multi-step processes composing primitives | `workflows/` |
| **Protocols** | Reusable reasoning and tracking patterns | `protocols/` |

### Why This Exists

AI coding assistants often skip steps, violate conventions, and don't complete workflows. This framework solves these problems with:

- **Dynamic execution** — Skills evaluate which actions to take based on context
- **Progress tracking** — Skills report what they're doing (less skipping)
- **Fallback handling** — Skills recover from failures gracefully
- **Protocol-based reasoning** — Consistent techniques across all skills

---

## Architecture

### Primitives

Atomic cognitive actions. Each primitive:
- Inherits steps from `base.md`
- Has Key Results (KR) and Requirements (REQ)
- Lists possible actions with conditions and protocol references
- Evaluates actions dynamically based on context

**Format:**
```
Execute [action] using `protocol.md` (Technique) when [condition]. [Details.]
```

### Workflows

Multi-step processes composing primitives. Dynamic workflows:
- Inherit steps from `dynamic_workflow_base.md`
- Evaluate tasks with fallbacks
- Produce deliverables with validation criteria
- Support recovery from interruption

### Protocols

Reusable reasoning patterns. Referenced inline:
```
Execute X using `thinking.md` (Analytical) when...
```

---

## Tool Compatibility

Works with any AI coding assistant:
- Claude Code (`.claude/skills/`)
- Cursor
- Windsurf
- Others (adapt to your tool's format)

---

## License

MIT
