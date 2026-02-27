[TODO: Changed the name to the broader "informing". Currently documentation is too narrow. ]

# Documentation Protocol

Maintain documentation that enables both humans and AI to understand, navigate, and modify code effectively.

---

## Goal

Ensure documentation remains accurate, synchronized with code, and serves both human developers and AI assistants.

---

## Intent

Documentation quality directly impacts comprehension for both humans and AI. Outdated or missing documentation causes confusion, poor suggestions, and maintenance burden. Documentation must be treated as code: versioned, reviewed, and maintained with rigor.

---

## Scope

**Addresses:** Documentation maintenance, synchronization with code, package/module documentation, structure for comprehension

**Does not address:** Marketing copy, user-facing product documentation, API reference generation tools

---

## Core Concepts

### Two Consumers, Same Needs

Documentation serves humans and AI equally. Both need:

1. **Facts** - What the code does (signatures, parameters, behavior)
2. **Intent** - Why it exists (design decisions, tradeoffs, constraints)
3. **Patterns** - How to use it (examples, common cases)

### Diátaxis: Four Documentation Types

Know which type you're writing ([Diátaxis](https://diataxis.fr/)):

| Type | Purpose | Example |
|------|---------|---------|
| **Tutorial** | Guided learning | "Build your first X" |
| **How-to** | Solve specific problem | "How to configure auth" |
| **Reference** | Accurate facts | API signatures |
| **Explanation** | Understanding context | "Why we use saga pattern" |

### Minimalism and Cognitive Load

From [Carroll's minimalism](https://en.wikipedia.org/wiki/Minimalism_(technical_communication)) and [Cognitive Load Theory](https://www.docsie.io/blog/glossary/cognitive-load/):

- **Support doing, not reading** - Task-focused, action-oriented
- **Short chunks** - Not lengthy monolithic docs
- **Progressive disclosure** - Overview first, details on demand
- **Real examples** - Anchor in the task domain, not abstract explanations

---

## Package Documentation

**Every package/folder needs documentation.** Essential for understanding structure, respecting boundaries, and maintaining modularity (see `protocols/modularity.md`).

### Required Elements

```markdown
# [Package Name]

[One sentence: what this does]

## Purpose
What this provides. What it does NOT do (boundaries).

## Dependencies
**Internal:** Sibling packages and why
**External:** Libraries and why

## Contents
- `file.py` - Description
- `subpackage/` - Description

## Entry Points
Primary classes/functions consumers use.
```

Without package docs: code lands in wrong places, dependencies tangle, boundaries erode.

---

## Synchronization

**Same commit rule:** Documentation updates happen in the same commit as code changes. Separating them guarantees drift.

**Triggers:**
- API changes → update docstrings
- New package → create README
- Behavior changes → update docs
- Dependency changes → update package docs

**Single source of truth:** Each fact lives in one place. Link, don't duplicate.

---

## Techniques

- **Proximity** - Docs live close to code (docstrings > separate files > wikis)
- **Hierarchy** - Project README → Package README → Function docs
- **Consistent terminology** - Same term for same concept
- **Examples over explanations** - Show, don't just tell
- **Delete outdated docs** - Stale docs mislead; rely on git history
- **Review docs with code** - PRs without doc updates are incomplete

---

## Anti-Patterns

- **Documentation debt** - "Until later" never comes. Update in same commit.
- **Scattered docs** - Wiki here, Confluence there. Keep everything in repo.
- **Missing package docs** - AI and humans can't understand structure. README every folder.
- **Implementation details** - Internals change. Document behavior and contracts.

---

## Example: Package README

```markdown
# auth/

User authentication and session management.

## Purpose
Provides authentication flows (login, logout, password reset) and session handling.
Does NOT handle authorization - see `authz/`.

## Dependencies
**Internal:** `db/` (storage), `email/` (notifications)
**External:** `bcrypt` (hashing), `jwt` (tokens)

## Contents
- `login.py` - Login flow
- `session.py` - Session management
- `reset.py` - Password reset
- `tokens.py` - JWT handling

## Entry Points
- `authenticate(username, password) -> User`
- `create_session(user) -> Session`
- `validate_token(token) -> User`
```

---

## When to Apply

- Implementing code → update docs in same commit
- Adding packages → create README immediately
- Changing APIs → update affected docstrings
- Code review → verify docs match code

---

## References

- [Diátaxis Framework](https://diataxis.fr/) - Documentation types
- [Minimalism](https://en.wikipedia.org/wiki/Minimalism_(technical_communication)) - Carroll's approach
- [Cognitive Load Theory](https://www.docsie.io/blog/glossary/cognitive-load/) - Information density
