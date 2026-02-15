# Agents

Setup guides for AI agents that can be delegated to for specialized tasks.

---

## What's Here

This directory contains setup guides for creating agents:

| Guide | Agent | Use When |
|-------|-------|----------|
| [Code Review Agent](code-review-agent-setup.md) | `code-review-agent` | Need automated code review against standards |
| [Test Architect Agent](test-architect-agent-setup.md) | `test-architect-agent` | Need test strategy design or test generation |

---

## Skills vs Agents

Understanding when to use each:

| Aspect | Skills | Agents |
|--------|--------|--------|
| **Invocation** | User invokes directly (`/feature-define`) | AI delegates to agent |
| **Interaction** | Multi-phase with user gates | Autonomous, returns result |
| **Scope** | Complete workflows | Focused, specialized tasks |
| **Output** | Artifacts (PRDs, plans, code) | Analysis, recommendations, generated code |

### When to Use Skills

- User-driven workflows with checkpoints
- Multi-phase processes requiring approval
- Tasks that produce versioned artifacts
- Processes with defined start and end states

### When to Use Agents

- Specialized expertise needed
- Task can be fully delegated
- Consistent methodology matters
- Reducing main conversation complexity

---

## Agent Architecture

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

---

## Creating New Agents

### Agent Structure

Agents follow a standard pattern:

```markdown
# [Agent Name]

You are an expert [domain] with deep knowledge of [specialties].

## Your Expertise

- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]

## Your Process

Before [output], you must:

### Step 1: [Name]
[What to do]

### Step 2: [Name]
[What to do]

### Step 3: [Name]
[What to do]

## Output Format

Provide [output type] in this structure:

```[format]
[template]
```

## Quality Checklist

Before finalizing, verify:
- [ ] [Critical criterion 1]
- [ ] [Critical criterion 2]
- [ ] [Completeness criterion]

## Example

**Input:** [Sample input]

**Analysis:** [What agent considers]

**Output:** [Example output]
```

### Naming Convention

Agents follow `{role}-{domain}` or `{expertise}-{specialty}` pattern:

- `code-review-agent` - Reviews code quality
- `test-architect-agent` - Designs test strategies
- `security-reviewer-agent` - Reviews security
- `performance-optimizer-agent` - Optimizes performance

---

## Available Agents

### code-review-agent

**Expertise:** Code quality, project conventions, design principles

**Use for:**
- Reviewing code against project standards
- Checking naming conventions
- Validating design patterns
- Ensuring test coverage

**Output:** Structured review report with issues and recommendations

### test-architect-agent

**Expertise:** Test strategy, test design, coverage analysis

**Use for:**
- Designing test strategies for new features
- Generating test structures
- Analyzing coverage gaps
- Recommending test approaches

**Output:** Test plan or generated test code

---

## Shared Resources

All agent setup guides reference:
- [Repository Detection](../shared/repo-detection.md) - Understanding project context
- [Code Review Checklist](../shared/code-review-checklist.md) - Quality standards
- [Interview Protocol](../shared/interview-protocol.md) - Customization process

---

## Future Agents

Planned agent types (not yet documented):

- `security-reviewer-agent` - Security vulnerability analysis
- `performance-optimizer-agent` - Performance analysis and optimization
- `api-design-agent` - API design review and suggestions
- `documentation-agent` - Documentation generation and review
