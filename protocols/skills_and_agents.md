# Skills and Agents Protocol

Define when to use skills versus agents for extending AI coding assistant capabilities.

---

## Goal

Enable appropriate selection between skills and agents based on task requirements, execution model, and context management needs.

---

## Intent

AI coding assistants can be extended through skills (modular capability packages) and agents (specialized execution contexts). Without clear criteria for when to use each, teams create redundant solutions, misapply patterns, or build capabilities that don't fit their needs. This protocol provides a framework for understanding the differences and choosing the right approach.

---

## Scope

This protocol addresses the distinction between skills and agents: their architecture, invocation patterns, context handling, and appropriate use cases. It covers file locations, naming conventions, and integration patterns across AI coding tools.

---

## Key Results

- Skills and agents are chosen appropriately for each use case
- Context management is efficient (no unnecessary token consumption)
- Capabilities are reusable and composable
- Invocation patterns match user and AI needs
- File organization follows tool conventions

---

## Essential Concepts

### Skills

**Skills are modular capability packages that extend what an AI assistant can do.**

A skill is a directory containing a `SKILL.md` file with instructions, metadata, and optional supporting resources (scripts, templates, reference docs). Skills teach Claude how to complete specific tasks in a repeatable way.

**Key characteristics:**
- **On-demand loading**: Only loaded into context when relevant
- **Progressive disclosure**: Metadata loaded at startup, full content when triggered
- **Inline or forked**: Can run in main conversation or isolated context
- **User or AI invocable**: Can be triggered by `/skill-name` or automatically by Claude

**Structure:**
```
skill-name/
├── SKILL.md              # Main instructions (required)
├── reference.md          # Additional docs (loaded as needed)
├── examples.md           # Usage examples (loaded as needed)
└── scripts/
    └── helper.py         # Utility scripts (executed, not loaded)
```

**SKILL.md format:**
```yaml
---
name: skill-name
description: What this skill does and when to use it
---

Instructions for Claude to follow when this skill is active.
```

### Agents (Subagents)

**Agents are specialized AI instances that handle tasks autonomously in isolated context.**

An agent runs in its own context window with custom system prompts, tools, and permissions. The main conversation delegates tasks to agents, which return summarized results.

**Key characteristics:**
- **Isolated context**: Separate context window from main conversation
- **Autonomous execution**: Works independently, returns results
- **Specialized focus**: Configured for specific domains or tasks
- **Delegation model**: AI decides when to invoke, not user

**When agents execute:**
1. Main conversation recognizes a task suitable for delegation
2. Agent is spawned with its own context and system prompt
3. Agent works autonomously using its configured tools
4. Results are summarized and returned to main conversation

### Three-Level Loading (Skills)

Skills use progressive disclosure to manage context efficiently:

| Level | When Loaded | Token Cost | Content |
|-------|-------------|------------|---------|
| **Level 1: Metadata** | At startup | ~100 tokens | `name` and `description` from YAML |
| **Level 2: Instructions** | When triggered | Under 5k tokens | SKILL.md body |
| **Level 3: Resources** | As needed | Unlimited | Bundled files, scripts |

This architecture allows many skills to be installed without context penalty. Only relevant content loads when needed.

---

## Processes

### Choosing Skills vs Agents

**Use Skills when:**
- Task requires domain expertise applied to user's current work
- Instructions should be reusable across conversations
- User may want to invoke directly (`/skill-name`)
- Content includes workflows, conventions, or best practices
- Output integrates with the current conversation

**Use Agents when:**
- Task can be fully delegated without user interaction
- Specialized focus benefits from isolated context
- High-volume operations (tests, logs, exploration) should not pollute main context
- Parallel processing of independent tasks is valuable
- Consistent methodology matters more than conversation continuity

### Skill Invocation Patterns

**Both user and Claude can invoke (default):**
```yaml
---
name: api-conventions
description: API design patterns for this codebase
---
```
- Claude loads automatically when relevant
- User can invoke with `/api-conventions`

**User-only invocation:**
```yaml
---
name: deploy
description: Deploy the application to production
disable-model-invocation: true
---
```
- Prevents Claude from triggering automatically
- Use for side effects, destructive actions, or timing-sensitive workflows

**Claude-only invocation:**
```yaml
---
name: legacy-context
description: Background knowledge about the legacy system
user-invocable: false
---
```
- Hides from `/` menu
- Use for background knowledge that isn't actionable as a command

### Running Skills in Agent Context

Skills can run in a forked subagent context with `context: fork`:

```yaml
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly...
```

This creates an isolated context where:
- The skill content becomes the task
- The agent type (`Explore`, `Plan`, etc.) determines tools and model
- Results are summarized and returned to main conversation

---

## Patterns and Anti-Patterns

### Patterns

**Pattern 1: Reference Content as Skills**

Use skills for knowledge Claude applies to current work:

```yaml
---
name: api-conventions
description: API design patterns for this codebase
---

When writing API endpoints:
- Use RESTful naming conventions
- Return consistent error formats
- Include request validation
```

**Pattern 2: Task Workflows as User-Invoked Skills**

Use `disable-model-invocation: true` for workflows with side effects:

```yaml
---
name: deploy
description: Deploy to production
disable-model-invocation: true
---

1. Run test suite
2. Build application
3. Push to deployment target
```

**Pattern 3: Heavy Operations as Agent Tasks**

Delegate high-volume or exploratory work to agents:

- Code exploration across large codebases → Explore agent
- Test suite execution and analysis → dedicated test agent
- Security audit with many file reads → security review agent

**Pattern 4: Progressive Disclosure with Supporting Files**

Keep SKILL.md focused, link to details:

```markdown
# API Processing

## Quick start
[Essential instructions here]

## Advanced features
See [ADVANCED.md](ADVANCED.md) for edge cases
See [REFERENCE.md](REFERENCE.md) for API details
```

### Anti-Patterns

**Anti-Pattern 1: Background Knowledge as User-Invokable Skill**

```yaml
# Bad: User can't meaningfully invoke this
---
name: system-architecture
description: How the system is architected
---

# Good: Hide from menu, let Claude use when relevant
---
name: system-architecture
description: How the system is architected
user-invocable: false
---
```

**Anti-Pattern 2: Side Effects Without Protection**

```yaml
# Bad: Claude might deploy unexpectedly
---
name: deploy
description: Deploy to production
---

# Good: Require user invocation
---
name: deploy
description: Deploy to production
disable-model-invocation: true
---
```

**Anti-Pattern 3: Verbose Skills**

```markdown
# Bad: Over-explains what Claude already knows
PDF (Portable Document Format) files are a common file format that
contains text, images, and other content. To extract text from a PDF,
you'll need to use a library...

# Good: Concise, assumes Claude's knowledge
Use pdfplumber for text extraction:
```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
```

**Anti-Pattern 4: Monolithic Skills**

```markdown
# Bad: Everything in one file
[500+ lines of instructions, examples, reference docs]

# Good: Progressive disclosure
SKILL.md (overview, ~100 lines)
├── reference.md (loaded when needed)
├── examples.md (loaded when needed)
└── edge-cases.md (loaded when needed)
```

---

## File Locations

File locations vary by AI coding tool. Use placeholders in documentation:

| Placeholder | Claude Code | Cursor | Windsurf |
|-------------|-------------|--------|----------|
| `${TOOL_CONFIG}` | `.claude/` | `.cursor/` | `.windsurf/` |

### Skill Locations (Claude Code)

| Scope | Path | Applies to |
|-------|------|------------|
| Personal | `~/.claude/skills/<name>/SKILL.md` | All your projects |
| Project | `.claude/skills/<name>/SKILL.md` | This project only |
| Plugin | `<plugin>/skills/<name>/SKILL.md` | Where plugin is enabled |

### Agent Locations (Claude Code)

| Scope | Path |
|-------|------|
| Personal | `~/.claude/agents/<name>.md` |
| Project | `.claude/agents/<name>.md` |

---

## Naming Conventions

### Skills

Use kebab-case with descriptive names:

**Good:**
- `feature-define` - Define a feature (atomic skill)
- `feature-workflow` - Full feature development (orchestrator)
- `api-conventions` - API design patterns (reference)
- `deploy-production` - Deploy to production (task)

**Avoid:**
- `helper`, `utils`, `tools` (vague)
- `do-stuff`, `process` (unclear)
- Names with `anthropic` or `claude` (reserved)

### Agents

Use domain or role-based names:

**Good:**
- `code-review` - Reviews code quality
- `test-architect` - Designs test strategies
- `security-reviewer` - Reviews security vulnerabilities
- `Explore` - Built-in exploration agent

---

## Examples

### Example 1: Skill for Code Conventions

```yaml
---
name: api-conventions
description: API design conventions for this codebase. Use when writing or reviewing API endpoints.
---

# API Conventions

## Endpoint naming
- Use plural nouns: `/users`, `/orders`
- Use kebab-case: `/user-profiles`

## Response format
Always return:
```json
{
  "data": {...},
  "meta": {"timestamp": "..."}
}
```

## Error handling
Return appropriate HTTP status codes with error details.
```

### Example 2: User-Invoked Workflow Skill

```yaml
---
name: release
description: Create a release with changelog and version bump
disable-model-invocation: true
argument-hint: "[version]"
---

# Release Workflow

Create release for version $ARGUMENTS:

1. Update version in package.json
2. Generate changelog from commits since last tag
3. Create git tag
4. Push tag to origin

Confirm with user before pushing.
```

### Example 3: Skill with Agent Execution

```yaml
---
name: security-audit
description: Audit codebase for security issues
context: fork
agent: Explore
allowed-tools: Read, Grep, Glob
---

# Security Audit

Analyze the codebase for security vulnerabilities:

1. Check for hardcoded secrets (API keys, passwords)
2. Review authentication/authorization patterns
3. Look for SQL injection, XSS, CSRF vulnerabilities
4. Check dependency vulnerabilities

Report findings with severity, location, and remediation.
```

---

## Key Takeaways

- **Skills teach expertise**: Reusable instructions Claude applies when relevant
- **Agents execute autonomously**: Isolated context for focused, delegated tasks
- **Progressive disclosure**: Only load what's needed to preserve context
- **Invocation control**: Use `disable-model-invocation` for side effects
- **Context matters**: Use `context: fork` when isolation benefits the task
- **Keep skills concise**: Claude is smart; don't over-explain
- **File organization**: Follow tool conventions for discoverability

---

## When to Apply This Protocol

- When deciding how to extend AI assistant capabilities
- When creating new skills or agents
- When organizing capability files in a project
- When debugging why skills aren't triggering
- When optimizing context usage
- When setting up team conventions for AI tooling

---

## References

- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Agent Skills Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Claude Code Skills](https://code.claude.com/docs/en/skills)
- [Equipping Agents with Skills](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills)
