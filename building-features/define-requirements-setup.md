# Define Requirements Setup

Setup instructions for `feature-define` and `feature-define-review` skills - creating PRDs from feature ideas.

---

## Overview

The **Feature Definition** skills transform feature ideas into lightweight, confirmable Product Requirements Documents (PRDs).

**Skills in this guide:**
- `feature-define` - Create PRD from feature ideas
- `feature-define-review` - Review and validate PRD completeness

**What they produce:**
- PRD with background, user stories, OKRs, UI designs, risks, and scope

**When to use:**
- Starting a new feature
- Documenting user requirements
- Aligning stakeholders on scope
- Creating input for development planning

**When NOT to use:**
- PRD already exists (workflow skills detect and skip)
- Writing technical design (use feature-plan instead)
- Quick prototype/experiment

---

## Phase 0: Repository Detection

Follow the standard repository detection process from [../shared/repo-detection.md](../shared/repo-detection.md).

**Suite-specific checks:**
- Find where PRDs are stored (specs/, dev-docs/, etc.)
- Review existing PRD formats
- Understand approval process

---

## Phase 1: Validate Prerequisites

Follow the standard prerequisites validation from [../shared/prerequisites.md](../shared/prerequisites.md).

**Skill directories to create:**
- `${TOOL_CONFIG}/skills/feature-define/`
- `${TOOL_CONFIG}/skills/feature-define-review/`

---

## Phase 2: Define Skills

### Skill 1: `feature-define`

**Purpose:** Create lightweight, confirmable PRD from feature ideas with user stories and OKRs.

**Phases:**
0. Intake - Capture feature idea and supporting materials
1. Background - Why this feature matters
2. User Stories - Who needs what and why
3. OKRs - Outcome-oriented success criteria
4. UI Designs - Interface descriptions (if applicable)
5. Non-Technical Risks - Business, user, operational concerns
6. Scope Boundaries - Explicit out-of-scope items
7. Write PRD - Compile into formal document
8. Review - Get stakeholder sign-off

**Output:** `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`

### Skill 2: `feature-define-review`

**Purpose:** Review and validate existing PRD for completeness and clarity.

**Phases:**
1. Read PRD
2. Validate Structure - Check all sections present
3. Validate User Stories - Ensure specificity
4. Validate OKRs - Verify measurability
5. Identify Gaps - Missing information
6. Provide Feedback - Actionable suggestions

**Output:** Review report with approval status or required changes

---

## Phase 3: SKILL.md Templates

### Template: feature-define

**File:** `${TOOL_CONFIG}/skills/feature-define/SKILL.md`

```markdown
---
name: feature-define
description: Create lightweight, confirmable PRD from feature ideas with user stories and OKRs
disable-model-invocation: true
argument-hint: "[feature idea or description]"
---

# Feature Define

Create a lightweight Product Requirements Document from a feature idea.

## When to Use

Use when:
- Starting a new feature
- Documenting user requirements
- Creating input for development planning

Do NOT use when:
- Writing technical design (use feature-plan)
- PRD already exists

## Input

- Feature idea (text or file path)
- Optional: UI designs, mockups
- Optional: User feedback or business drivers

## Output

PRD at `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md` containing:
- Background and motivation
- Prioritized user stories
- Outcome-oriented OKRs
- UI/interface design
- Non-technical risks
- Scope boundaries

## Process

### Phase 0: Intake

**Purpose:** Capture the initial feature idea and supporting materials.

1. **Capture Feature Idea**
   - If argument is text, capture it
   - If argument is file path, read it
   - If no argument, ask user to describe

2. **Check for UI Designs**
   - Mockups (Figma, Sketch, images)
   - Wireframes
   - Screenshots to reference

3. **Gather Context**
   - Related features
   - User feedback driving this
   - Business goals

**Gate:** User confirms this captures the idea correctly.

---

### Phase 1: Background and Context

**Purpose:** Establish why this feature matters.

Write background answering:
- What problem does this solve?
- Who experiences this problem?
- Why is solving it important now?
- What's the current workaround?

Keep to 3-5 paragraphs maximum.

**Gate:** User approves background.

---

### Phase 2: User Stories

**Purpose:** Define who needs what and why.

1. **Identify Personas**
   - Primary users (main beneficiaries)
   - Secondary users
   - Admin/operator roles

2. **Write Stories**
   Format: **As a [persona], I want [capability], so that [benefit]**

   Good characteristics:
   - Specific persona (not "user")
   - Clear capability (one thing)
   - Tangible benefit (outcome, not feature)
   - Independent

3. **Prioritize**
   - **Must Have** - Core value
   - **Should Have** - Important but not critical
   - **Nice to Have** - Can defer

**Gate:** User approves user stories.

---

### Phase 3: OKRs (Success Criteria)

**Purpose:** Define outcome-oriented success criteria.

1. **Write Objective**
   One clear statement: "Enable [aspirational goal]"

2. **Write Key Results**
   3-5 measurable outcomes:
   - KR1: [What will be true when done]
   - KR2: [Measurable capability]
   - KR3: [Performance target]

   Good KRs are:
   - Outcome-oriented (what exists, not what we build)
   - High-level (system capabilities)
   - Measurable (can verify objectively)

   Avoid:
   - "Implement X" (implementation detail)
   - "Add settings page" (feature, not outcome)
   - "Write tests" (process, not outcome)

3. **Map to User Stories**
   Verify each "Must Have" story maps to at least one KR.

**Gate:** User approves OKRs.

---

### Phase 4: UI Designs

**Purpose:** Capture or describe the user interface.

If UI mockups provided:
- Include links or embed images
- Extract key UI elements
- Note interactions/flows

If no designs, describe:
- Component type (button/form/dashboard)
- Location in app
- Elements and interactions
- States (default, loading, error, success)

For non-UI features (API/backend):
- Entry points
- Data formats

**Gate:** User approves UI description.

---

### Phase 5: Non-Technical Risks

**Purpose:** Identify business, user, and operational risks.

Categories:
- **User Risks:** Adoption, confusion, workflow disruption
- **Business Risks:** Revenue, compliance, reputation
- **Operational Risks:** Support burden, maintenance
- **Timeline Risks:** Dependencies, unknowns

For each risk:
- Description
- Likelihood (Low/Medium/High)
- Impact (Low/Medium/High)
- Mitigation strategy

**Gate:** User acknowledges risks.

---

### Phase 6: Scope Boundaries

**Purpose:** Define explicit out-of-scope items.

1. **List What's NOT Included**
   - Features explicitly excluded
   - User types not supported
   - Edge cases deferred
   - Integrations not included

2. **Explain Why**
   Brief rationale for each exclusion.

**Gate:** User confirms scope boundaries.

---

### Phase 7: Write PRD Document

**Purpose:** Compile all sections into formal document.

Create file: `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`

```markdown
# [Feature Name] PRD

**Date:** YYYY-MM-DD
**Status:** Draft

## Background
[From Phase 1]

## User Stories
[From Phase 2]

## OKRs
[From Phase 3]

## UI Design
[From Phase 4]

## Non-Technical Risks
[From Phase 5]

## Out of Scope
[From Phase 6]
```

---

### Phase 8: Review and Confirmation

**Purpose:** Get stakeholder sign-off.

Present complete PRD and ask:
- "Does this capture the feature correctly?"
- "Any gaps or concerns?"
- "Ready to approve for planning?"

If changes needed:
- Update relevant sections
- Return to this phase

If approved:
- Update status to "Approved"
- Ready for `/feature-plan`

---

## Best Practices

### User Stories
- Be specific about personas
- One capability per story
- Focus on outcomes, not features
- Keep stories independent

### OKRs
- Outcome-oriented, not implementation
- High-level system capabilities
- Measurable and verifiable
- Map to user stories

### Scope
- Be explicit about exclusions
- Explain rationale
- Prevents scope creep
```

---

### Template: feature-define-review

**File:** `${TOOL_CONFIG}/skills/feature-define-review/SKILL.md`

```markdown
---
name: feature-define-review
description: Review and validate PRD for completeness and clarity
disable-model-invocation: true
argument-hint: "[path-to-prd-file]"
---

# Feature Define Review

Review a PRD for completeness, clarity, and quality.

## When to Use

- Reviewing a draft PRD
- Validating before planning starts
- Providing feedback on requirements

## Input

Path to PRD file (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`)

## Output

Review report with:
- Validation results
- Identified gaps
- Actionable feedback
- Approval status

## Process

### Phase 1: Read PRD

Read the PRD file and extract all sections.

### Phase 2: Validate Structure

Check all required sections present:
- [ ] Background
- [ ] User Stories
- [ ] OKRs
- [ ] UI Design (if applicable)
- [ ] Risks
- [ ] Out of Scope

### Phase 3: Validate User Stories

Check user stories are:
- [ ] Specific personas (not "user")
- [ ] Single capability each
- [ ] Outcome-focused benefits
- [ ] Independent
- [ ] Prioritized

### Phase 4: Validate OKRs

Check OKRs are:
- [ ] Outcome-oriented (not implementation)
- [ ] Measurable
- [ ] High-level
- [ ] Map to user stories

### Phase 5: Identify Gaps

Check for:
- Unresolved questions
- Vague requirements
- Missing edge cases
- Undefined behaviors

### Phase 6: Provide Feedback

Generate review report:

```markdown
# PRD Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** Approved / Needs Changes

## Structure Validation
- [x] Background present
- [x] User stories present
- [ ] Missing: UI Design section

## User Stories Validation
- [x] Specific personas
- [ ] Issue: Story 3 has two capabilities

## OKRs Validation
- [x] Outcome-oriented
- [ ] Issue: KR2 is implementation detail

## Gaps Identified
1. [Gap description]
2. [Gap description]

## Recommendations
1. [Specific recommendation]
2. [Specific recommendation]

## Approval Decision
- [ ] Approved - Ready for planning
- [x] Needs Changes - Address feedback
- [ ] Major Revision - Significant rework needed
```
```

---

## Phase 4: Integration

### How These Skills Work Together

```
feature-define
     │
     └──► Produces PRD
              │
              └──► feature-define-review
                        │
                        └──► Approved PRD
                                  │
                                  └──► Input for feature-plan
```

### Called by Workflows

`feature-workflow` calls these skills:
- Phase 1: `/feature-define`
- Phase 2: `/feature-define-review` (if changes needed)

---

## Phase 5: Testing & Validation

### Test 1: Simple Feature

Create PRD for a straightforward feature.

**Expected:**
- All phases complete quickly
- Clear, concise PRD
- Minimal review changes

### Test 2: Complex Feature

Create PRD for feature with multiple personas and UI components.

**Expected:**
- Multiple user stories across personas
- UI descriptions for each component
- Comprehensive risk assessment

### Test 3: Review with Issues

Review a PRD with intentional issues.

**Expected:**
- Issues identified correctly
- Actionable feedback provided
- Clear approval decision

---

## Summary

**Skills:** `feature-define`, `feature-define-review`

**Purpose:** Create and validate PRDs from feature ideas

**Key features:**
- Structured intake process
- User story best practices
- Outcome-oriented OKRs
- Review gates throughout
- Validation checklist

**Locations:**
- `${TOOL_CONFIG}/skills/feature-define/SKILL.md`
- `${TOOL_CONFIG}/skills/feature-define-review/SKILL.md`
