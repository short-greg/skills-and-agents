# Feature Definition Skill Suite Setup

Setup instructions for skills that define feature requirements (PRDs).

---

## Overview

The **Feature Definition Suite** helps transform feature ideas into lightweight, confirmable requirements documents (PRDs) that serve as input for feature planning.

**This suite includes:**
- `feature-define` - Create PRD from feature ideas (orchestrates the workflow)
- `feature-define-review` - Review and validate PRD completeness

**When to use:**
- Capturing product requirements before development
- Documenting user stories and success criteria
- Defining feature scope and boundaries
- Creating input for feature planning

**When NOT to use:**
- PRD already exists (workflow skills detect and skip this phase)
- User provides complete written requirements
- Quick prototype or experiment

**Note:** These skills are typically called by workflow skills (like `feature-workflow`), which perform state detection and skip this phase if a PRD already exists.

**Output:** Lightweight PRD with user stories, OKRs, risks, and scope

---

## Phase 0: Repository Detection

Before setting up the PRD suite, detect and validate the repository structure.

### Step 1: Detect Spec Directory

Find where PRDs and specifications are stored:

```bash
# Common locations
for dir in specs dev-docs docs/planning .docs planning docs/prd; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$SPEC_DIR" ]; then
  echo "Where should PRDs be stored? (e.g., specs/, dev-docs/)"
  # Get user input and create directory
fi
```

**Record the spec directory** for use in skill templates.

### Step 2: Detect Tool Configuration

Find the AI tool's configuration directory:

```bash
# Check for common tool configs
for dir in .claude .cursor .codex .windsurf; do
  if [ -d "$dir/skills" ]; then
    echo "Found tool config: $dir"
    TOOL_CONFIG="$dir"
    break
  fi
done
```

### Step 3: Read Project Conventions

Extract project-specific conventions:

```bash
# Read project documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - PRD naming conventions
# - Document structure preferences
# - Stakeholder approval process
# - File naming patterns (YYYY-MM-DD-feature-name-prd.md)
```

### Step 4: Check for Existing PRDs

Review existing PRDs to understand format:

```bash
# Find existing PRDs
find ${SPEC_DIR} -name "*-prd.md" -o -name "prd-*.md" 2>/dev/null

# Read 1-2 examples to extract format and style
```

**Output Phase 0 Summary:**
```markdown
## Repository Detection Summary

- **Spec directory**: specs/ [or detected location]
- **Tool config**: .claude/ [or detected location]
- **PRD naming pattern**: YYYY-MM-DD-feature-name-prd.md [or detected]
- **Existing PRDs found**: 3 [or count]
- **Format conventions**: [Detected from existing PRDs]
```

**Gate:** Do not proceed until spec directory exists and conventions are understood.

---

## Phase 1: Validate Prerequisites

Ensure all prerequisites are met for PRD creation.

### Step 1: Verify Spec Directory Exists

```bash
# Create if doesn't exist
mkdir -p ${SPEC_DIR}
```

### Step 2: Verify Tool Skills Directory Exists

```bash
# Create skills directories
mkdir -p ${TOOL_CONFIG}/skills/feature-define
mkdir -p ${TOOL_CONFIG}/skills/feature-define-review
```

### Step 3: Understand PRD Consumers

Identify who will use the PRDs:

- **Development planning skills** - Will transform PRD into dev plan
- **Stakeholders** - Product, engineering, design, business
- **Implementation teams** - Will build what's in the PRD

### Step 4: Test File Creation

Verify write access to spec directory:

```bash
# Test creating a file
touch ${SPEC_DIR}/test-prd.md && rm ${SPEC_DIR}/test-prd.md
```

**Output Phase 1 Summary:**
```markdown
## Prerequisites Validation

- [✓] Spec directory exists: ${SPEC_DIR}
- [✓] Tool skills directory exists
- [✓] File creation tested successfully
- [✓] PRD consumers identified
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Define Skills in Suite

This suite includes two skills following the naming conventions.

### Skill 1: `feature-define`

**Name:** `feature-define` (follows `{category}-{action}` pattern)
**Alternative names:** `prd-create`, `prd-draft`

**Purpose:** Create lightweight, confirmable PRD from feature ideas with user stories and OKRs.

**Phases:**
0. Intake - Capture feature idea and supporting materials
1. Background and Context - Why this feature matters
2. User Stories - Who needs what and why
3. OKRs - Outcome-oriented success criteria
4. UI Designs - Interface mockups or descriptions (if applicable)
5. Non-Technical Risks - Business, user, operational concerns
6. Scope Boundaries - Explicit out-of-scope items
7. Write PRD Document - Compile into formal document
8. Review and Confirmation - Get stakeholder sign-off

**Output:** Approved PRD document at `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`

**PRD Structure:**
1. **Title** - Clear feature name
2. **Background** - Context and motivation
3. **User Stories** - Who needs what and why
4. **OKRs** - Outcome-oriented success criteria (high-level)
5. **UI Designs** - Visual mockups or descriptions (if applicable)
6. **Non-Technical Risks** - Business, user, operational concerns
7. **Out of Scope** - What we're NOT doing (clarity boundaries)

### Skill 2: `feature-define-review`

**Name:** `feature-define-review` (follows `{category}-{action}` pattern)

**Purpose:** Review and validate existing PRD for completeness and clarity.

**Phases:**
1. Read PRD
2. Validate Structure - Check all required sections present
3. Validate User Stories - Ensure specificity and independence
4. Validate OKRs - Verify measurability and achievability
5. Identify Gaps - Missing information or ambiguities
6. Provide Feedback - Actionable improvement suggestions

**Output:** Review report with approval status or required changes

---

## Phase 3: Provide SKILL.md Templates

Complete template for the feature-define skill.

### Template: feature-define

**File:** `${TOOL_CONFIG}/skills/feature-define/SKILL.md`

```markdown
---
name: feature-define
description: Create lightweight, confirmable PRD from feature ideas with user stories and OKRs
disable-model-invocation: true
argument-hint: "[feature idea or description]"
---

# PRD Write

Create a lightweight Product Requirements Document from a feature idea.

## When to Use

Use this skill when:
- Starting a new feature
- Documenting user requirements
- Need to align stakeholders on scope
- Creating input for development planning

Do NOT use when:
- Writing technical design documents
- Creating implementation plans
- Documenting existing features

## Input

- Feature idea (text or file path)
- Optional: UI designs, mockups, wireframes
- Optional: User feedback or business drivers

## Output

PRD document at `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md` containing:
- Background and motivation
- Prioritized user stories
- Outcome-oriented OKRs
- UI/interface design
- Non-technical risks
- Scope boundaries

## Process

### Phase 0: Intake

**Purpose:** Capture the initial feature idea and any supporting materials.

**Steps:**

1. **Capture Feature Idea**
   - If argument is text, capture it
   - If argument is file path, read it
   - If no argument, ask user to describe the feature

2. **Check for UI Designs**
   Ask user:
   ```markdown
   Do you have UI designs for this feature?
   - Mockups (Figma, Sketch, images)
   - Wireframes
   - Screenshots of existing UIs to reference
   - Descriptions of desired UI
   ```

3. **Gather Supporting Context**
   - Existing features this relates to
   - User feedback or requests driving this
   - Business goals or metrics
   - Technical constraints known upfront

**Output:**
```markdown
## Feature Idea Summary
[2-3 sentence summary]

## Supporting Materials
- UI designs: [Yes/No - if yes, location]
- Related features: [List]
- Driving factors: [User feedback, business goal, etc.]
```

**Gate:** User confirms this captures the initial idea correctly.

---

### Phase 1: Background and Context

**Purpose:** Establish why this feature matters and what problem it solves.

**Steps:**

1. **Write Background Section**

   Answer these questions:
   - What problem does this solve?
   - Who experiences this problem?
   - Why is solving it important now?
   - What's the current state/workaround?
   - What's the business or user value?

2. **Keep It Concise**

   Background should be 3-5 paragraphs maximum:
   - Paragraph 1: The problem
   - Paragraph 2: Who it affects and current impact
   - Paragraph 3: Why now / business value
   - Optional Paragraph 4: Current state/workaround
   - Optional Paragraph 5: Strategic alignment

3. **Present to User**

   ```markdown
   ## Background

   [Draft background]

   **Does this capture the motivation correctly?**
   **Anything missing or incorrect?**
   ```

**Output:** Confirmed background section.

**Gate:** User approves background before proceeding.

---

### Phase 2: User Stories

**Purpose:** Define who needs what and why, from the user's perspective.

**Steps:**

1. **Identify User Personas**

   Who will use this feature?
   - Primary users (main beneficiaries)
   - Secondary users (indirect beneficiaries)
   - Admin/operator roles (if applicable)

2. **Write User Stories**

   Use format: **As a [persona], I want [capability], so that [benefit]**

   **Good User Stories:**
   ```markdown
   - As a content moderator, I want to batch-approve similar items, so that I can process backlogs faster
   - As a team lead, I want to see moderation metrics, so that I can balance workload across the team
   - As an end user, I want faster response times, so that my experience is smoother
   ```

   **Characteristics of good stories:**
   - Specific persona (not "user" or "someone")
   - Clear capability (one thing)
   - Tangible benefit (outcome, not feature)
   - Independent (doesn't require other stories to make sense)

3. **Prioritize Stories**

   Mark priority:
   - **Must Have** - Core value, feature incomplete without
   - **Should Have** - Important, but feature works without
   - **Nice to Have** - Enhancement, can defer

4. **Present to User**

   ```markdown
   ## User Stories

   ### Must Have
   1. As a [persona], I want [capability], so that [benefit]
   2. [...]

   ### Should Have
   1. [...]

   ### Nice to Have
   1. [...]

   **Do these capture the user value correctly?**
   **Any stories missing or misprioritized?**
   ```

**Output:** Prioritized user stories.

**Gate:** User approves user stories.

---

### Phase 3: OKRs (Success Criteria)

**Purpose:** Define high-level, outcome-oriented success criteria.

**Steps:**

1. **Understand OKR Principles**

   OKRs should be:
   - **Outcome-oriented** - What state will exist, not what we'll build
   - **High-level** - System-level results, not implementation details
   - **Measurable** - Can verify objectively when done
   - **Achievable** - Realistic given scope and timeline

2. **Write Objective**

   One clear objective statement:
   ```markdown
   **Objective:** [What we want to achieve - aspirational but clear]
   ```

   Examples:
   - "Enable efficient batch moderation workflows"
   - "Provide real-time collaboration on shared documents"
   - "Reduce user onboarding friction"

3. **Write Key Results**

   3-5 key results that define success:

   **Format:** KR[N]: [Measurable outcome that will be true when done]

   **Good Key Results:**
   ```markdown
   - KR1: Moderators can review and approve 10+ items at once
   - KR2: Team leads can view per-moderator metrics in dashboard
   - KR3: Average moderation time reduced by 40%
   - KR4: System handles 1000+ concurrent batch operations
   ```

   **Characteristics:**
   - Starts with outcome, not action
   - Measurable (numbers, states, capabilities)
   - Focused on "what exists after" not "what we built"
   - High-level (not "button exists" but "users can accomplish X")

   **Avoid:**
   ❌ "Implement batch approval API" (implementation detail)
   ❌ "Add settings page" (feature, not outcome)
   ❌ "Write tests" (process, not outcome)
   ❌ "Code is clean" (too vague)

   ✅ "Users can configure batch size in settings" (capability exists)
   ✅ "All batch operations have <100ms response time" (measurable outcome)
   ✅ "System gracefully handles batch failures with retry" (behavior exists)

4. **Check Against User Stories**

   Verify each "Must Have" story maps to at least one KR:
   ```markdown
   ## Mapping Check

   User Story 1: [...]
   → Validated by: KR1, KR2

   User Story 2: [...]
   → Validated by: KR3

   [All must-have stories covered?]
   ```

5. **Present to User**

   ```markdown
   ## OKRs

   **Objective:** [Aspirational goal]

   **Key Results:**
   - KR1: [Measurable outcome]
   - KR2: [Measurable outcome]
   - KR3: [Measurable outcome]
   - KR4: [Measurable outcome]

   **Do these define success clearly?**
   **Can we objectively verify when each KR is met?**
   **Anything missing from the success criteria?**
   ```

**Output:** Confirmed OKRs.

**Gate:** User approves OKRs as complete success criteria.

---

### Phase 4: UI Designs (If Applicable)

**Purpose:** Capture or describe the user interface.

**Steps:**

1. **Check for Provided Designs**

   If user provided mockups/wireframes:
   - Include links or embed images
   - Extract key UI elements
   - Note interactions/flows

2. **If No Designs Provided, Describe UI**

   For features with UI components:
   ```markdown
   ## UI Components

   ### [Component Name]
   **Type:** [Button/Form/Dashboard/Modal/etc.]
   **Location:** [Where it appears]
   **Elements:**
   - [Element 1]: [Description]
   - [Element 2]: [Description]

   **Interactions:**
   - [User action] → [System response]

   **States:**
   - Default: [Description]
   - Loading: [Description]
   - Error: [Description]
   - Success: [Description]
   ```

3. **For Non-UI Features**

   If feature has no UI (API, backend service):
   ```markdown
   ## Interface Design

   **Type:** [API/Service/Library]
   **Entry Points:**
   - [Endpoint/Function/Method]: [Description]

   **Data Formats:**
   - Input: [Description]
   - Output: [Description]
   ```

4. **Present to User**

   ```markdown
   ## UI/Interface Design

   [Designs or descriptions]

   **Does this capture the interface correctly?**
   **Any UI elements missing or incorrect?**
   ```

**Output:** UI designs or interface descriptions.

**Gate:** User confirms UI/interface design is accurate.

---

### Phase 5: Non-Technical Risks

**Purpose:** Identify business, user, and operational risks (technical risks handled in development plan).

**Steps:**

1. **Identify Risk Categories**

   Consider these areas:

   | Category | Questions to Ask |
   |----------|------------------|
   | **User Adoption** | Will users understand how to use it? Change management needed? |
   | **Business Impact** | Could this cannibalize existing revenue? Contractual implications? |
   | **Operational** | Support burden? Training required? Monitoring needs? |
   | **Legal/Compliance** | Privacy concerns? Regulatory requirements? Terms of service? |
   | **Market/Competition** | Are competitors already doing this better? Market timing? |
   | **Resources** | Do we have needed expertise? Budget constraints? |

2. **Write Risk Assessment**

   For each identified risk:
   ```markdown
   ### Risk: [Name]
   **Category:** [User/Business/Operational/Legal/Market/Resources]
   **Impact:** [High/Medium/Low]
   **Likelihood:** [High/Medium/Low]
   **Description:** [What could go wrong]
   **Mitigation:** [How we'll address it]
   ```

3. **Focus on Non-Technical**

   This section covers:
   - ✅ User adoption challenges
   - ✅ Business model impacts
   - ✅ Operational complexity
   - ✅ Legal/compliance issues
   - ✅ Market dynamics

   Development plan will cover:
   - ❌ Technical implementation risks
   - ❌ Architecture complexity
   - ❌ Integration challenges
   - ❌ Performance concerns

4. **Keep It Lightweight**

   Only document meaningful risks:
   - Don't list risks for completeness
   - Focus on risks likely to affect success
   - 3-5 risks maximum in most PRDs
   - It's okay to have zero risks if truly low-risk

5. **Present to User**

   ```markdown
   ## Non-Technical Risks

   ### High Priority
   [Risks with High impact or High likelihood]

   ### Medium Priority
   [Moderate risks]

   ### Low Priority
   [Minor risks worth noting]

   **Any risks we haven't considered?**
   **Do the mitigations seem reasonable?**
   ```

**Output:** Non-technical risk assessment.

**Gate:** User reviews and approves risk assessment.

---

### Phase 6: Scope Boundaries

**Purpose:** Clarify what's explicitly NOT in scope to prevent scope creep.

**Steps:**

1. **Identify Related Features**

   What might people expect that we're NOT doing?
   - Related features that could be added
   - Extensions or enhancements
   - Alternative approaches
   - Integration points we're not building

2. **Write Out of Scope Section**

   Be explicit:
   ```markdown
   ## Out of Scope

   **Not included in this PRD:**
   - [Feature/capability we're NOT building]
   - [Integration we're NOT doing]
   - [Extension we're deferring]

   **Why:** [Brief rationale for each]

   **Future consideration:** [Yes/No - could this be added later?]
   ```

3. **Benefits of Explicit Boundaries**
   - Prevents scope creep during development
   - Sets clear expectations
   - Identifies future opportunities
   - Speeds up confirmation (clear what we're committing to)

4. **Present to User**

   ```markdown
   ## Out of Scope

   [List of explicitly excluded items]

   **Does this correctly bound the scope?**
   **Anything that should actually be in scope?**
   **Anything in scope that should be out?**
   ```

**Output:** Clear scope boundaries.

**Gate:** User confirms scope boundaries are correct.

---

### Phase 7: Write PRD Document

**Purpose:** Compile all sections into a formal, lightweight PRD.

**Steps:**

1. **Determine File Location**

   Use project conventions:
   - Check for spec directory (specs/, dev-docs/, docs/prd/, etc.)
   - Follow naming: `YYYY-MM-DD-feature-name-prd.md`
   - If unclear, ask user

2. **Compile Full Document**

   ```markdown
   # [Feature Name] - Product Requirements Document

   **Date:** YYYY-MM-DD
   **Status:** Draft → Review → Approved
   **Author:** [Name or "AI-assisted"]

   ---

   ## Background

   [3-5 paragraph context from Phase 1]

   ---

   ## User Stories

   ### Must Have
   - As a [persona], I want [capability], so that [benefit]
   - [...]

   ### Should Have
   - [...]

   ### Nice to Have
   - [...]

   ---

   ## OKRs

   **Objective:** [Aspirational goal]

   **Key Results:**
   - KR1: [Measurable outcome]
   - KR2: [Measurable outcome]
   - KR3: [Measurable outcome]
   - KR4: [Measurable outcome]

   ---

   ## UI/Interface Design

   [Designs, mockups, or descriptions from Phase 4]

   ---

   ## Non-Technical Risks

   ### High Priority
   [Risk assessments from Phase 5]

   ### Medium Priority
   [...]

   ---

   ## Out of Scope

   [Explicit boundaries from Phase 6]

   ---

   ## Next Steps

   1. Review and approve this PRD
   2. Use development planning skill to create technical design
   3. Break into implementable tasks
   4. Begin implementation

   ---

   ## Approval

   **Stakeholders:**
   - [ ] Product: [Name]
   - [ ] Engineering: [Name]
   - [ ] Design: [Name] (if applicable)
   - [ ] Business: [Name] (if applicable)

   **Approved Date:** ___________
   ```

3. **Quality Check**

   Before finalizing:
   ```markdown
   ## PRD Quality Checklist

   - [ ] Background clearly explains motivation (3-5 paragraphs)
   - [ ] User stories cover all personas (prioritized)
   - [ ] OKRs are outcome-oriented and high-level
   - [ ] OKRs are measurable and achievable
   - [ ] All must-have stories map to KRs
   - [ ] UI/interface design is clear (if applicable)
   - [ ] Non-technical risks identified and mitigated
   - [ ] Out of scope explicitly stated
   - [ ] Document is concise and easy to review
   - [ ] No technical implementation details (saved for dev plan)
   ```

4. **Save Document**

   Write to the spec directory with proper naming.

**Output:** Complete, lightweight PRD document.

---

### Phase 8: Review and Confirmation

**Purpose:** Get stakeholder sign-off on the PRD.

**Steps:**

1. **Present Complete PRD**

   ```markdown
   ## PRD Complete

   **Document:** [path to PRD]

   **Summary:**
   - Background: [One sentence]
   - User stories: [X must-have, Y should-have, Z nice-to-have]
   - OKRs: [X key results]
   - Risks: [X identified, all mitigated]
   - UI: [Included/Described]
   - Out of scope: [X items explicitly excluded]

   **Length:** [~X pages - should be concise]

   **Next step:** Review and approve
   ```

2. **Request Review**

   Guide user through review:
   ```markdown
   ## Review Checklist

   **Background:**
   - [ ] Motivation is clear
   - [ ] Problem is well-defined
   - [ ] Business value is articulated

   **User Stories:**
   - [ ] All key personas represented
   - [ ] Stories are specific and independent
   - [ ] Priorities make sense

   **OKRs:**
   - [ ] Objective is clear and aspirational
   - [ ] Key results are measurable
   - [ ] Success criteria are complete
   - [ ] Can verify objectively when done

   **UI/Interface:**
   - [ ] Design is clear and complete
   - [ ] All key interactions covered

   **Risks:**
   - [ ] Meaningful risks identified
   - [ ] Mitigations are reasonable
   - [ ] Non-technical focus (not implementation)

   **Scope:**
   - [ ] In-scope is clear
   - [ ] Out-of-scope prevents confusion
   - [ ] Boundaries are reasonable

   **Overall:**
   - [ ] Document is concise (easy to review)
   - [ ] No technical implementation details
   - [ ] Ready to hand to development planning
   ```

3. **Confirm Development Requirements**

   Explicitly confirm what must be implemented:
   ```markdown
   ## Development Requirements Confirmation

   **Must-Have User Stories (all must be implemented):**
   1. [User Story 1] → Will be validated by KR[X]
   2. [User Story 2] → Will be validated by KR[Y]
   3. [User Story 3] → Will be validated by KR[Z]

   **Should-Have User Stories (implement if feasible):**
   1. [User Story 4]
   2. [User Story 5]

   **Goal:**
   [Restate the objective - what we're trying to achieve]

   **Success Criteria (OKRs):**
   All of these must be met for the feature to be considered complete:
   - KR1: [Outcome]
   - KR2: [Outcome]
   - KR3: [Outcome]
   - KR4: [Outcome]

   **Confirmation Question:**
   Do you confirm that:
   - All must-have stories must be implemented?
   - All OKRs must be met for completion?
   - This defines the minimum viable feature?
   ```

4. **Get Explicit Approval**

   User must explicitly approve:
   - The goal/objective
   - All must-have user stories
   - All OKRs as success criteria
   - That this defines what goes into development

5. **Incorporate Feedback**

   If changes requested:
   - Update relevant sections
   - Re-present updated version
   - Repeat until approved

6. **Mark as Approved**

   Update document status:
   ```markdown
   **Status:** Approved
   **Approved Date:** YYYY-MM-DD
   **Approvers:** [Names]

   **Confirmed for Development:**
   ✅ Goal and objective
   ✅ [X] must-have user stories
   ✅ [Y] OKRs as success criteria
   ```

**Output:** Approved PRD with confirmed development requirements ready for development planning.

---

## Completion

When all phases complete:

```markdown
## PRD Writing Complete ✅

**Document:** [path to PRD]
**Status:** Approved
**Feature:** [Name]

### PRD Contents
- **Background**: Clear motivation and context
- **User Stories**: [X] prioritized stories
- **OKRs**: 1 objective, [Y] key results
- **UI Design**: [Included/Described]
- **Risks**: [Z] non-technical risks identified
- **Scope**: Clear boundaries set

### Key Characteristics
✅ Lightweight and concise
✅ Easy to review and confirm
✅ Outcome-oriented OKRs
✅ Non-technical focus
✅ Ready for development planning

### Next Steps
1. Use development planning skill with this PRD
2. Break into parallelizable tasks
3. Create technical design
4. Begin implementation
```

---

## Best Practices

### Background Section
- **Start with the problem** - Not the solution
- **Be concise** - 3-5 paragraphs maximum
- **Focus on value** - Why does this matter?
- **Avoid technical details** - Save for dev plan

### User Stories
- **Specific personas** - "Content moderator" not "user"
- **One capability** - Don't combine multiple wants
- **Clear benefit** - The "so that" must be tangible
- **Independent** - Each story stands alone
- **Prioritize ruthlessly** - Most things are "should have" or "nice to have"

### OKRs
- **Outcome not output** - "Users can X" not "We built Y"
- **High-level** - System capabilities, not implementation
- **Measurable** - Can objectively verify
- **Achievable** - Be realistic given scope
- **Complete** - Cover all must-have stories
- **3-5 KRs** - More than 5 suggests scope is too large

### UI Designs
- **Use visuals when possible** - A picture is worth 1000 words
- **Describe interactions** - Not just static screens
- **Include states** - Default, loading, error, success
- **Note accessibility** - If relevant to feature

### Non-Technical Risks
- **Focus on business/user/operational** - Not implementation
- **Be realistic** - Don't manufacture risks
- **Provide mitigations** - Don't just list problems
- **Prioritize** - What actually threatens success?

### Scope Boundaries
- **Be explicit** - Say what we're NOT doing
- **Explain why** - Brief rationale helps
- **Prevent confusion** - Address obvious questions
- **Identify future opportunities** - What could come later?

---

## Anti-Patterns to Avoid

### ❌ Technical Implementation Details

**Bad:**
```markdown
## User Stories
- As a developer, I want to implement a Redis cache, so that queries are faster
```

**Good:**
```markdown
## User Stories
- As an end user, I want instant search results, so that I can find information quickly

## OKRs
- KR2: Search queries return in <100ms (p95)
```

### ❌ Vague Success Criteria

**Bad:**
```markdown
- KR1: Feature works well
- KR2: Users are happy
- KR3: Performance is good
```

**Good:**
```markdown
- KR1: Users can batch process 100+ items with one action
- KR2: 80% of beta users report time savings in survey
- KR3: API response time <200ms for batch operations (p95)
```

### ❌ Implementation in User Stories

**Bad:**
```markdown
- As a developer, I want to create a new API endpoint
- As a team, we need to write integration tests
```

**Good:**
```markdown
- As a mobile app user, I want to sync data in the background, so that my data is always current
```

### ❌ Too Detailed or Too Long

**Bad:**
- 10-page PRD with architectural diagrams
- Database schemas
- API specifications
- Wire-by-wire UI specifications

**Good:**
- 2-4 page concise PRD
- High-level UI mockups or descriptions
- Focus on what and why, not how

### ❌ Missing Scope Boundaries

**Bad:**
```markdown
[No out-of-scope section]
```

**Good:**
```markdown
## Out of Scope
- Mobile app integration (future phase)
- Email notifications (separate feature)
- Advanced analytics dashboard (v2 consideration)
```

---

## Example PRD Excerpt

```markdown
# Batch Moderation Feature - PRD

**Date:** 2026-02-11
**Status:** Approved

---

## Background

Our content moderation team currently processes items one at a time, leading to inefficiency when similar items accumulate in the queue. Moderators report spending 60% of their time on repetitive approval tasks for similar content.

With user-generated content increasing 3x over the past quarter, the moderation backlog has grown from 2 hours to 8+ hours. This delays legitimate content from reaching users and impacts user satisfaction scores.

Enabling batch moderation would allow moderators to review and action similar items simultaneously, reducing processing time and backlog. This aligns with our Q1 goal of improving operational efficiency by 40%.

---

## User Stories

### Must Have
- As a content moderator, I want to select multiple similar items, so that I can approve or reject them all at once
- As a content moderator, I want to see why items are grouped together, so that I can confidently batch process them
- As a team lead, I want to see batch operation metrics, so that I can measure efficiency gains

### Should Have
- As a content moderator, I want to preview all items before batch action, so that I can catch any outliers
- As an admin, I want to set batch size limits, so that we can prevent accidental mass actions

---

## OKRs

**Objective:** Enable efficient batch moderation workflows to reduce backlog processing time

**Key Results:**
- KR1: Moderators can select and action 10-100 items in a single operation
- KR2: Batch operations complete in <5 seconds for 50 items (p95)
- KR3: Team leads can view batch operation history and metrics in dashboard
- KR4: Average item processing time reduced by 50% for similar-content scenarios

---

## UI Design

### Batch Selection Interface
[Mockup image: moderation-batch-ui.png]

**Key Elements:**
- Checkbox for each item in queue
- "Select Similar" button (AI-suggested grouping)
- Bulk action toolbar (Approve All, Reject All, Skip)
- Item count indicator

**Interactions:**
- Click checkbox → Item selected
- Click "Select Similar" → System auto-selects similar items
- Click "Approve All" → Confirmation modal → Batch action executes
- Items process → Progress indicator → Success notification

---

## Non-Technical Risks

### High Priority

**Risk: Moderator Error with Batch Actions**
- **Category:** Operational
- **Impact:** High (incorrect batch approval could release violating content)
- **Likelihood:** Medium
- **Mitigation:**
  - Require confirmation modal with item count
  - Limit batch size to 100 items maximum
  - Provide undo feature (5-minute window)
  - Log all batch actions for audit

### Medium Priority

**Risk: User Adoption**
- **Category:** User Adoption
- **Impact:** Medium (feature doesn't provide value if not used)
- **Likelihood:** Low
- **Mitigation:**
  - Provide training session for moderation team
  - Create quick-start guide
  - Monitor adoption metrics first 2 weeks

---

## Out of Scope

**Not included in this PRD:**
- AI-powered similarity detection (using simple rule-based for MVP)
- Mobile moderation interface (desktop web only)
- Batch editing (only approve/reject, not modify)
- Integration with external moderation tools

**Why:** Limiting to MVP scope to ship faster. These can be considered for v2 based on user feedback.

---

## Next Steps

1. ✅ PRD approved
2. → Create development plan (break into tasks, technical design)
3. → Implementation
4. → Testing and rollout
```

---

```
<!-- End of feature-define SKILL.md template -->

---

### Template: feature-define-review

**File:** `${TOOL_CONFIG}/skills/feature-define-review/SKILL.md`

```markdown
---
name: feature-define-review
description: Review and validate existing PRD for completeness and clarity
disable-model-invocation: true
argument-hint: "[path-to-prd-file]"
---

# PRD Review

Review an existing PRD for completeness, clarity, and quality.

## When to Use

Use this skill when:
- Reviewing a draft PRD before approval
- Validating PRD completeness
- Providing feedback on PRD quality
- Ensuring PRD meets standards

## Input

Path to PRD file (e.g., `${SPEC_DIR}/2026-02-12-feature-name-prd.md`)

## Output

Review report with:
- Validation results (structure, user stories, OKRs)
- Identified gaps or ambiguities
- Actionable feedback
- Approval status (Approved / Needs Changes)

## Process

### Phase 1: Read PRD

Read the PRD file and extract all sections.

### Phase 2: Validate Structure

Check for required sections:
- [ ] Title
- [ ] Background (3-5 paragraphs)
- [ ] User Stories (prioritized: Must Have, Should Have, Nice to Have)
- [ ] OKRs (1 objective, 3-5 key results)
- [ ] UI/Interface Design (if applicable)
- [ ] Non-Technical Risks
- [ ] Out of Scope

### Phase 3: Validate User Stories

For each user story, check:
- [ ] Follows format: "As a [persona], I want [capability], so that [benefit]"
- [ ] Specific persona (not generic "user")
- [ ] Single capability (not multiple wants)
- [ ] Tangible benefit
- [ ] Independent (stands alone)
- [ ] Properly prioritized

Common issues:
- ❌ "As a user..." (too generic)
- ❌ "I want X and Y..." (multiple capabilities)
- ❌ "...so that the system..." (not a user benefit)

### Phase 4: Validate OKRs

Check objective:
- [ ] Clear and aspirational
- [ ] Outcome-oriented (not output-oriented)

For each key result, check:
- [ ] Measurable (can verify objectively)
- [ ] Outcome-oriented (what exists after, not what we build)
- [ ] High-level (system capabilities, not implementation)
- [ ] Achievable given scope
- [ ] Maps to at least one must-have user story

Common issues:
- ❌ "Implement caching layer" (implementation detail)
- ❌ "Users are happy" (not measurable)
- ❌ "Feature works well" (vague)

### Phase 5: Identify Gaps

Check for missing information:
- Ambiguous requirements
- Undefined terms or acronyms
- Missing acceptance criteria
- Unclear scope boundaries
- Unaddressed risks
- Missing stakeholders

### Phase 6: Provide Feedback

Generate review report:

```markdown
# PRD Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** ✅ Approved / ⚠️ Needs Changes

## Structure Validation
- [✓/✗] All required sections present
- [✓/✗] Appropriate length (concise, 2-4 pages)

## User Stories Validation
- [✓/✗] All stories follow correct format
- [✓/✗] Specific personas identified
- [✓/✗] Properly prioritized
- **Issues:** [List any issues]

## OKRs Validation
- [✓/✗] Objective is clear and aspirational
- [✓/✗] Key results are measurable
- [✓/✗] All must-have stories map to KRs
- **Issues:** [List any issues]

## Gaps Identified
1. [Gap 1]
2. [Gap 2]

## Recommendations
1. [Recommendation 1]
2. [Recommendation 2]

## Approval Decision
- [ ] Approved - Ready for development planning
- [ ] Needs Minor Changes - Address feedback and re-submit
- [ ] Needs Major Revision - Significant rework required
```
```
<!-- End of feature-define-review SKILL.md template -->

---

## Phase 4: Integration Instructions

How the PRD skills integrate with other suites.

### Integration with Development Planning

PRDs created by `feature-define` become input for the development planning suite:

**PRD provides:**
- Background → Informs design decisions
- User Stories → Map to features and tasks
- OKRs → Become acceptance criteria
- UI Designs → Guide implementation
- Non-Technical Risks → Complement technical risk analysis
- Scope Boundaries → Prevent feature creep

**Development planner consumes:**
```bash
# Read PRD
plan-dev ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md

# Planner will:
# - Break PRD into parallelizable tasks
# - Define task dependencies
# - Create technical designs
# - Add technical risk assessment
# - Define system-level OKRs for the plan
```

### Integration with Orchestration

For large PRDs that decompose into parallel tasks:

```bash
# 1. Create PRD
feature-define "Feature idea..."

# 2. Review PRD
feature-define-review ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md

# 3. Orchestrate parallel development
orchestrate-parallel plan ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md
```

### Standalone Usage

PRDs can also be used independently:
- Stakeholder alignment before development starts
- Product documentation
- Feature planning and roadmapping
- User research and validation

---

## Phase 5: Testing & Validation

Test the PRD skills with various scenarios.

### Test 1: Simple Feature PRD

Create a PRD for a simple, well-defined feature:

```bash
feature-define "Add a search bar to the dashboard that filters items by name"
```

**Expected:**
- Clear background (why search is needed)
- Simple user stories (1-2 must-have)
- Measurable OKRs (search works, performance targets)
- Minimal risks
- Clear scope

**Success criteria:**
- PRD is 2-3 pages
- All sections present
- Can be reviewed and approved in <15 minutes

### Test 2: Complex Feature PRD

Create a PRD for a feature with multiple personas and complex interactions:

```bash
feature-define "Build a content moderation system with batch operations, approval workflows, and analytics"
```

**Expected:**
- Rich background explaining the problem
- Multiple user stories (moderators, team leads, admins)
- Comprehensive OKRs (functionality, performance, usability)
- Identified risks (operational, user adoption)
- Clear scope boundaries

**Success criteria:**
- PRD is 3-5 pages
- Multiple personas represented
- OKRs cover all must-have stories

### Test 3: PRD Review

Review a draft PRD with intentional issues:

```bash
feature-define-review ${SPEC_DIR}/draft-with-issues-prd.md
```

**Expected issues to catch:**
- Generic user stories ("As a user...")
- Vague OKRs ("Feature works well")
- Missing sections
- Ambiguous requirements

**Success criteria:**
- All issues identified
- Actionable feedback provided
- Clear approval decision

### Test 4: End-to-End Workflow

Test complete workflow from idea to development planning:

```bash
# 1. Create PRD
feature-define "Add user authentication with OAuth"

# 2. Review PRD
feature-define-review ${SPEC_DIR}/YYYY-MM-DD-auth-prd.md

# 3. Make revisions based on feedback

# 4. Final approval

# 5. Hand off to dev planning
plan-dev ${SPEC_DIR}/YYYY-MM-DD-auth-prd.md
```

**Success criteria:**
- PRD approved on first or second iteration
- Development planning skill can consume PRD
- No missing information causes planning blockers

---

## Troubleshooting

### Issue: PRD too long

**Symptoms:** PRD exceeds 5 pages, takes too long to review

**Solutions:**
- Focus background on problem, not solution details
- Limit user stories to must-have and critical should-haves
- Keep UI designs high-level (not pixel-perfect specs)
- Move technical details to development planning phase

### Issue: Vague OKRs

**Symptoms:** OKRs that can't be objectively verified

**Solutions:**
- Add specific metrics (numbers, percentages, timeframes)
- Focus on "what will be true" not "what we'll build"
- Make each KR testable (can write a test for it)

Example fix:
- ❌ Before: "Users can search efficiently"
- ✅ After: "Search returns results in <100ms for 95% of queries"

### Issue: Missing scope boundaries

**Symptoms:** Scope creep during development, unclear what's in/out

**Solutions:**
- Explicitly list what's NOT included
- Call out features that could be added but won't be (with reasoning)
- Define MVP vs future iterations

### Issue: User stories are implementation-focused

**Symptoms:** Stories describe technical solutions, not user needs

**Solutions:**
- Reframe from user perspective
- Focus on benefit, not mechanism
- Ask "what problem does this solve for the user?"

Example fix:
- ❌ Before: "As a developer, I want to implement a Redis cache"
- ✅ After: "As an end user, I want instant search results, so that I can find information quickly"

---

## Customization

### Adapting to Your Project

1. **Spec directory**: Replace `${SPEC_DIR}` with your actual directory
2. **File naming**: Adjust pattern to match project conventions
3. **Stakeholder sections**: Add project-specific approval sections
4. **PRD template**: Customize sections based on organizational needs
5. **Review criteria**: Adapt checklist to company standards

### Adding Custom Sections

Common additions to PRD structure:
```markdown
## Technical Constraints
[Known technical limitations or requirements]

## Success Metrics
[How we'll measure success post-launch]

## Rollout Plan
[Phased rollout strategy]

## Dependencies
[External dependencies - other teams, services, etc.]
```

---

## Summary

**This suite provides:**
- ✅ Lightweight, confirmable PRD creation
- ✅ Outcome-oriented OKRs
- ✅ Prioritized user stories
- ✅ Clear scope boundaries
- ✅ PRD review and validation

**Key skills:**
- `feature-define` - Create PRD from feature ideas
- `feature-define-review` - Validate PRD quality

**Prerequisites met:**
- Spec directory identified and created
- Tool skills directory configured
- PRD naming convention established
- File creation tested

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths
3. Test with a simple feature idea
4. Use for product requirements documentation!

---

**Last Updated:** 2026-02-12
**Related:**
- [Skills and Agents Guidelines](skills-and-agents-setup-guidelines-and-conventions.md)
- [Dev Plan Skill Suite](dev-plan-skill-suite-setup.md) - Consumes PRDs
