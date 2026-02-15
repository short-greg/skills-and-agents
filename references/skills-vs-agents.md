# Skills vs Subagents

This reference explains the difference between skills and subagents, and when to use each.

---

## Summary

| Aspect | Skills | Subagents |
|--------|--------|-----------|
| **What they are** | Reusable instructions the AI loads | Specialized AI assistants with own context |
| **Invocation** | User (`/skill-name`) or AI auto-loads | AI delegates via task |
| **Context** | Inline or forked | Always separate context window |
| **Interaction** | Multi-phase with optional user gates | Autonomous, returns result |
| **Structure** | Orchestrator + atomic skills | Single-purpose specialists |
| **Output** | Artifacts (PRDs, plans, code) | Analysis, recommendations, generated code |

---

## Key Insight

> **Skills teach expertise; subagents execute tasks independently.**

- If multiple workflows need the same procedures, create a **skill**
- If a task requires autonomous execution with specialized focus, create a **subagent**

---

## When to Use Skills

- User-driven workflows with checkpoints
- Multi-phase processes requiring approval
- Tasks that produce versioned artifacts
- Processes with defined start and end states
- Reusable expertise that runs inline
- Instructions the AI should know when relevant

**Examples:**
- `/feature-define` - Multi-phase PRD creation with user review
- `/bugfix` - Hypothesis-based debugging with checkpoints
- `api-conventions` - Knowledge the AI applies when writing APIs

---

## When to Use Subagents

- Specialized expertise needed for a focused task
- Task can be fully delegated without user interaction
- Consistent methodology matters
- Reducing main conversation complexity
- Isolating high-volume operations (tests, logs, exploration)
- Parallel processing of independent tasks

**Examples:**
- `code-review` - Autonomous code review returning structured report
- `test-architect` - Test strategy design delegated to specialist
- `Explore` - Read-only codebase exploration in separate context

---

## Subagent Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Main Conversation                         │
│                                                              │
│  User: "Review this code for security issues"               │
│                                                              │
│  AI: [Recognizes security review task]                       │
│      [Delegates to security-reviewer agent]                  │
│                                                              │
│      ┌─────────────────────────────────────────────────┐    │
│      │           security-reviewer agent                │    │
│      │                                                  │    │
│      │  1. Read project conventions                    │    │
│      │  2. Analyze code for vulnerabilities            │    │
│      │  3. Check OWASP top 10                          │    │
│      │  4. Generate report                             │    │
│      │  5. Return findings                             │    │
│      │                                                  │    │
│      └──────────────────┬──────────────────────────────┘    │
│                         │                                    │
│                         ▼                                    │
│  AI: [Presents agent's findings to user]                    │
│      "Found 3 potential issues..."                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Context isolation:** Each subagent runs in its own context window. Results are summarized and returned to the main conversation.

---

## Skills Running in Subagent Context

Some AI coding tools (e.g., Claude Code) allow skills to run in a forked subagent context. This gives the skill's instructions a separate context window while still being invoked as a skill.

| Approach | System prompt | Task | Best for |
|----------|---------------|------|----------|
| Skill (inline) | Main conversation | Skill instructions | Quick knowledge application |
| Skill (forked) | Agent type + skill | Skill instructions | Heavy exploration/analysis |
| Subagent | Custom system prompt | Delegated task | Specialized autonomous work |

---

## File Locations

File locations vary by AI coding tool. Use placeholders:

| Placeholder | Claude Code | Cursor | Windsurf |
|-------------|-------------|--------|----------|
| `${TOOL_CONFIG}` | `.claude/` | `.cursor/` | `.windsurf/` |
| `${SKILL_DIR}` | `skills/` | varies | varies |
| `${AGENT_DIR}` | `agents/` | varies | varies |

**Skills:** `${TOOL_CONFIG}/${SKILL_DIR}/[skill-name]/SKILL.md`
**Subagents:** `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name].md`

---

## Naming Conventions

**Skills:** `{task}-{action}` or `{workflow}-{phase}`
- `feature-define` - Define a feature (atomic skill)
- `feature-workflow` - Full feature development (orchestrator)
- `bugfix-investigate` - Investigation phase of bugfixing

**Subagents:** `{domain}` or `{role}-{specialty}`
- `code-review` - Reviews code quality
- `test-architect` - Designs test strategies
- `security-reviewer` - Reviews security vulnerabilities

---

## Related References

- [templates/skill-template.md](../templates/skill-template.md) - Skill file template
- [templates/agent-template.md](../templates/agent-template.md) - Subagent file template
- [skill-guidelines/](../skill-guidelines/) - Skill setup guides
- [agent-guidelines/](../agent-guidelines/) - Subagent setup guides
