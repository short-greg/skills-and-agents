# Feature Planning Suite Setup

Setup instructions for skills that transform PRDs into actionable development plans.

---

## Overview

The **Feature Planning Suite** transforms Product Requirements Documents into comprehensive development plans with task decomposition, technical designs, and system-level OKRs.

**This suite includes:**
- `feature-plan` - Transform PRD into development plan
- `feature-plan-review` - Review and validate development plans

**When to use:**
- After PRD is approved
- Before implementation begins
- Need to break work into parallel tasks
- Creating technical design from requirements

**When NOT to use:**
- Development plan already exists (workflow skills detect and skip this phase)
- Implementing a very simple change that doesn't need planning
- Fixing a small bug (use bugfix-workflow instead)

**Note:** These skills are typically called by workflow skills (like `feature-workflow`), which perform state detection and skip this phase if a plan already exists.

**Output:** Development plan with task breakdown, dependencies, designs, and system OKRs

---

## Phase 0: Repository Detection

Before setting up the dev plan suite, detect and validate the repository structure.

### Step 1: Detect Spec Directory

Find where development plans are stored:

```bash
# Common locations
for dir in specs dev-docs docs/planning .docs planning; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$SPEC_DIR" ]; then
  echo "Where should development plans be stored? (e.g., specs/, dev-docs/)"
  # Get user input and create directory
fi
```

**Verify PRDs exist:**
```bash
# Find existing PRDs (inputs for planning)
find ${SPEC_DIR} -name "*-prd.md" 2>/dev/null
```

### Step 2: Detect Tool Configuration

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

```bash
# Read project documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - Code structure patterns
# - Testing conventions
# - Naming conventions (classes, functions, files)
# - Dependency management approach
```

### Step 4: Review Codebase Structure

```bash
# Identify project type and structure
ls -la

# Common patterns:
# - src/, lib/, app/ - source code location
# - tests/, test/ - test location
# - docs/ - documentation
# - requirements.txt, package.json - dependencies
# - framework/ - framework submodule (if applicable)
```

### Step 5: Check for Framework Documentation

If project uses a framework:

```bash
# Look for framework docs
ls framework/README.md framework/docs/ 2>/dev/null

# This informs technical design decisions
```

**Output Phase 0 Summary:**
```markdown
## Repository Detection Summary

- **Spec directory**: specs/ [or detected location]
- **Tool config**: .claude/ [or detected location]
- **Plan naming pattern**: YYYY-MM-DD-feature-name-plan.md [or detected]
- **Project type**: Python/JavaScript/etc
- **Test location**: tests/ [or detected]
- **Framework**: framework/ [if applicable]
- **Existing PRDs**: 3 ready for planning [or count]
```

**Gate:** Do not proceed until spec directory exists and codebase structure is understood.

---

## Phase 1: Validate Prerequisites

Ensure all prerequisites are met for development planning.

### Step 1: Verify Spec Directory Exists

```bash
# Create if doesn't exist
mkdir -p ${SPEC_DIR}
```

### Step 2: Verify Tool Skills Directory Exists

```bash
# Create skills directories
mkdir -p ${TOOL_CONFIG}/skills/feature-plan
mkdir -p ${TOOL_CONFIG}/skills/feature-plan-review
```

### Step 3: Verify Access to PRDs

```bash
# Test reading a PRD
ls ${SPEC_DIR}/*-prd.md 2>/dev/null || echo "No PRDs found - create one with prd-write first"
```

### Step 4: Verify Access to Codebase

```bash
# Test reading project files
cat CLAUDE.md README.md 2>/dev/null
```

### Step 5: Understand Plan Consumers

Identify who will use the development plans:

- **Implementation skills** - Will execute the plan
- **Orchestration skills** - Will parallelize tasks
- **Team members** - Will review technical approach

**Output Phase 1 Summary:**
```markdown
## Prerequisites Validation

- [✓] Spec directory exists: ${SPEC_DIR}
- [✓] Tool skills directory exists
- [✓] PRDs available for planning
- [✓] Codebase readable
- [✓] Plan consumers identified
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Define Skills in Suite

This suite includes two skills following the naming conventions.

### Skill 1: `feature-plan`

**Name:** `feature-plan` (follows `{category}-{action}` pattern)
**Alternative names:** `feature-planelopment`, `plan-feature`

**Purpose:** Transform PRD into comprehensive development plan with task decomposition and system-level OKRs.

**Phases:**
0. Intake and Context - Read PRD and gather project context
1. Question Resolution - Clarify ambiguities and uncertainties
2. Risk and Complexity Analysis - Identify technical risks
3. Structural Design Options - Propose multiple design approaches
4. Task Decomposition and Dependencies - Break into parallelizable tasks
5. System-Level OKRs - Define outcome-oriented success criteria
6. Detailed Development Plan - Elaborate implementation details
7. Write Development Plan Document - Save to spec directory

**Output:** Development plan at `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`

**Plan Contents:**
- Clarified requirements
- Risk assessment and mitigation
- Multiple structural design options (with recommendation)
- Task decomposition with dependency graph
- System-level OKRs (technical perspective)
- Detailed implementation roadmap

### Skill 2: `feature-plan-review`

**Name:** `feature-plan-review` (follows `{category}-{action}` pattern)

**Purpose:** Review and validate development plan for completeness and feasibility.

**Phases:**
1. Read Plan
2. Validate Task Decomposition - Check dependencies and parallelization
3. Validate Design Options - Assess feasibility and trade-offs
4. Validate System OKRs - Verify measurability and coverage
5. Identify Gaps - Missing details or unaddressed concerns
6. Provide Feedback - Actionable improvement suggestions

**Output:** Review report with approval status or required changes

---

## Phase 3: Provide SKILL.md Templates

Complete template for the feature-plan skill.

### Template: feature-plan

**File:** `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`

```markdown
---
name: feature-plan
description: Transform PRD into comprehensive development plan with task decomposition and system OKRs
disable-model-invocation: true
argument-hint: "[path-to-prd or PRD description]"
---

# Plan Dev

Transform a Product Requirements Document into an actionable development plan.

## When to Use

Use this skill when:
- Have an approved PRD
- Need to create technical design
- Breaking work into parallel tasks
- Before implementation begins

Do NOT use when:
- Creating the PRD (use prd-write)
- Already have a development plan
- Implementing (use impl-feature)

## Input

- PRD file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`)
- OR PRD description text

## Output

Development plan at `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md` containing:
- Clarified requirements
- Risk assessment
- Design options with recommendation
- Task breakdown with dependencies
- System-level OKRs
- Implementation roadmap

## Process

### Phase 0: Intake and Context

**Purpose:** Understand the PRD and gather project context.

**Steps:**
1. **Read the PRD**
   - If argument is a file path, read it
   - If argument is text, capture it
   - If no argument, ask user to provide PRD

2. **Gather Project Context**
   - Read project conventions (CLAUDE.md, README.md)
   - Review similar features in the codebase
   - Check framework documentation if applicable
   - Identify relevant patterns and utilities

3. **Initial Understanding Check**
   - Summarize the PRD in 2-3 sentences
   - List the key requirements identified
   - Note any immediate questions or concerns
   - Get user confirmation that understanding is correct

**Output:**
```markdown
## PRD Summary
[2-3 sentence summary]

## Key Requirements
1. [Requirement 1]
2. [Requirement 2]
...

## Initial Observations
- [Pattern found in existing code]
- [Relevant framework feature]
- [Potential concern]
```

**Gate:** Do not proceed until user confirms understanding is correct.

---

### Phase 1: Question Resolution

**Purpose:** Identify and resolve all ambiguities and uncertainties in requirements.

**Steps:**

1. **Identify Questions**

   Review the PRD for ambiguities in these categories:

   | Category | Questions to Ask |
   |----------|------------------|
   | **Scope** | What's in vs. out of scope? Edge cases? |
   | **Requirements** | What's a must-have vs. nice-to-have? |
   | **Behavior** | What should happen when X? Error states? |
   | **Integration** | How does this interact with existing systems? |
   | **Constraints** | Performance requirements? Compatibility needs? |
   | **User Experience** | UI/UX expectations? Feedback mechanisms? |

2. **Check for Better Designs**

   Even if requirements seem clear, ask:
   - Is there a simpler approach that meets the same goals?
   - Are there existing patterns in the codebase we should follow?
   - Would a different architecture be more maintainable?
   - Are we duplicating existing functionality?

3. **Present Questions to User**

   Group questions by category and priority:

   ```markdown
   ## Questions for Resolution

   ### Critical (blocking design)
   - Q1: [question]
   - Q2: [question]

   ### Important (affects design options)
   - Q3: [question]
   - Q4: [question]

   ### Nice to clarify (minor impact)
   - Q5: [question]
   ```

4. **Document Answers**

   Update the PRD understanding with clarified requirements

**Output:**
```markdown
## Clarified Requirements
[Updated requirements incorporating answers]

## Resolved Ambiguities
- Q: [question] → A: [answer]
- Q: [question] → A: [answer]

## Design Implications
- [How answers affect design choices]
```

**Gate:** Do not proceed until all critical questions are answered.

---

### Phase 2: Risk and Complexity Analysis

**Purpose:** Identify technical risks, uncertainties, and complexity factors. Develop mitigation strategies.

**Steps:**

1. **Identify Risks**

   | Risk Category | Assessment Questions |
   |---------------|---------------------|
   | **Technical Uncertainty** | Are we using unfamiliar technologies? New patterns? |
   | **Integration Risk** | Multiple systems to integrate? External APIs? |
   | **Performance Risk** | Scalability concerns? Latency requirements? |
   | **Complexity Risk** | Complex algorithms? State management? |
   | **Dependency Risk** | Third-party dependencies? Framework limitations? |

2. **Assess Complexity**

   For each major component, rate:
   - **Simple**: Straightforward, well-understood patterns
   - **Moderate**: Some complexity, established patterns exist
   - **Complex**: Novel approach, multiple unknowns, high interaction

3. **Research Mitigation Strategies**

   For each identified risk:
   - **Research**: Use web search if needed to find solutions
   - **Prototype**: Consider if a spike/prototype is needed
   - **Fallback**: Identify alternative approaches
   - **Validation**: How can we validate the approach early?

4. **Document Assessment**

**Output:**
```markdown
## Risk Assessment

### High Priority Risks
| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| [Risk 1] | High | Medium | [Strategy] |
| [Risk 2] | High | Low | [Strategy] |

### Medium Priority Risks
[Similar table]

## Complexity Analysis
- **Component A**: Simple - using existing pattern X
- **Component B**: Complex - requires new state management approach
- **Component C**: Moderate - adapting pattern Y

## Recommended Spikes/Prototypes
1. [Spike description] - to validate [uncertainty]
2. [Spike description] - to test [performance concern]
```

**Gate:** User reviews and approves risk assessment and mitigation strategies.

---

### Phase 3: Structural Design Options

**Purpose:** Propose multiple structural design options with trade-offs.

**Steps:**

1. **Design File Hierarchy**

   For each design option, show:
   ```
   Option A: [Name]
   -------------------
   path/to/module.py
     class MainClass
       method_1()
       method_2()
     class HelperClass
       method_3()

   path/to/other_module.py
     function_1()
     function_2()
   ```

2. **Create Dependency Graphs**

   Show how modules and classes relate:
   ```
   Option A Dependencies:
   ----------------------
   MainClass
     └─> HelperClass
     └─> ExistingFrameworkClass

   function_1()
     └─> function_2()
     └─> third_party.utility()
   ```

3. **Design Data Flow**

   Illustrate how data moves through the system:
   ```
   Input → Validator → Processor → Storage → Output
              ↓           ↓           ↓
           Config    Transform   Logging
   ```

4. **Propose Multiple Options**

   Present 2-3 distinct design approaches:

   ```markdown
   ## Design Option 1: [Name]

   **Approach**: [Brief description]

   ### File Structure
   [File hierarchy]

   ### Dependencies
   [Dependency graph]

   ### Data Flow
   [Flow diagram]

   ### Trade-offs
   **Pros:**
   - [Advantage 1]
   - [Advantage 2]

   **Cons:**
   - [Disadvantage 1]
   - [Disadvantage 2]

   **Best for:** [Use case]

   ---

   ## Design Option 2: [Name]
   [Similar structure]
   ```

5. **Recommend an Option**

   Provide a clear recommendation with rationale:
   ```markdown
   ## Recommendation: Option 2

   **Rationale:**
   - Aligns with existing codebase patterns
   - Lower complexity for this use case
   - Easier to test and maintain
   - Risk mitigation for [specific concern]

   However, if [condition], then Option 1 would be better because [reason].
   ```

**Output:** Complete structural design document with options and recommendation.

**Gate:** User selects or requests refinement of design option.

---

### Phase 4: Task Decomposition and Dependencies

**Purpose:** Break the PRD into parallelizable implementation tasks with clear dependencies.

**Steps:**

1. **Identify Natural Task Boundaries**

   Look for tasks that:
   - Can be implemented independently
   - Have clear interfaces/contracts
   - Can be developed and tested in isolation
   - Have minimal coupling to other tasks

2. **Define Tasks**

   For each task, specify:
   ```markdown
   ### Task: [task-name]
   **Goal:** [One sentence - what this task accomplishes]
   **Description:** [2-3 sentences elaborating]
   **Scope:** [What's included in this task]
   **Estimated Complexity:** Simple | Moderate | Complex
   ```

3. **Analyze Dependencies**

   For each task, identify:
   - **Depends On:** Which tasks must complete BEFORE this one can start
   - **Blocks:** Which tasks are waiting for this one
   - **Shared Components:** Any components multiple tasks will touch

   **Dependency Rules:**
   - Tasks with **no dependencies** can start immediately in parallel
   - Tasks with dependencies must wait until prerequisites complete
   - **Aim to keep dependencies simple** - complex dependency chains slow down parallel work
   - Consider creating shared foundation tasks that multiple tasks depend on

4. **Create Dependency Graph**

   Visualize the task relationships:
   ```
   Task Dependency Graph
   =====================

   Wave 1 (No dependencies - start immediately):
   ├─ task-data-models: Create core data structures
   ├─ task-api-spec: Define API interface contracts
   └─ task-ui-components: Build reusable UI components

   Wave 2 (Depends on Wave 1):
   ├─ task-business-logic: Implement core logic
   │  └─ Depends on: task-data-models, task-api-spec
   └─ task-ui-views: Assemble views from components
      └─ Depends on: task-ui-components, task-api-spec

   Wave 3 (Depends on Wave 2):
   └─ task-integration: Wire everything together
      └─ Depends on: task-business-logic, task-ui-views
   ```

5. **Simplify Dependencies**

   Review the graph and look for:
   - **Long chains:** Can we parallelize better?
   - **Complex interdependencies:** Can we introduce interfaces to decouple?
   - **Bottlenecks:** Are too many tasks waiting on one task?

   **Simplification strategies:**
   - Define clear interfaces/contracts early (reduces coupling)
   - Create shared foundation layer (better than cross-dependencies)
   - Split complex tasks into smaller independent pieces
   - Use mocks/stubs to break circular dependencies

   Example simplification:
   ```
   BEFORE (complex):
   task-a ─┬─> task-b ─┬─> task-d
           └─> task-c ─┘

   AFTER (simple):
   task-interfaces (foundation)
      ├─> task-a
      ├─> task-b
      ├─> task-c
      └─> task-d (all can work in parallel using interfaces)
   ```

6. **Document Task Plan**

   Create structured task breakdown:
   ```markdown
   ## Task Breakdown

   ### Task: task-data-models
   - **Goal:** Define and validate core data structures
   - **Dependencies:** None
   - **Blocks:** task-business-logic, task-api-implementation
   - **Complexity:** Simple
   - **Key Deliverables:**
     - Data classes with validation
     - Unit tests for data models
     - Type definitions

   ### Task: task-api-spec
   - **Goal:** Define API interface contracts
   - **Dependencies:** None
   - **Blocks:** task-business-logic, task-ui-views
   - **Complexity:** Simple
   - **Key Deliverables:**
     - API endpoint specifications
     - Request/response schemas
     - Error codes and messages

   ### Task: task-business-logic
   - **Goal:** Implement core business logic
   - **Dependencies:** task-data-models, task-api-spec
   - **Blocks:** task-integration
   - **Complexity:** Complex
   - **Key Deliverables:**
     - Core processing functions
     - Business rules implementation
     - Unit and integration tests

   [... continue for all tasks ...]

   ## Execution Waves

   **Wave 1** (Parallel - start immediately):
   - task-data-models
   - task-api-spec
   - task-ui-components

   **Wave 2** (Parallel - after Wave 1 merges):
   - task-business-logic (needs: task-data-models, task-api-spec)
   - task-ui-views (needs: task-ui-components, task-api-spec)

   **Wave 3** (After Wave 2 merges):
   - task-integration (needs: task-business-logic, task-ui-views)
   ```

7. **Present to User**

   ```markdown
   ## Proposed Task Decomposition

   **Total Tasks:** [X]
   **Execution Waves:** [Y]
   **Parallelization:** [X] tasks in Wave 1, [Y] tasks in Wave 2, etc.

   [Show task breakdown and dependency graph]

   **Questions:**
   - Are these tasks scoped appropriately?
   - Are dependencies correct and minimal?
   - Should any tasks be combined or split?
   - Can we simplify dependencies further?
   ```

**Output:** Task decomposition with dependency graph showing parallelization opportunities.

**Gate:** User approves task breakdown and dependencies.

---

### Phase 5: System-Level OKRs

**Purpose:** Define high-level, outcome-oriented success criteria for the complete development plan.

**Steps:**

1. **Review PRD OKRs**

   The PRD contains OKRs from the product perspective. The development plan adds system-level OKRs that describe:
   - What the system will be capable of after implementation
   - The state of the codebase after completion
   - System qualities that will exist

2. **Define System-Level Objective**

   One clear objective for the technical implementation:
   ```markdown
   **Development Objective:** [What we want to achieve - technical perspective]
   ```

   Examples:
   - "Deliver a scalable batch moderation system"
   - "Create a maintainable real-time collaboration platform"
   - "Build a high-performance search infrastructure"

3. **Write System-Level Key Results**

   3-5 key results that are:
   - **Outcome-oriented:** What state exists, not what we built
   - **High-level:** System capabilities, not implementation details
   - **Measurable:** Can verify objectively
   - **Technical focus:** Complement PRD's product OKRs

   **Good System-Level KRs:**
   ```markdown
   - KR1: System processes 1000+ batch operations/sec with <100ms latency (p95)
   - KR2: All components have >80% test coverage with passing test suite
   - KR3: API can handle 10x traffic spike without degradation
   - KR4: Code follows project conventions with zero linting errors
   - KR5: System can be deployed with zero downtime
   ```

   **Characteristics:**
   - Focus on system capabilities and qualities
   - Include performance/scalability targets
   - Include quality metrics (test coverage, code standards)
   - Include operational characteristics (deployability, monitoring)
   - High-level enough to remain stable as implementation details change

   **Avoid:**
   ❌ "Implement caching layer" (implementation detail)
   ❌ "Write 50 unit tests" (output, not outcome)
   ❌ "Use React for UI" (technology choice, not outcome)
   ❌ "Database has 5 tables" (implementation detail)

   ✅ "System response time <200ms for all operations (p95)"
   ✅ "All API endpoints have automated integration tests"
   ✅ "System handles component failures gracefully with automatic recovery"
   ✅ "Monitoring provides visibility into all critical operations"

4. **Map Tasks to System OKRs**

   Show how tasks contribute to OKRs:
   ```markdown
   ## Task-to-OKR Mapping

   **KR1: System processes 1000+ batch operations/sec**
   - Validated by: task-business-logic (implements efficient processing)
   - Validated by: task-performance-testing (measures throughput)

   **KR2: All components have >80% test coverage**
   - Validated by: task-data-models (includes unit tests)
   - Validated by: task-business-logic (includes integration tests)
   - Validated by: task-integration (includes e2e tests)

   **KR3: API handles 10x traffic spike**
   - Validated by: task-api-implementation (includes rate limiting)
   - Validated by: task-load-testing (verifies scalability)

   [All KRs covered by tasks?]
   ```

5. **Check Coverage**

   Verify:
   - Every system KR maps to at least one task
   - Every task contributes to at least one KR
   - No orphaned tasks or unaddressed KRs

6. **Present to User**

   ```markdown
   ## System-Level OKRs

   **Development Objective:** [Technical goal]

   **Key Results:**
   - KR1: [System capability outcome]
   - KR2: [Quality metric outcome]
   - KR3: [Performance outcome]
   - KR4: [Operational outcome]

   **Mapping to Tasks:**
   [Show which tasks validate which KRs]

   **Questions:**
   - Do these define technical success clearly?
   - Are they high-level and outcome-oriented?
   - Can we objectively verify when each KR is met?
   - Do they complement the PRD's product OKRs?
   ```

**Output:** System-level OKRs that define development success.

**Gate:** User approves system-level OKRs.

---

### Phase 6: Detailed Development Plan

**Purpose:** Create a detailed implementation roadmap for each task from Phase 4.

**Steps:**

1. **Elaborate Each Task**

   For each task from the decomposition, provide implementation details:
   ```markdown
   ## Task Implementation Details

   ### Task: task-data-models

   **Implementation Steps:**
   1. Define base data classes
   2. Add validation rules
   3. Implement serialization/deserialization
   4. Write unit tests

   **Acceptance Criteria:**
   - All data models have type hints
   - Validation catches invalid inputs
   - 100% test coverage for models
   - Models follow project conventions

   **Estimated Effort:** Simple (1-2 days)

   ---

   ### Task: task-business-logic

   **Implementation Steps:**
   1. Implement core processing algorithm
   2. Add error handling and recovery
   3. Integrate with data models
   4. Write unit and integration tests

   **Acceptance Criteria:**
   - Processes inputs correctly per spec
   - Handles all error cases gracefully
   - Performance meets targets (>1000 ops/sec)
   - 80%+ test coverage

   **Estimated Effort:** Complex (3-5 days)

   [... continue for all tasks ...]
   ```

2. **Define Interfaces**

   Specify key function signatures and contracts:
   ```python
   # Core interfaces to implement

   class Processor:
       """Main processing component."""

       def __init__(self, config: Config):
           """Initialize with configuration."""

       async def process(self, input: Input) -> Output:
           """Process input and return output.

           Args:
               input: The input data to process

           Returns:
               Processed output

           Raises:
               ValidationError: If input is invalid
               ProcessingError: If processing fails
           """
   ```

3. **Identify External Dependencies**

   List required libraries, APIs, or services:
   ```markdown
   ## External Dependencies

   ### New Dependencies to Add
   - `library-name>=1.2.0` - for [purpose]
   - `another-lib>=2.0` - for [purpose]

   ### Existing Framework Features to Use
   - `framework.Module` - [purpose]
   - `framework.utils.helper` - [purpose]
   ```

4. **Create Test Strategy**

   Define how each component will be tested:
   ```markdown
   ## Test Strategy

   ### Unit Tests
   - Data models: test validation, edge cases
   - Business logic: test each method independently
   - Utilities: test helper functions

   ### Integration Tests
   - End-to-end workflow tests
   - Error handling scenarios
   - Integration with existing systems

   ### Manual Tests
   - UI/UX validation (if applicable)
   - Performance benchmarks
   - User acceptance criteria
   ```

5. **Define Acceptance Criteria**

   Clear, measurable success criteria:
   ```markdown
   ## Acceptance Criteria

   - [ ] All unit tests pass
   - [ ] Integration tests cover happy path and error cases
   - [ ] Performance meets requirement: [metric]
   - [ ] Code follows project conventions (CLAUDE.md)
   - [ ] Documentation updated
   - [ ] No regression in existing features
   ```

6. **Estimate Effort**

   Provide rough complexity estimates (not time):
   ```markdown
   ## Effort Estimates

   - Phase 1: Simple (straightforward implementation)
   - Phase 2: Moderate (some complexity, well-understood)
   - Phase 3: Complex (multiple integration points)

   **Overall Complexity**: Moderate to Complex
   ```

**Output:** Complete development plan document.

**Gate:** User reviews and approves plan before implementation begins.

---

### Phase 7: Write Development Plan Document

**Purpose:** Create a formal development plan document in the project's spec directory.

**Steps:**

1. **Determine File Location**

   Use project conventions:
   - Check if project uses `dev-docs/`, `specs/`, `docs/planning/`, or similar
   - Follow naming convention: `YYYY-MM-DD-feature-name-plan.md`
   - If unclear, ask user for preferred location

2. **Write Comprehensive Document**

   Include all artifacts from previous phases:

   ```markdown
   # [Feature Name] Development Plan

   Date: YYYY-MM-DD
   Status: Approved

   ## Overview
   [PRD summary and context]

   ## Requirements
   [Clarified requirements from Phase 1]

   ## System-Level OKRs
   [OKRs from Phase 5 - outcome-oriented, high-level]

   **Development Objective:** [Technical goal]

   **Key Results:**
   - KR1: [System capability outcome]
   - KR2: [Quality metric outcome]
   - KR3: [Performance outcome]
   - KR4: [Operational outcome]

   ## Risk Assessment
   [Risks and mitigation from Phase 2]

   ## Design
   [Selected design option from Phase 3]

   ### File Structure
   [Detailed hierarchy]

   ### Dependencies
   [Dependency graph]

   ### Data Flow
   [Flow diagrams]

   ## Task Decomposition
   [Task breakdown from Phase 4]

   ### Task Breakdown
   [List of all tasks with goals, dependencies, complexity]

   ### Task Dependency Graph
   [Visual representation of dependencies]

   ### Execution Waves
   - **Wave 1** (Parallel): [tasks with no dependencies]
   - **Wave 2** (Parallel): [tasks dependent on Wave 1]
   - **Wave 3** (Sequential/Parallel): [remaining tasks]

   ## Implementation Details
   [Detailed plan for each task from Phase 6]

   ## Test Strategy
   [Testing approach]

   ## Acceptance Criteria
   [Success criteria - mapped to system OKRs]

   ## Notes
   [Additional context, decisions, or considerations]
   ```

3. **Link Related Documents**

   Reference related specs, designs, or discussions

**Output:** Development plan document saved to project spec directory.

---

## Completion

When all phases are complete, provide a summary:

```markdown
## Development Plan Complete

**Document**: [path to plan file]

**Key Decisions:**
- Design approach: [selected option]
- Risk mitigation: [key strategies]
- Task decomposition: [X tasks in Y waves]
- Dependencies: [Simplified to enable parallel work]

**System-Level OKRs:**
- Objective: [Technical goal]
- Key Results: [X measurable outcomes]

**Next Steps:**
1. Review and approve the plan
2. Use orchestration skill if parallelizing tasks
3. Use implementation process skill to begin development
4. Reference this plan during implementation for design decisions

**Task Breakdown:**
- Wave 1: [X tasks can start immediately]
- Wave 2: [Y tasks after Wave 1]
- Total parallelization potential: [description]
```

---

## Best Practices

### Question Resolution
- **Ask early**: Don't wait to discover ambiguities during implementation
- **Be specific**: "What should happen when X?" not "Tell me more about X"
- **Prioritize**: Mark which questions are blocking vs. nice-to-clarify
- **Document decisions**: Record both questions and answers

### Risk Analysis
- **Be thorough**: Consider technical, integration, and operational risks
- **Research actively**: Use web search for unfamiliar technologies
- **Provide fallbacks**: Always have a backup approach
- **Quantify when possible**: "May add 50ms latency" not "might be slow"

### Design Options
- **Show trade-offs**: Every design has pros and cons
- **Be realistic**: Don't propose options you wouldn't actually use
- **Use existing patterns**: Check the codebase for established approaches
- **Consider maintainability**: Simple is better than clever

### Task Decomposition
- **Aim for parallel work**: Maximize tasks that can start immediately
- **Keep dependencies simple**: Complex chains slow down progress
- **Use interfaces to decouple**: Define contracts early to enable parallel development
- **Right-size tasks**: Not too big (can't parallelize), not too small (overhead)
- **Clear deliverables**: Each task should produce testable artifacts

### System-Level OKRs
- **Outcome-oriented**: What exists after, not what we built
- **High-level**: System capabilities, not implementation details
- **Measurable**: Can verify objectively
- **Complete**: Cover all aspects (functionality, quality, performance, operations)
- **Complement PRD OKRs**: Technical perspective on product goals

### Implementation Planning
- **Break down incrementally**: Small, testable pieces
- **Define interfaces early**: Clear contracts between components
- **Plan for testing**: Test strategy is not an afterthought
- **Be specific about success**: Clear, measurable acceptance criteria

---

## Anti-Patterns to Avoid

1. **Skipping question resolution**: Assuming you understand requirements
2. **Single design option**: Not exploring alternatives
3. **Ignoring risks**: Hoping problems won't materialize
4. **Vague acceptance criteria**: "Works well" is not measurable
5. **Over-engineering**: Solving problems that don't exist
6. **Under-researching**: Not checking for existing solutions

---

```
<!-- End of feature-plan SKILL.md template -->

---

### Template: feature-plan-review

**File:** `${TOOL_CONFIG}/skills/feature-plan-review/SKILL.md`

```markdown
---
name: feature-plan-review
description: Review and validate development plan for completeness and feasibility
disable-model-invocation: true
argument-hint: "[path-to-plan-file]"
---

# Plan Review

Review a development plan for completeness, feasibility, and quality.

## When to Use

Use this skill when:
- Reviewing a draft development plan
- Validating plan before implementation starts
- Providing technical feedback on approach
- Ensuring plan meets standards

## Input

Path to plan file (e.g., `${SPEC_DIR}/2026-02-12-feature-name-plan.md`)

## Output

Review report with:
- Validation results (tasks, design, OKRs)
- Identified gaps or issues
- Actionable feedback
- Approval status (Approved / Needs Changes)

## Process

### Phase 1: Read Plan

Read the plan file and extract all sections.

### Phase 2: Validate Task Decomposition

Check task breakdown:
- [ ] Tasks are atomic and independently testable
- [ ] Dependencies are clearly specified
- [ ] Dependency graph shows execution waves
- [ ] Independent tasks can run in parallel
- [ ] No circular dependencies
- [ ] Each task has clear deliverables

Common issues:
- ❌ Tasks too large (can't parallelize)
- ❌ Tasks too small (excessive overhead)
- ❌ Complex dependency chains (slow execution)
- ❌ Undefined dependencies

### Phase 3: Validate Design Options

For each design option:
- [ ] File structure is clear
- [ ] Dependencies are mapped
- [ ] Data flow is illustrated
- [ ] Trade-offs are documented
- [ ] Recommendation has clear rationale

Check that:
- [ ] At least 2 design options presented
- [ ] Selected design aligns with codebase patterns
- [ ] Design considers maintainability and testing

### Phase 4: Validate System OKRs

Check that system OKRs:
- [ ] Are outcome-oriented (what exists, not what we build)
- [ ] Are high-level (system capabilities, not implementation)
- [ ] Are measurable (can verify objectively)
- [ ] Cover all aspects (functionality, quality, performance, ops)
- [ ] Complement PRD's product OKRs
- [ ] Map to all tasks (no orphaned tasks or KRs)

Common issues:
- ❌ Implementation details as OKRs
- ❌ Vague or unmeasurable criteria
- ❌ Missing coverage of quality/performance
- ❌ Tasks don't contribute to any OKR

### Phase 5: Identify Gaps

Check for missing information:
- Unresolved questions from PRD
- Missing risk mitigation strategies
- Undefined interfaces between components
- Untested assumptions
- Missing acceptance criteria
- No testing strategy

### Phase 6: Provide Feedback

Generate review report:

```markdown
# Development Plan Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** ✅ Approved / ⚠️ Needs Changes

## Task Decomposition Validation
- [✓/✗] Tasks are atomic and testable
- [✓/✗] Dependencies clearly specified
- [✓/✗] Good parallelization potential
- **Issues:** [List any issues]

## Design Validation
- [✓/✗] Multiple options presented
- [✓/✗] Trade-offs documented
- [✓/✗] Recommendation is sound
- **Issues:** [List any issues]

## System OKRs Validation
- [✓/✗] Outcome-oriented and measurable
- [✓/✗] Complete coverage
- [✓/✗] All tasks map to OKRs
- **Issues:** [List any issues]

## Gaps Identified
1. [Gap 1]
2. [Gap 2]

## Recommendations
1. [Recommendation 1]
2. [Recommendation 2]

## Approval Decision
- [ ] Approved - Ready for implementation
- [ ] Needs Minor Changes - Address feedback and re-submit
- [ ] Needs Major Revision - Significant rework required
```
```
<!-- End of feature-plan-review SKILL.md template -->

---

## Phase 4: Integration Instructions

How the dev plan skills integrate with other suites.

### Integration with PRD Suite

Development plans consume PRDs:

```bash
# 1. Create PRD
prd-write "Feature idea..."

# 2. Approve PRD
prd-review ${SPEC_DIR}/YYYY-MM-DD-feature-prd.md

# 3. Create development plan
feature-plan ${SPEC_DIR}/YYYY-MM-DD-feature-prd.md
```

**PRD provides:**
- Background → Context for design decisions
- User Stories → Features to implement
- OKRs → Basis for system-level OKRs
- Scope → Boundaries for planning

### Integration with Implementation Suite

Development plans feed into implementation:

```bash
# 1. Create and approve plan
feature-plan ${SPEC_DIR}/YYYY-MM-DD-feature-prd.md
feature-plan-review ${SPEC_DIR}/YYYY-MM-DD-feature-plan.md

# 2. Implement
impl-feature ${SPEC_DIR}/YYYY-MM-DD-feature-plan.md
```

**Plan provides:**
- Design → Architecture to follow
- Tasks → What to implement
- System OKRs → Acceptance criteria
- Interfaces → Component contracts

### Integration with Orchestration Suite

For plans with parallel tasks:

```bash
# 1. Create plan with task decomposition
feature-plan ${SPEC_DIR}/YYYY-MM-DD-feature-prd.md

# 2. Orchestrate parallel implementation
orchestrate-parallel plan ${SPEC_DIR}/YYYY-MM-DD-feature-plan.md
orchestrate-parallel setup
```

**Plan provides:**
- Task breakdown → What to parallelize
- Dependencies → Execution order
- Task specs → What each worktree implements

### Standalone Usage

Plans can also be used independently:
- Architecture documentation
- Technical design reviews
- Estimation and scoping
- Team coordination

---

## Phase 5: Testing & Validation

Test the dev plan skills with various scenarios.

### Test 1: Simple Feature Plan

Create a plan for a simple, well-defined feature:

```bash
feature-plan ${SPEC_DIR}/2026-02-12-add-search-bar-prd.md
```

**Expected:**
- Few clarifying questions (PRD was clear)
- Minimal risks (straightforward implementation)
- Simple task breakdown (1-2 tasks)
- Clear design (one obvious approach)

**Success criteria:**
- Plan completed in one session
- Design decision is obvious
- Ready to implement immediately

### Test 2: Complex Feature Plan

Create a plan for a feature with multiple components:

```bash
feature-plan ${SPEC_DIR}/2026-02-12-batch-moderation-prd.md
```

**Expected:**
- Multiple questions to resolve
- Identified technical risks
- Multiple design options presented
- 4-6 tasks with some dependencies
- Comprehensive system OKRs

**Success criteria:**
- All ambiguities resolved
- Risk mitigations defined
- Tasks parallelizable
- Clear execution waves

### Test 3: Plan Review

Review a plan with intentional issues:

```bash
feature-plan-review ${SPEC_DIR}/draft-plan-with-issues.md
```

**Expected issues to catch:**
- Tasks with undefined dependencies
- Vague system OKRs ("works well")
- Missing design trade-offs
- Circular dependencies

**Success criteria:**
- All issues identified
- Actionable feedback provided
- Clear approval decision

### Test 4: End-to-End Workflow

Test complete workflow from PRD to implementation:

```bash
# 1. Create PRD
prd-write "Add OAuth authentication"

# 2. Create Plan
feature-plan ${SPEC_DIR}/YYYY-MM-DD-oauth-prd.md

# 3. Review Plan
feature-plan-review ${SPEC_DIR}/YYYY-MM-DD-oauth-plan.md

# 4. Make revisions

# 5. Implement
impl-feature ${SPEC_DIR}/YYYY-MM-DD-oauth-plan.md
```

**Success criteria:**
- Plan approved on first or second iteration
- Implementation skill can consume plan
- No blockers during implementation

---

## Troubleshooting

### Issue: Too many tasks

**Symptoms:** Task breakdown has 10+ tasks, complex dependencies

**Solutions:**
- Group related tasks together
- Create "foundation" tasks that multiple tasks depend on
- Simplify by defining clear interfaces early

### Issue: Vague task descriptions

**Symptoms:** Tasks don't have clear deliverables or acceptance criteria

**Solutions:**
- Make each task specific and atomic
- Define "done" criteria for each task
- Specify files/components to be created

### Issue: Circular dependencies

**Symptoms:** Task A depends on Task B which depends on Task A

**Solutions:**
- Break cycles by extracting shared interfaces
- Define contracts/APIs as separate foundation task
- Reorder tasks to eliminate circularity

### Issue: System OKRs are too implementation-focused

**Symptoms:** OKRs describe what you'll build, not outcomes

**Solutions:**
- Reframe from system capabilities perspective
- Focus on "what will be true after"
- Make measurable and verifiable

Example fix:
- ❌ Before: "Implement caching layer with Redis"
- ✅ After: "System response time <200ms for all operations (p95)"

---

## Customization

### Adapting to Your Project

1. **Spec directory**: Replace `${SPEC_DIR}` with your actual directory
2. **File naming**: Adjust pattern to match project conventions
3. **Design options**: Customize number based on complexity
4. **Task granularity**: Adjust based on team size and parallelization needs
5. **System OKRs**: Adapt to organizational quality standards

### Adding Custom Sections

Common additions to plan structure:
```markdown
## Database Schema Changes
[Schema migrations required]

## API Changes
[New or modified endpoints]

## Performance Benchmarks
[Specific performance targets]

## Rollback Plan
[How to revert if needed]
```

---

## Summary

**This suite provides:**
- ✅ Comprehensive development planning
- ✅ Task decomposition with dependencies
- ✅ Multiple design options with trade-offs
- ✅ System-level OKRs (technical perspective)
- ✅ Risk assessment and mitigation
- ✅ Plan review and validation

**Key skills:**
- `feature-plan` - Transform PRD into development plan
- `feature-plan-review` - Validate plan quality

**Prerequisites met:**
- Spec directory identified and created
- Tool skills directory configured
- PRDs available for planning
- Codebase structure understood

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths
3. Test with an approved PRD
4. Use for development planning!

---

**Last Updated:** 2026-02-12
**Related:**
- [Skills and Agents Guidelines](skills-and-agents-setup-guidelines-and-conventions.md)
- [PRD Skill Suite](prd-skill-suite-setup.md) - Provides input PRDs
- [Implementation Skill Suite](implementation-skill-suite-setup.md) - Consumes plans
- [Orchestration Skill Suite](orchestration-skill-suite-setup.md) - Parallelizes tasks
