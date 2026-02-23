# Coding Agent Sub-Level Documentation Template

Template for package/module documentation that AI coding agents read to understand sub-level conventions.

---

## Template

```markdown
# [Package/Module Name]

[One-line description]

---

## Intent

**This [package/module/namespace] provides:**
- [Core responsibility 1]
- [Core responsibility 2]

**This does NOT:**
- [Anti-scope - what belongs elsewhere]

---

## Public API

**Classes:**
- `ClassName` - [Description]

**Functions:**
- `function_name()` - [Description]

**Constants:**
- `CONSTANT_NAME` - [Description]

---

## Dependencies

**Internal (from this project):**
- `[sibling.module]` - [What we use from it]

**External (third-party):**
- `[library]` - [What we use it for]

---

## Consumers

**Used by:**
- `[other.module]` - [How it uses this]

---

## Adding New Code

**Follow existing patterns:**
- [Pattern to follow when adding X]
- [Pattern to follow when adding Y]

**Common mistakes:**
- [Mistake to avoid]

---

## Conventions (if different from root)

[Only if this package overrides or extends root conventions]
```

---

## Sources

- [Creating the Perfect CLAUDE.md for Claude Code - Dometrain](https://dometrain.com/blog/creating-the-perfect-claudemd-for-claude-code/)
- [AGENTS.md](https://agents.md/)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
