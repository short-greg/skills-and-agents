# Refactoring

Setup guides for skills that help you improve existing code safely.

---

## What's Here

This directory contains setup guides for safe code refactoring:

| Guide | Skills | Use When |
|-------|--------|----------|
| [Full Workflow](full-workflow-setup.md) | `refactor-workflow` | Want guided, safe refactoring |
| [Implement Refactor](implement-refactor-setup.md) | `refactor-impl` | Know what to refactor, ready to do it |

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                   refactor-workflow                          │
│  (Safe refactoring with test preservation)                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│   │   Analyze    │ -> │    Plan      │ -> │   Execute    │  │
│   │              │    │              │    │              │  │
│   │ Identify     │    │ Define safe  │    │ Refactor in  │  │
│   │ what to      │    │ steps with   │    │ small steps  │  │
│   │ improve      │    │ test points  │    │ + verify     │  │
│   └──────────────┘    └──────────────┘    └──────────────┘  │
│                                                              │
│   ┌─────────────────────────────────────────────────────┐   │
│   │                  refactor-impl                       │   │
│   │  - Preserve existing tests                          │   │
│   │  - Run tests after each change                      │   │
│   │  - Measure modularity improvement                   │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Which Guide Do I Need?

**Start here if you're not sure:**

1. **"This code needs improvement but I'm not sure where to start"** → Start with [Full Workflow](full-workflow-setup.md)
   - Analyzes codebase for improvement opportunities
   - Plans safe refactoring steps
   - Includes modularity metrics before/after
   - Ensures no regressions

2. **"I know exactly what to refactor"** → Use [Implement Refactor](implement-refactor-setup.md)
   - Skip analysis
   - Execute refactoring safely
   - Preserve and run tests continuously
   - Measure improvement

---

## Key Principles: Safe Refactoring

The refactor workflow emphasizes safety:

1. **Tests First** - Ensure test coverage before changing code
2. **Small Steps** - Make one change at a time
3. **Continuous Verification** - Run tests after each change
4. **No Behavior Change** - Refactoring improves structure, not functionality
5. **Measure Improvement** - Track modularity metrics

**What refactoring IS:**
- Improving code structure
- Reducing duplication
- Improving naming
- Simplifying complex logic
- Breaking up large functions/classes

**What refactoring is NOT:**
- Adding new features
- Fixing bugs
- Changing behavior

---

## Modularity Metrics

The workflow tracks improvement using metrics:

| Metric | What It Measures |
|--------|------------------|
| **Coupling** | How dependent components are on each other |
| **Cohesion** | How related functionality is grouped |
| **Complexity** | Cyclomatic complexity of functions |
| **Duplication** | Repeated code patterns |

Goal: Reduce coupling, increase cohesion, reduce complexity, eliminate duplication.

---

## Shared Resources

All guides reference these shared documents:
- [Repository Detection](../shared/repo-detection.md) - Phase 0 setup
- [Prerequisites](../shared/prerequisites.md) - Phase 1 validation
- [Code Review Checklist](../shared/code-review-checklist.md) - Review standards
- [Interview Protocol](../shared/interview-protocol.md) - Customization process

---

## Related Intents

- [Building Features](../building-features/) - When you need to build something new
- [Fixing Bugs](../fixing-bugs/) - When you need to fix a bug (behavior change)
