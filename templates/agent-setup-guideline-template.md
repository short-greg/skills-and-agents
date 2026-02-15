Purpose: This document defines how to create a guideline for setting up a particular type of subagent.

**IMPORTANT: Guidelines are setup guides, NOT pre-filled agents.**
- Do NOT write complete AGENT.md content in the guideline
- Do NOT pre-fill all sections from agent-template.md
- DO describe the agent's purpose, expertise, and output format
- DO define the full agent suite (related specialists for a domain)
- DO reference agent-template.md for users to fill during interview
- See code-review.md as the canonical example

**Skills vs Subagents:**
- **Skills** = reusable instructions (orchestrator + atomic pattern, phases with user gates)
- **Subagents** = specialized AI assistants (single-purpose specialists, autonomous execution)

Subagents do NOT have orchestrator/atomic relationships. Each agent is a standalone specialist.
Skills orchestrate workflows; subagents execute delegated tasks.

[TEMPLATE]

# [Guideline Name]

---

# Overview

## Name
[Agent domain name - e.g., "code-review", "test-architect"]

## Background
[Context and motivation. Why does this guideline exist? What problems led to its creation?]

## Agent Intent
[What problem does this agent solve? What specialized expertise does it provide?]

## Agents in This Suite

| Agent | Purpose | Specialization |
|-------|---------|----------------|
| `[agent-name]` | [What this agent does] | [Domain focus] |
| `[agent-name-2]` | [What this agent does] | [Domain focus] |

**Note:** Agent suites are collections of related specialists, not hierarchical relationships.
Each agent operates independently. Skills handle orchestration when multiple agents are needed.

### When to Delegate to Each Agent

**Delegate to `[agent-name]` when:**
- [Use case 1]
- [Use case 2]

**Delegate to `[agent-name-2]` when:**
- [Use case 1]
- [Use case 2]

## When to Use This Guideline

**Use this guideline when:**
- [Use case 1]
- [Use case 2]

**Do NOT use when:**
- [Anti-use case 1]
- [Anti-use case 2]

## Guideline OKRs

[See [references/goals-and-objectives.md](../references/goals-and-objectives.md)]

**Objective:** [Aspirational outcome for agents created from this guideline]

**Key Results:**
1. [Measurable outcome 1]
2. [Measurable outcome 2]
3. [Measurable outcome 3]

## Guideline Checklist

**Process to create agents from this guideline:**

- [ ] 1. Read this guideline and referenced framework docs
- [ ] 2. Interview user for agent requirements (see Agent Output Template)
- [ ] 3. Customize for repo type (prototype/production/library)
- [ ] 4. Confirm agent designs with user
- [ ] 5. Write AGENT.md files:
  - `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name].md`
  - `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name-2].md` (if applicable)
- [ ] 6. Test agent by delegating a simple task

---

# Process

**How to design an agent:**

1. Read project documentation (see [references/skills-vs-agents.md](../references/skills-vs-agents.md) for tool-specific locations) to understand conventions
2. Identify the domain expertise needed
3. Define what inputs the agent receives
4. Design the step-by-step process the agent follows
5. Create a structured output format
6. Build a quality checklist for self-validation
7. Write example input/output demonstrations

**Agent process design:**
```yaml
expertise:
  - "[Domain knowledge area 1]"
  - "[Domain knowledge area 2]"
  - "[Domain knowledge area 3]"

steps:
  step_1:
    name: "[Understand Context]"
    action: "[Read project conventions, understand the input]"
  step_2:
    name: "[Analyze]"
    action: "[Apply expertise to the input]"
  step_3:
    name: "[Generate Output]"
    action: "[Produce structured response]"

quality_checks:
  - "[Check 1 - completeness]"
  - "[Check 2 - accuracy]"
  - "[Check 3 - format compliance]"
```

---

# Procedures

[Each procedure describes how a specific agent type works]

## Procedure: [Agent Name]

**Purpose:** [What this agent accomplishes]

**Expertise areas:**
- [Area 1]
- [Area 2]

**Process steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Output format:**
- [What the output looks like]
- [Key sections included]

**Override Points:**
- [What can be customized per project]

---

# Customization by Repo Type

## Prototype
- [Lighter analysis]
- [Fewer checks]
- [Focus on speed over thoroughness]

## Production
- [Full analysis]
- [All quality checks]
- [Comprehensive output]

## Library
- [Extra API/compatibility focus]
- [Public interface emphasis]
- [Documentation requirements]

---

# Framework References

- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria

---

# Agent Output Template

Use [agent-template.md](agent-template.md) as the base for creating each AGENT.md file.

**During the interview process, fill in these sections for each agent:**

1. **Frontmatter** - name, description, tool-specific fields
2. **Background** - why this agent exists
3. **Expertise** - domain knowledge areas
4. **When to Delegate** - use cases and boundaries
5. **Input** - what the agent receives
6. **Process** - step-by-step methodology
7. **Output Format** - structured response template
8. **Quality Checklist** - verification before returning
9. **Example** - input/analysis/output demonstration
10. **Integration** - how skills delegate to this agent
11. **Related Agents** - connections to other agents in suite

**Suite structure:**
- Each agent is a standalone specialist
- No orchestrator/atomic relationship (skills handle orchestration)
- Related agents share a domain but have distinct specializations

**Output locations:**
- `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name].md`
- `${TOOL_CONFIG}/${AGENT_DIR}/[agent-name-2].md`

**Placeholder reference:**

| Placeholder | Claude Code | Cursor | Windsurf |
|-------------|-------------|--------|----------|
| `${TOOL_CONFIG}` | `.claude/` | `.cursor/` | `.windsurf/` |
| `${AGENT_DIR}` | `agents/` | `agents/` | varies |
