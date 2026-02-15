Purpose: This document defines the template for an individual subagent.

**Skills vs Subagents:**
- **Skills** teach expertise that runs in the main conversation or a forked context
- **Subagents** are specialized AI assistants with their own context windows that execute tasks independently

Use this template when creating a subagent that will be delegated to by skills or by the AI assistant directly.

---

## Frontmatter

The frontmatter configures how the AI coding tool treats this agent. Fields vary by tool.

**Core fields (supported by most tools):**

```yaml
---
name: [agent-name]
description: [When to delegate to this agent - used by AI to decide when to use it]
---
```

**Extended fields (tool-dependent):**

```yaml
---
name: [agent-name]
description: [When to delegate to this agent]

# Tool restrictions (which tools the agent can use)
# Claude Code: tools, disallowedTools
# Example: tools: Read, Grep, Glob, Bash

# Model selection
# Claude Code: model (sonnet/opus/haiku/inherit)
# Other tools: varies

# Additional Claude Code fields:
# permissionMode, maxTurns, skills, hooks, memory, mcpServers

# Consult your AI coding tool's documentation for supported fields
---
```

**Placeholder reference:**

| Placeholder | Claude Code | Cursor | Windsurf |
|-------------|-------------|--------|----------|
| `${TOOL_CONFIG}` | `.claude/` | `.cursor/` | `.windsurf/` |
| `${AGENT_DIR}` | `agents/` | `agents/` | varies |

Agent file location: `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name].md`

---

## Template

[TEMPLATE]

```yaml
---
name: [agent-name]
description: [When to delegate to this agent. Be specific so the AI knows when to use it.]
# Add tool-specific fields as needed (see Frontmatter section above)
---
```

# Agent: [Agent Display Name]

---

# Background

[Context and motivation. Why does this agent exist? What problems led to its creation?]

---

# Expertise

[Domain knowledge this agent possesses]

**Domain expertise:**
- [Expertise area 1]
- [Expertise area 2]
- [Expertise area 3]

**Example domains:**
- `Code Review` - Code quality, conventions, design patterns, security
- `Test Architecture` - Test strategy, coverage, test pyramid, TDD
- `Security Analysis` - Vulnerabilities, OWASP, input validation, secrets
- `Performance` - Profiling, optimization, bottleneck identification

---

# When to Delegate

**Delegate to this agent when:**
- [Use case 1]
- [Use case 2]

**Do NOT delegate when:**
- [Anti-use case 1]
- [Anti-use case 2]

---

# Input

**What this agent receives:**

| Input | Required | Description |
|-------|----------|-------------|
| [Input 1] | Yes/No | [What it is and expected format] |
| [Input 2] | Yes/No | [What it is and expected format] |

**Input validation:**
- [How to validate input before processing]
- [What to do if input is invalid]

---

# Process

**How this agent produces output:**

## Step 1: [Name]

[What to do in this step]

## Step 2: [Name]

[What to do in this step]

## Step 3: [Name]

[What to do in this step]

[Add more steps as needed]

---

# Output Format

**Structure of agent's response:**

```markdown
## [Output Title]

**[Section 1]:** [content]
**[Section 2]:** [content]

### [Subsection]

[Structured content - tables, lists, etc.]

### [Next Steps/Recommendations]

[What to do with this output]
```

---

# Quality Checklist

**Before returning output, verify:**
- [ ] [Critical criterion 1]
- [ ] [Critical criterion 2]
- [ ] [Completeness criterion]
- [ ] [Format criterion]
- [ ] [Accuracy criterion]

---

# Example

**Input:** [Sample input to agent]

**Analysis:** [What agent considers internally - show reasoning]

**Output:**
```markdown
[Example of agent's actual output following the Output Format]
```

---

# Integration

**How skills delegate to this agent:**

```markdown
## Phase N: [Phase Name]

Delegate to [agent-name]:

**Input:** [What to pass to the agent]
**Focus:** [Optional focus areas or constraints]

Handle agent response:
- If [condition]: [action]
- If [condition]: [action]
```

**Standalone delegation example:**
> "[Natural language example of how a user or AI might invoke this agent]"

---

# Related Agents

**Works with:**
- [Agent that complements this one]

**Alternative to:**
- [Agent that solves similar problem differently]

**Delegated from:**
- [Skills or workflows that commonly use this agent]
