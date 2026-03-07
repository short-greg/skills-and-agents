# Skills and Agents Framework

**A framework for building reliable AI coding assistant skills with primitives, workflows, and protocols.**

---

## Installation

### Prerequisites

- Claude Code installed ([code.claude.com/docs](https://code.claude.com/docs/en/skills))
- GitHub CLI (`gh`) installed ([cli.github.com](https://cli.github.com/))

### Installation Steps

1. Copy all primitives, workflows and protocols into a temporary directory. Ensure it can be deleted. Ask whether to use gh, clone, or another method, recommend gh.
2. Copy protocols into a protocols directory underneat Claude `.claude/protocols/`
3. Copy each primitive and workflow into a new directory for it
   .claude/skills/<skill name>/SKILL.md
4. Verify installation:
   ```bash
   ls .claude/skills/
   ```
5. Skills are now available via slash commands (e.g., `/orienting`, `/defining`, `/dev-planning`)

---

## How It Works

### Skill Loading

**Claude Code automatically loads skills from `.claude/skills/`:**

Put all skills into a folder using the name of the skill. Then put the skill Markdown file in a SKILL.md file.

```
your-project/
├── .claude/
│   └── skills/                   # Skills loaded automatically
│       ├── [some skill]/
│       │   └── SKILL.md          # /[some skill] skill
│       ├── ...
│       └── protocols/            # Referenced by skills
└── CLAUDE.md
```

**Each skill directory contains SKILL.md with frontmatter:**

```markdown
---
name: orienting
description: Understand the current state of a system, project, or task
argument-hint: "[subject to orient on]"
user-invocable: true              # Makes it a /orienting skill
---

# Orienting

**Goal:** Produce accurate picture of current state...
```

**Invoke with:**
```
/orienting my-feature
/dev-planning new-api
```

### Framework Components

**Primitives** — Atomic cognitive actions (orienting, defining, planning, designing, implementing, evaluating, investigating, brainstorming, maintaining, positioning)

**Workflows** — Multi-step processes that compose primitives (dev-planning)

**Protocols** — Supporting patterns and techniques referenced by skills (thinking, transparency, risk management, etc.)

---

## Creating Custom Skills

### Create a New Skill

```bash
# Create skill directory and file
mkdir -p .claude/skills/my-skill
touch .claude/skills/my-skill/SKILL.md
```

Edit `.claude/skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: What this skill does
argument-hint: "[what user provides]"
user-invocable: true
---

# My Skill

**Goal:** What this skill achieves

**Intent:** Why this skill exists

Follow these steps...
```

**Invoke:** `/my-skill [args]`

### Skill Frontmatter Fields

- `name:` Skill name for invocation (use `/name`)
- `description:` What the skill does
- `argument-hint:` What arguments the skill expects
- `user-invocable:` `true` to make it callable with `/name`
- `allowed-tools:` Tools the skill can use (optional)

---

## Updating Framework

To get the latest version:

1. Pull latest changes from this repository:
   ```bash
   cd /path/to/skills-and-agents
   git pull
   ```

2. Re-run the installation commands from step 3 in the [Installation Steps](#installation-steps) to update your project's skills

---

## Tool Compatibility

This framework follows Claude Code's skill structure and is designed for:

- **Claude Code** (`.claude/skills/`) - Primary support, skills auto-loaded
- **Cursor** - Adapt paths and invocation method
- **Windsurf** - Adapt paths and invocation method
- **Custom AI Tools** - Use the markdown files as templates

For other tools, adapt the file structure to match your tool's skill/agent format.

---

## Troubleshooting

### Skills Not Loading

1. Verify files are in `.claude/skills/` directory
2. Check frontmatter has `user-invocable: true`
3. Restart Claude Code to reload skills
4. Run `/help` to see loaded skills

### Skills Not Executing Correctly

1. Check skill references correct protocols (in `.claude/skills/protocols/`)
2. Verify skill directory structure is correct (`skill-name/SKILL.md`)
3. Review Claude Code logs for errors

---

## References

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [How to Use Skills in Claude Code](https://skywork.ai/blog/how-to-use-skills-in-claude-code-install-path-project-scoping-testing/)
- [Mastering Claude Skills Guide](https://apidog.com/blog/claude-skills/)

---

## License

MIT
