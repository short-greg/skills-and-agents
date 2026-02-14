# Building Features

Setup guides for skills that help you build new features.

---

## What's Here

This directory contains setup guides for the complete feature development workflow:

| Guide | Skills | Use When |
|-------|--------|----------|
| [Full Workflow](full-workflow-setup.md) | `feature-workflow` | Want guided end-to-end feature development |
| [Define Requirements](define-requirements-setup.md) | `feature-define`, `feature-define-review` | Need to create a PRD from an idea |
| [Plan Development](plan-development-setup.md) | `feature-plan`, `feature-plan-review` | Have a PRD, need a development plan |
| [Implement Feature](implement-feature-setup.md) | `feature-impl` | Have a plan, ready to implement |
| [Parallelize Work](parallelize-work-setup.md) | `parallel-orchestrate` | Large feature, want parallel execution |

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    feature-workflow                          │
│  (Orchestrates the complete flow with state detection)       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│   │feature-define│ -> │ feature-plan │ -> │ feature-impl │  │
│   │              │    │              │    │              │  │
│   │ Create PRD   │    │ Create Plan  │    │ Implement    │  │
│   └──────────────┘    └──────────────┘    └──────────────┘  │
│                                                              │
│   For large features with multiple tasks:                    │
│   ┌─────────────────────────────────────────────────────┐   │
│   │              parallel-orchestrate                    │   │
│   │  (Break into tasks, run in parallel worktrees)      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Which Guide Do I Need?

**Start here if you're not sure:**

1. **"I have a feature idea"** → Start with [Full Workflow](full-workflow-setup.md)
   - Guides you through the complete process
   - Detects existing PRDs/plans and skips completed phases
   - Includes review gates between phases

2. **"I already have a PRD"** → Use [Plan Development](plan-development-setup.md)
   - Skip requirements definition
   - Create development plan from existing PRD

3. **"I already have a plan"** → Use [Implement Feature](implement-feature-setup.md)
   - Skip planning
   - Implement using TDD methodology

4. **"I have a large feature with many tasks"** → Add [Parallelize Work](parallelize-work-setup.md)
   - Break work into independent tasks
   - Run tasks in parallel using Git worktrees
   - Orchestrate merging with dependency tracking

---

## Shared Resources

All guides reference these shared documents:
- [Repository Detection](../shared/repo-detection.md) - Phase 0 setup
- [Prerequisites](../shared/prerequisites.md) - Phase 1 validation
- [Code Review Checklist](../shared/code-review-checklist.md) - Review standards
- [Interview Protocol](../shared/interview-protocol.md) - Customization process
- [Skill Template](../shared/skill-template.md) - SKILL.md structure

---

## Related Intents

- [Fixing Bugs](../fixing-bugs/) - When you need to fix a bug, not build a feature
- [Refactoring](../refactoring/) - When you need to improve existing code
- [Creating Examples](../creating-examples/) - When you need educational content
