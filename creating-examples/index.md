# Creating Examples and Tutorials

Setup guides for skills that help you create educational content.

---

## What's Here

This directory contains setup guides for creating examples and tutorials:

| Guide | Skills | Use When |
|-------|--------|----------|
| [Examples and Tutorials](examples-and-tutorials-setup.md) | `create-example`, `create-tutorial` | Need to create educational content |

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    create-example                            │
│  (Create complete, working framework examples)               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│   │   Research   │ -> │    Design    │ -> │  Implement   │  │
│   │              │    │              │    │              │  │
│   │ Framework    │    │ Example      │    │ Write code   │  │
│   │ docs + prior │    │ structure    │    │ + tests      │  │
│   │ examples     │    │              │    │              │  │
│   └──────────────┘    └──────────────┘    └──────────────┘  │
│          │                                       │           │
│          │           ┌──────────────┐           │           │
│          └────────── │  Document +  │ <─────────┘           │
│                      │  Integrate   │                        │
│                      └──────────────┘                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                   create-tutorial                            │
│  (Create step-by-step learning guides)                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│   │   Outline    │ -> │    Write     │ -> │   Review     │  │
│   │              │    │              │    │              │  │
│   │ Define       │    │ Step-by-step │    │ Test with    │  │
│   │ learning     │    │ instructions │    │ target       │  │
│   │ objectives   │    │ + code       │    │ audience     │  │
│   └──────────────┘    └──────────────┘    └──────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Which Skill Do I Need?

**Examples vs Tutorials:**

| Aspect | `create-example` | `create-tutorial` |
|--------|------------------|-------------------|
| **Purpose** | Demonstrate a concept | Teach how to do something |
| **Format** | Working code + brief docs | Step-by-step guide |
| **Audience** | Developers who learn by reading code | Developers who learn by following along |
| **Output** | Code files + README | Markdown guide with code snippets |

**Use `create-example` when:**
- Demonstrating a framework feature
- Showing a pattern or technique
- Creating reference implementations
- Building sample applications

**Use `create-tutorial` when:**
- Teaching a concept from scratch
- Guiding through a process step-by-step
- Creating onboarding material
- Writing how-to guides

---

## Example Workflow

```
1. Research
   - Read framework documentation
   - Review existing examples
   - Understand the concept to demonstrate

2. Design
   - Define what the example shows
   - Plan code structure
   - Identify edge cases to cover

3. Implement
   - Write the example code
   - Add comprehensive tests
   - Document the code

4. Integrate
   - Register in application (if applicable)
   - Add to documentation index
   - Test end-to-end

5. Grievances (optional)
   - Document framework friction points
   - Suggest improvements
```

---

## Shared Resources

All guides reference these shared documents:
- [Repository Detection](../shared/repo-detection.md) - Phase 0 setup
- [Prerequisites](../shared/prerequisites.md) - Phase 1 validation
- [Code Review Checklist](../shared/code-review-checklist.md) - Review standards
- [Interview Protocol](../shared/interview-protocol.md) - Customization process

---

## Related Intents

- [Building Features](../building-features/) - When you need to build a real feature, not an example
