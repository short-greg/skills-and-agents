# Coding Agent Documentation Template

Template for project documentation that AI coding agents read (CLAUDE.md, AGENTS.md, etc.).

---

## Template

```markdown
# [Project Name]

[One-line description]

---

## Project Overview

**What this is:**
- [Core purpose 1]
- [Core purpose 2]

**What this is NOT:**
- [Anti-purpose 1]

---

## Tech Stack

- **Language:** [Python 3.11 / TypeScript 5.x / etc.]
- **Framework:** [FastAPI / React / etc.]
- **Testing:** [pytest / Jest]
- **Database:** [PostgreSQL / etc.]

---

## Common Commands

```bash
# Install dependencies
[npm install | pip install -r requirements.txt]

# Run development server
[npm run dev | python -m app]

# Run tests
[npm test | pytest]

# Build
[npm run build | python -m build]
```

---

## Architecture

**Structure:**
```
[project]/
├── src/           # Source code
├── tests/         # Tests
├── tasks/         # Task Descriptions include specs (${TASK_DIR})
└── .claude/       # AI config (${TOOL_CONFIG})
```

**Key patterns:**
- [Pattern 1]
- [Pattern 2]

---

## Code Style

**Naming:**
- Files: [snake_case]
- Classes: [PascalCase]
- Functions: [snake_case]

**Formatting:**
- [black / prettier]
- [pylint / eslint]

---

## Terminology

| Term | Meaning |
|------|---------|
| [Domain term] | [Definition and how it maps to code] |

---

## Primary Developer Workflows

### Adding a New Feature
1. [Create spec in ${SPEC_DIR}]
2. [Create branch]
3. [Write tests first]
4. [Implement feature]
5. [Run quality checks]
6. [Create PR]

### Fixing a Bug
1. [Reproduce with failing test]
2. [Identify root cause]
3. [Implement minimal fix]
4. [Verify all tests pass]
5. [Create PR]

### Refactoring Code
1. [Ensure tests pass before starting]
2. [Make incremental changes]
3. [Run tests after each change]
4. [Document what changed]

### Adding a New API Endpoint
1. [Define route in routes/]
2. [Create handler in handlers/]
3. [Add validation schemas]
4. [Write integration tests]
5. [Update API docs]

### [Project-Specific Workflow]
1. [Step 1]
2. [Step 2]

---

## Examples

### [Common Use Case 1]
```[language]
[code example showing typical usage]
```

### [Common Use Case 2]
```[language]
[code example]
```

---

## Quality Standards

- Coverage: [threshold]
- Security: [scans]
- Performance: [requirements]

---

## Do / Don't

**Do:**
- [Practice]

**Don't:**
- [Anti-pattern]

---

## Subpackages

Each subpackage may have its own doc file with package-specific conventions.
```

---

## Sources

- [Creating the Perfect CLAUDE.md for Claude Code - Dometrain](https://dometrain.com/blog/creating-the-perfect-claudemd-for-claude-code/)
- [AGENTS.md](https://agents.md/)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
