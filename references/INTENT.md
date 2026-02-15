# References Folder Intent

## Purpose

This folder contains **reference documents** that prevent duplication within the skills-and-agents repository.

Instead of repeating the same guidance in every skill guideline (feature-development.md, bugfixing.md, refactoring.md, etc.), we extract common patterns into references that all skills can reference.

---

## What Goes Here

**Reference documents for common patterns:**
- Assessment criteria (how to evaluate code quality)
- Goal/OKR setting (how to define measurable objectives)
- Checklist patterns (how to structure and manage checklists)
- Documentation practices (how to maintain specs/plans)

**NOT in this folder:**
- Skill templates (those go in skill-guidelines/)
- Agent configurations (those go in agent-guidelines/)
- Setup instructions (those go in SETUP.md)

---

## Current References

### [assessment-approaches.md](assessment-approaches.md)

**What:** Comprehensive assessment types for evaluating code quality

**Includes:**
1. Modularity/Maintainability (8 criteria + check for existing implementations)
2. Convention/Best Practices
3. Accuracy/Correctness
4. Non-Functional Requirements (Security, Performance)
5. Completeness

**Referenced by:** All implementation skills (feature-impl, bugfix-impl, refactor-impl)

**Why it's here:** Every skill needs to assess quality, but the criteria shouldn't be duplicated in every skill document.

---

### [goals-and-objectives.md](goals-and-objectives.md)

**What:** How to set clear goals and measurable OKRs (Objectives and Key Results)

**Includes:**
- Good vs bad examples
- Outcome-oriented vs implementation-focused
- Context-specific examples (PRD, plan, implementation)
- Anti-patterns to avoid

**Referenced by:** All planning and implementation skills

**Why it's here:** OKR guidance is the same across all skills - write once, reference everywhere.

---

### [checklist-management.md](checklist-management.md)

**What:** Standard format and rules for checklists

**Includes:**
- Numbered items with conditions
- Conditional execution (If production repo, If tests fail, etc.)
- Loop-back references (return to step N)
- Skip transparency
- Required items (progress updates)

**Referenced by:** All skills (every skill has checklists)

**Why it's here:** Checklist format should be consistent across all skills.

---

### [spec-maintenance.md](spec-maintenance.md)

**What:** How to keep spec/plan documents updated during work

**Includes:**
- What to document (progress, design changes, challenges, solutions, next steps)
- When to update (after each phase, not at end)
- Format examples
- Why it matters

**Referenced by:** All implementation and planning skills

**Why it's here:** Every skill checklist must include a progress update item - this defines what that means.

---

## How Skills Reference These

**Example from a skill guideline:**

```markdown
## Goal and OKRs

[See references/goals-and-objectives.md for OKR guidance]

**Goal:** Implement the feature defined in the PRD

**OKRs:**
- Objective: Deliver a fully functional, high-quality feature
- Key Results:
  1. All PRD requirements implemented (100%)
  2. Test coverage ≥85%
  3. Zero critical bugs
  4. Code review approved

---

## Phase 3: Design Scratchpad

[See references/checklist-management.md for checklist patterns]

**Checklist:**
- [ ] 1. Design module structure
- [ ] 2. (If production repo) Run modularity assessment
- [ ] 3. Always: Update ${SPEC_DIR}/plan.md with design decisions

[See references/spec-maintenance.md for what to document]

---

## Phase 7: Assess Quality

[See references/assessment-approaches.md for assessment criteria]

**Run assessments:**
- Modularity/Maintainability
- Conventions/Best Practices
- Accuracy/Correctness
- (If production repo) Security
- (If production repo) Performance
```

---

## Benefits

### Prevents Duplication

**Problem:** Without references, every skill would need to:
- Explain the 8 modularity criteria
- Define OKR structure with examples
- Document checklist format rules
- Describe progress logging format

**Solution:** Write once in references/, link from all skills.

### Easier Maintenance

**Problem:** If assessment criteria change, need to update 5+ skill documents.

**Solution:** Update once in references/assessment-approaches.md, all skills automatically reference latest version.

### Cleaner Skills

**Problem:** Skills become long and hard to navigate with repeated guidance.

**Solution:** Skills focus on workflow/phases, reference docs provide detailed guidance.

---

## Adding New References

**When to create a new reference:**
- Pattern is used by 3+ skills
- Content would be duplicated
- Guidance is framework-level (not skill-specific)
- Makes skills cleaner and more focused

**When NOT to create a reference:**
- Skill-specific workflow steps
- Context that's unique to one skill
- Content used by only 1-2 skills

**Template for new references:**
```markdown
# [Reference Name]

[One sentence describing purpose]

---

## Overview

[Why this exists, what problem it solves]

---

## [Main Content Sections]

[Detailed guidance with examples]

---

## Summary

[Key takeaways, how skills should reference this]
```

---

## Summary

**This folder exists to:**
- Prevent duplication within the skills-and-agents repository
- Provide common patterns all skills can reference
- Make skills cleaner and more maintainable
- Ensure consistency across the framework

**Not for:**
- User repository documentation (that's CLAUDE.md, CONTRIBUTING.md, etc.)
- Skill templates (that's skill-guidelines/)
- Setup instructions (that's SETUP.md)
