# Skill Template

Base structure for creating SKILL.md files across all AI tools.

---

## Purpose

This template provides the standard structure for skill definition files. Adapt the format to your AI tool's requirements.

---

## SKILL.md Template

```markdown
---
name: [skill-name]
description: [One-line description of what this skill does]
disable-model-invocation: true
argument-hint: "[expected arguments]"
---

# [Skill Display Name]

[One sentence describing what this skill does and when to use it.]

## When to Use

Use this skill when:
- [Condition 1]
- [Condition 2]
- [Condition 3]

Do NOT use when:
- [Anti-condition 1]
- [Anti-condition 2]

## Input

- [Input 1]: [Description]
- [Input 2]: [Description]

## Output

- [Output 1]: [Description and location]
- [Output 2]: [Description]

## Process

### Phase 0: [Optional] Orchestration Check

If running as part of an orchestrated workflow:
1. Check for orchestration context (task ID, dependencies)
2. Verify dependencies are met
3. Read and apply any overrides from PRD/plan
4. Mark task as in-progress

If not orchestrated:
- Skip to Phase 1 (standalone execution)

---

### Phase 1: [First Phase Name]

**Purpose:** [What this phase accomplishes]

**Steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Output:**
```markdown
[Expected output format]
```

**Gate:** [Condition to proceed to next phase]

---

### Phase 2: [Second Phase Name]

**Purpose:** [What this phase accomplishes]

**Steps:**
1. [Step 1]
2. [Step 2]

**Output:**
[Description of output]

**Gate:** [Condition to proceed]

---

### Phase N: [Final Phase Name]

**Purpose:** [What this phase accomplishes]

**Steps:**
1. [Step 1]
2. [Final step]

**Output:**
[Final deliverable description]

---

## Completion

When all phases are complete, provide a summary:

```markdown
## [Skill Name] Complete

**Output:** [Path to created artifact]

**Summary:**
- [Key point 1]
- [Key point 2]

**Next Steps:**
1. [What to do next]
2. [Optional follow-up]
```

---

## Best Practices

- [Best practice 1]
- [Best practice 2]
- [Best practice 3]

## Anti-Patterns to Avoid

- [Anti-pattern 1]: [Why it's bad]
- [Anti-pattern 2]: [Why it's bad]
```

---

## Template Notes

### Frontmatter Fields

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Skill identifier (used for invocation) | `feature-define` |
| `description` | Brief description shown in help | `Create PRD from feature ideas` |
| `disable-model-invocation` | Prevents AI from auto-running (enforces step-by-step) | `true` |
| `argument-hint` | Shows expected arguments in help | `[path-to-file or description]` |

### Phase Structure

Each phase should have:
1. **Purpose** - Why this phase exists
2. **Steps** - Explicit instructions (numbered)
3. **Output** - What this phase produces
4. **Gate** - Condition to proceed (user approval, validation, etc.)

### Naming Convention

Skills follow `{task}-{action}` pattern:
- `feature-define` - Define feature requirements
- `feature-plan` - Plan feature development
- `feature-impl` - Implement feature
- `bugfix-impl` - Implement bug fix
- `refactor-impl` - Implement refactor

Exception: Content creation skills use `{action}-{noun}`:
- `create-example` - Create a framework example
- `create-tutorial` - Create a tutorial

### Related Skills

If skills work together, document the relationship:
- `feature-define` produces PRD → consumed by `feature-plan`
- `feature-plan` produces plan → consumed by `feature-impl`

---

## Tool Adaptation

### Claude Code
- Store in: `.claude/skills/[skill-name]/SKILL.md`
- Invoke with: `/skill-name`

### Cursor
- Store in: `.cursor/skills/[skill-name]/SKILL.md` or `.cursor/prompts/`
- Adapt invocation to Cursor's prompt system

### Codex
- Store in: `.codex/skills/[skill-name]/SKILL.md`
- Adapt to Codex conventions

### Other Tools
- Find your tool's skill/prompt configuration directory
- Adapt frontmatter format as needed
- Core phase structure remains the same
