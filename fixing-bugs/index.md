# Fixing Bugs

Setup guides for skills that help you diagnose and fix bugs.

---

## What's Here

This directory contains setup guides for systematic bug fixing:

| Guide | Skills | Use When |
|-------|--------|----------|
| [Full Workflow](full-workflow-setup.md) | `bugfix-workflow` | Want guided, systematic bug fixing |
| [Implement Fix](implement-fix-setup.md) | `bugfix-impl` | Know the root cause, ready to fix |

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    bugfix-workflow                           │
│  (Hypothesis-based debugging with systematic testing)        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│   │   Reproduce  │ -> │  Hypothesize │ -> │    Test      │  │
│   │              │    │              │    │              │  │
│   │ Confirm bug  │    │ 3+ possible  │    │ Validate/    │  │
│   │ exists       │    │ root causes  │    │ Eliminate    │  │
│   └──────────────┘    └──────────────┘    └──────────────┘  │
│          │                                       │           │
│          │           ┌──────────────┐           │           │
│          └────────── │  bugfix-impl │ <─────────┘           │
│                      │              │                        │
│                      │ Fix + Test   │                        │
│                      └──────────────┘                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Which Guide Do I Need?

**Start here if you're not sure:**

1. **"Something is broken and I don't know why"** → Start with [Full Workflow](full-workflow-setup.md)
   - Guides you through systematic debugging
   - Generates multiple hypotheses before diving in
   - Tests each hypothesis to find root cause
   - Includes fix implementation

2. **"I know exactly what's wrong"** → Use [Implement Fix](implement-fix-setup.md)
   - Skip diagnosis
   - Write regression test first
   - Implement fix
   - Verify no regressions

---

## Key Methodology: Hypothesis-Based Debugging

The bugfix workflow uses a systematic approach:

1. **Reproduce** - Confirm the bug exists and is reproducible
2. **Hypothesize** - Generate at least 3 possible root causes
3. **Test** - Design tests to validate or eliminate each hypothesis
4. **Identify** - Narrow down to actual root cause
5. **Fix** - Implement fix with regression test
6. **Verify** - Ensure fix works and no regressions

**Why 3+ hypotheses?**
- Prevents tunnel vision on first guess
- Ensures systematic exploration
- Often reveals the actual cause is different than expected

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
- [Refactoring](../refactoring/) - When you need to improve existing code without fixing bugs
