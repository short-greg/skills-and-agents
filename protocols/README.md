# Protocols

This directory contains protocols that define standard processes and patterns used across primitives and workflows.

---

## What is a Protocol?

A protocol is a standardized process or pattern that multiple primitives and workflows follow. Protocols ensure consistency and quality across the framework.

**Protocols are:**
- **Reusable** — Referenced by multiple primitives/workflows
- **Prescriptive** — Define how to do something, not what to build
- **Foundational** — Provide common patterns that higher-level components depend on

**Protocols are NOT:**
- Skills (user-invocable)
- Primitives (cognitive actions)
- Workflows (multi-step processes)

---

## Available Protocols

### Creation & Design

**[create_primitive.md](create_primitive.md)**
- How to create new primitives
- Primitive structure and requirements
- Quality guidelines for primitives

**[create_workflow.md](create_workflow.md)**
- How to create workflows that compose primitives
- Workflow types (deterministic vs. adaptive, imperative vs. declarative)
- Workflow patterns from research
- Quality gates and iteration handling

**[manage_complexity_uncertainty_risk.md](manage_complexity_uncertainty_risk.md)**
- Identifying and mitigating complexity in workflows
- Reducing uncertainty before committing
- Assessing and addressing risk
- Relationships between the three concepts

### Execution & Progress

**[tracking.md](tracking.md)**
- How to maintain trace files during workflow execution
- Trace format and structure
- Progress documentation

**[recovery.md](recovery.md)**
- How to resume workflows after interruption
- Recovery rules and patterns
- Checkpoint management

**[checklist_management.md](checklist_management.md)**
- Creating dynamic checklists in workflows
- Adding/removing items during execution
- Loop-back patterns for iteration
- Progress tracking and reporting

### Goals & Design

**[goals_and_objectives.md](goals_and_objectives.md)**
- Writing clear goals and measurable objectives (OKRs)
- Outcome-focused key results vs implementation steps
- Context-specific examples (PRD, technical plan, implementation)
- Anti-patterns and verification checklist

**[skills_and_agents.md](skills_and_agents.md)**
- Understanding skills vs subagents (when to use each)
- Skill structure and progressive disclosure
- SKILL.md format and invocation patterns
- File locations by AI coding tool

### Quality & Maintenance

**[project_quality.md](project_quality.md)**
- Assessing artifact quality across 6 dimensions
- Correctness, clarity, reliability, completeness, efficiency, testability
- Multi-dimensional tradeoff analysis
- Tool-based assessment with evidence requirements
- Customization by repo type (prototype/production/library)

**[modularity.md](modularity.md)**
- Assessing modularity in code, design, and workflows
- 7 modularity dimensions (cohesion, coupling, encapsulation, composability, coherence, traceability)
- Use-case driven design
- Multi-objective optimization approach

**[doc_maintenance.md](doc_maintenance.md)**
- When and how to update documentation
- Preventing documentation drift
- Documentation proximity principle
- Integration with implementation workflows

### Templates

**[skill_template.md](skill_template.md)**
- Template for creating skills
- Standard skill structure with frontmatter
- Goal, Intent, Scope pattern
- Preconditions and Postconditions

---

## When to Use Each Protocol

### During Primitive/Workflow Creation

- **create_primitive.md** — When creating a new primitive
- **create_workflow.md** — When creating a new workflow
- **skill_template.md** — Template for skill structure

### During Workflow Execution

- **tracking.md** — REQUIRED for all workflows (maintain trace)
- **recovery.md** — REQUIRED for all workflows (resume from interruption)
- **checklist_management.md** — For workflows with dynamic checklists
- **manage_complexity_uncertainty_risk.md** — For adaptive workflows facing complexity/uncertainty/risk

### During Implementation

- **doc_maintenance.md** — When code changes affect documentation
- **modularity.md** — When designing or refactoring for modularity
- **project_quality.md** — When assessing implementation quality

### During Design

- **modularity.md** — When assessing design modularity
- **project_quality.md** — When evaluating design quality across dimensions
- **manage_complexity_uncertainty_risk.md** — When mitigating complexity/uncertainty/risk
- **goals_and_objectives.md** — When defining OKRs for features/plans

### During Skill/Agent Creation

- **skills_and_agents.md** — When deciding between skills and agents
- **skills_and_agents.md** — When structuring SKILL.md files
- **goals_and_objectives.md** — When defining skill OKRs

### During Code Review

- **project_quality.md** — When reviewing artifacts across quality dimensions
- **modularity.md** — When critiquing code structure
- **doc_maintenance.md** — When verifying documentation is updated

---

## Protocol Relationships

```
create_primitive.md ──┐
                      ├──> Define structure
create_workflow.md ───┘

tracking.md ──┐
              ├──> Enable execution
recovery.md ──┘

checklist_management.md ──> Support tracking

manage_complexity_uncertainty_risk.md ──> Guide adaptive workflows

goals_and_objectives.md ──> Define success criteria
skills_and_agents.md ─────> Guide skill/agent design

project_quality.md ──┐
modularity.md ───────┼──> Ensure quality
doc_maintenance.md ──┘
```

---

## Creating New Protocols

**When to create a protocol:**
- Pattern is used by multiple primitives/workflows
- Process needs standardization
- Quality concern needs systematic approach

**When NOT to create a protocol:**
- Used by only one primitive/workflow (put it there instead)
- Too specific to a use case (belongs in a skill)
- No clear process to define (needs research first)

**How to create a protocol:**
1. Use the protocol template (see below)
2. Define Goal, Intent, Scope
3. Provide clear process/guidelines
4. Include examples
5. Reference from primitives/workflows that use it

---

## Protocol Template

```markdown
# [Protocol Name]

Brief one-sentence description.

---

## Overview

High-level explanation of what this protocol is and why it exists.

---

## Purpose

**Goal:** What this protocol achieves.

**Intent:** Why this protocol exists — what problem it solves.

**Scope:** What this protocol covers and does NOT cover.

---

## [Main Content Sections]

Detailed process, guidelines, patterns, etc.

---

## Summary

Key takeaways and how to use this protocol.

**Reference this protocol:**
- Context 1
- Context 2
- Context 3
```

---

## Contributing

When adding a new protocol:
1. Use the template above
2. Add entry to this README
3. Update "When to Use" section
4. Update "Protocol Relationships" diagram
5. Reference from relevant primitives/workflows
