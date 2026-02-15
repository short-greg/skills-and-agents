# Creating Examples - Complete Skill Suite

Consolidated setup guide for creating educational examples and tutorials: code demonstrations and step-by-step learning content.

---

## Overview

The **Creating Examples Suite** provides skills for building educational content and framework demonstrations. This guide contains setup instructions for all related skills in one place.

### Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `create-example` | Create working code examples with documentation and integration | Atomic |
| `create-tutorial` | Create step-by-step tutorial content with explanations | Atomic |

### When to Use Each Skill

**Use `create-example` when:**
- Building framework examples
- Demonstrating specific concepts or patterns
- Creating working code demonstrations
- Need runnable, self-contained examples
- Want to showcase framework features

**Use `create-tutorial` when:**
- Creating step-by-step educational content
- Teaching concepts progressively
- Building learning paths
- Need detailed explanations with code
- Want to guide learners through a process

### How Skills Work Together

Both skills create educational content but with different formats:

```
create-example
    ├─► Phase 0: Intake - Understand requirements
    ├─► Phase 1: Research - Study framework and existing examples
    ├─► Phase 2: Design - Plan structure and components
    ├─► Phase 3: Implementation - Build working code
    ├─► Phase 4: Testing - Verify example works
    ├─► Phase 5: Documentation - Create README with explanations
    ├─► Phase 6: Integration - Register with main app
    └─► Phase 7: Grievances (optional) - Document friction points

create-tutorial
    ├─► Phase 1: Define Learning Objectives
    ├─► Phase 2: Outline Tutorial Steps
    ├─► Phase 3: Write Tutorial Content
    ├─► Phase 4: Create Code Samples
    ├─► Phase 5: Add Visuals
    └─► Phase 6: Review and Test
```

**Key Differences:**
- `create-example` = Working code first, documentation explains it
- `create-tutorial` = Progressive teaching, code illustrates concepts

---

## Phase 0: Repository Detection

Follow the standard repository detection process to understand your project structure.

### Reference: setup-guidelines/repo-detection.md

The repository detection process helps customize skills to your project. Key steps:

1. **Detect Spec Directory** - Where example specifications are stored
2. **Detect Tool Configuration** - AI tool's config directory
3. **Read Project Conventions** - Example naming, structure, integration patterns
4. **Check Existing Examples** - Review format and organization
5. **Analyze Framework Structure** - Understand what's being demonstrated

### Creating Examples Suite Specific Checks

```bash
# Find examples directory
for dir in examples src/examples lib/examples docs/examples samples; do
  if [ -d "$dir" ]; then
    echo "Found examples directory: $dir"
    EXAMPLES_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$EXAMPLES_DIR" ]; then
  echo "Where should examples be stored? (e.g., examples/)"
fi

# Find spec directory for example specifications
for dir in specs dev-docs docs/planning .docs planning; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# Review existing examples to understand pattern
ls -la ${EXAMPLES_DIR}/

# Read 1-2 examples to extract structure
# Typical structure:
# examples/
# ├── example1_name/
# │   ├── __init__.py or main.py
# │   ├── README.md
# │   └── tests/ (optional)

# Check for framework directory
ls -la framework/ 2>/dev/null && echo "Framework: framework/"

# Find main application entry point
for file in app.py main.py __main__.py src/main.py; do
  if [ -f "$file" ]; then
    echo "Found main app: $file"
    MAIN_APP="$file"
    break
  fi
done

# Read project conventions
cat CLAUDE.md README.md 2>/dev/null

# Look for:
# - Example naming conventions
# - Example structure requirements
# - Registration process (how examples integrate with main app)
# - Documentation standards
```

### Detection Summary Template

```markdown
## Creating Examples Suite Detection Summary

- **Examples directory**: ${EXAMPLES_DIR}
- **Spec directory**: ${SPEC_DIR}
- **Tool config**: ${TOOL_CONFIG}
- **Framework directory**: [if applicable]
- **Main app**: ${MAIN_APP}
- **Example naming pattern**: [e.g., example{N}_{name}]
- **Registration process**: [How examples integrate]
- **Existing examples**: [count]
- **Documentation standard**: [README format, etc.]
```

**Gate:** Do not proceed until examples directory location is confirmed and conventions are understood.

---

## Phase 1: Prerequisites

Follow the standard prerequisites validation to ensure all required directories and access exist.

### Reference: setup-guidelines/prerequisites.md

Standard validation steps:

1. **Verify Spec Directory Exists** - Create if needed
2. **Verify Tool Skills Directory Exists** - Create base structure
3. **Test File Access** - Ensure read/write permissions
4. **Verify Project Documentation Access** - Can read conventions
5. **Identify Skill Consumers** - Who/what uses these skills

### Creating Examples Suite Specific Prerequisites

```bash
# Create spec directory if needed
mkdir -p ${SPEC_DIR}

# Create examples directory if needed
mkdir -p ${EXAMPLES_DIR}

# Create skill directories
mkdir -p ${TOOL_CONFIG}/skills/create-example
mkdir -p ${TOOL_CONFIG}/skills/create-tutorial

# Verify framework access (if applicable)
ls framework/README.md 2>/dev/null || echo "No framework found"

# Verify main app can register examples
grep -q "examples" ${MAIN_APP} 2>/dev/null || echo "Main app may need example registration support"

# Verify access
touch ${SPEC_DIR}/.test-access && rm ${SPEC_DIR}/.test-access
touch ${EXAMPLES_DIR}/.test-access && rm ${EXAMPLES_DIR}/.test-access
touch ${TOOL_CONFIG}/skills/.test-access && rm ${TOOL_CONFIG}/skills/.test-access
```

### Example Structure Pattern

Identify the standard example structure:

```
examples/
└── example{N}_{name}/
    ├── __init__.py          # Python: module initialization
    ├── {name}.py            # Main implementation
    ├── demo.py              # Demo/usage script (optional)
    ├── README.md            # Documentation
    ├── tests/               # Optional: example-specific tests
    │   └── test_{name}.py
    └── assets/              # Optional: images, data files
```

### Prerequisites Checklist

```markdown
## Creating Examples Suite Prerequisites

- [ ] Spec directory exists and is writable: ${SPEC_DIR}
- [ ] Examples directory exists and is writable: ${EXAMPLES_DIR}
- [ ] Skill directories created under ${TOOL_CONFIG}/skills/
- [ ] Framework accessible (if applicable)
- [ ] Main app structure understood
- [ ] Example structure pattern identified
- [ ] File access verified
- [ ] Project documentation accessible (CLAUDE.md or README.md)
```

**Gate:** Do not proceed until all directories exist and example conventions are understood.

---

## Phase 2: Skill Definitions

Complete SKILL.md templates for all skills in the creating examples suite.

---

## Skill 1: create-example

**Purpose:** Create complete framework examples with implementation, tests, documentation, and integration.

**File:** `${TOOL_CONFIG}/skills/create-example/SKILL.md`

```markdown
---
name: create-example
description: Create a framework example with implementation, tests, docs, and integration
disable-model-invocation: true
argument-hint: "[example-spec-file or example idea]"
---

# Create Example

Create a complete framework example demonstrating a specific concept or pattern.

## When to Use

Use this skill when:
- Creating a new framework example
- Demonstrating a framework feature
- Building educational content
- Documenting usage patterns

Do NOT use when:
- Implementing production features (use feature-impl)
- Creating tutorials without code (use create-tutorial)
- Fixing existing examples (use bugfix-impl)

## Input

- Example spec file (e.g., `${SPEC_DIR}/YYYY-MM-DD-example{N}-spec.md`)
- OR example idea description

## Output

Complete example at `${EXAMPLES_DIR}/example{N}_{name}/` with:
- Implementation code
- Tests (if applicable)
- README documentation
- Integration with main app
- Grievances document (optional)

## Process

### Phase 0: Intake

**Purpose:** Understand what example needs to demonstrate.

**Steps:**

1. **Read Example Spec**
   - If argument is spec file, read it
   - Extract: goal, features to demonstrate, acceptance criteria
   - If no spec, ask user to describe example

2. **Identify Next Example Number**
   ```bash
   # Find highest example number
   ls ${EXAMPLES_DIR}/ | grep -E "example[0-9]+" | sort -V | tail -1
   # Increment by 1
   ```

3. **Determine Example Name**
   - Follow project naming convention
   - Format: `example{N}_{descriptive_name}`
   - Example: `example12_behavior_tree_decorators`

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 CREATE-EXAMPLE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 0: Intake [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 1: Research
⏸️ Phase 2: Design
⏸️ Phase 3: Implementation
⏸️ Phase 4: Testing
⏸️ Phase 5: Documentation
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 0: Understanding example requirements
Status: Identifying example number and name
Started: [TIME]

CHECKLIST:
✅ Read example spec or description
🔲 Identify next example number
🔲 Determine example name
🔲 Confirm with user
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:**
```markdown
## Example Spec Summary

- **Number**: 12
- **Name**: example12_behavior_tree_decorators
- **Goal**: Demonstrate BT decorator patterns
- **Features**: Retry decorator, timeout decorator, logging decorator
- **Directory**: ${EXAMPLES_DIR}/example12_behavior_tree_decorators/
```

**Gate:** User confirms example number and name.

---

### Phase 1: Research

**Purpose:** Study framework documentation and existing examples.

**Steps:**

1. **Read Framework Documentation**
   ```bash
   # Read relevant framework docs
   cat framework/README.md
   cat framework/docs/relevant-topic.md
   ```

2. **Review Similar Examples**
   ```bash
   # Find related examples
   ls ${EXAMPLES_DIR}/ | grep {related_topic}

   # Read implementation
   cat ${EXAMPLES_DIR}/example{X}_{related}/*.py
   ```

3. **Identify Patterns to Follow**
   - Code structure patterns
   - Import conventions
   - Documentation style
   - Integration approach

4. **Note Framework APIs**
   - Classes to use
   - Methods to demonstrate
   - Common patterns

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
🔄 Phase 1: Research [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Design
⏸️ Phase 3: Implementation
⏸️ Phase 4: Testing
⏸️ Phase 5: Documentation
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 1: Researching framework and existing examples
Status: Reading framework documentation
Started: [TIME]

RESEARCH PROGRESS:
✅ Framework documentation read
🔲 Similar examples reviewed
🔲 Patterns identified
🔲 Framework APIs noted
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:**
```markdown
## Research Summary

**Relevant Framework APIs:**
- dachi.act.bt.Decorator
- dachi.act.bt.Task

**Similar Examples:**
- example3_basic_behavior_tree
- example7_advanced_bt_patterns

**Patterns Identified:**
- Each decorator is a separate class
- Examples use composition pattern
- Tests verify decorator behavior
```

**Gate:** User reviews research findings.

---

### Phase 2: Design

**Purpose:** Plan example structure and components.

**Steps:**

1. **Design File Structure**
   ```
   examples/example12_behavior_tree_decorators/
   ├── __init__.py
   ├── decorators.py          # Main implementation
   ├── demo.py                # Demo/usage script
   ├── README.md
   ├── tests/
   │   └── test_decorators.py
   └── assets/ (if needed)
   ```

2. **Plan Components**
   - What classes/functions to create
   - What framework features to use
   - How components interact

3. **Design Example Flow**
   - How will the example run?
   - What will it output/demonstrate?
   - Interactive or batch?

4. **Plan Documentation Sections**
   - What to explain in README
   - Key concepts to cover
   - Usage instructions

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
🔄 Phase 2: Design [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Implementation
⏸️ Phase 4: Testing
⏸️ Phase 5: Documentation
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 2: Designing example structure
Status: Planning components and flow
Started: [TIME]

DESIGN PROGRESS:
✅ File structure planned
✅ Components identified
🔲 Example flow designed
🔲 Documentation sections outlined
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:**
```markdown
## Example Design

**Files:**
- decorators.py - Retry, Timeout, Logging decorators
- demo.py - Demonstrates each decorator
- README.md - Explains decorator pattern

**Components:**
- RetryDecorator(max_retries=3)
- TimeoutDecorator(timeout_ms=1000)
- LoggingDecorator(log_level="INFO")

**Demo Flow:**
1. Create tasks
2. Wrap with decorators
3. Execute and show results
4. Display logs
```

**Gate:** User approves design.

---

### Phase 3: Implementation

**Purpose:** Build the example code.

**Steps:**

1. **Create Directory Structure**
   ```bash
   mkdir -p ${EXAMPLES_DIR}/example12_behavior_tree_decorators
   mkdir -p ${EXAMPLES_DIR}/example12_behavior_tree_decorators/tests
   ```

2. **Implement Main Code**
   - Follow design from Phase 2
   - Use framework APIs identified in research
   - Follow project code quality standards:
     - PascalCase classes
     - snake_case functions
     - Type hints
     - Docstrings

3. **Implement Demo/Usage Script**
   - Show how to use the components
   - Make output clear and educational
   - Include comments explaining key points

4. **Follow Import Ordering**
   ```python
   # Standard library
   import asyncio
   from typing import Optional

   # Third-party
   import numpy as np

   # Framework (if applicable)
   from framework.module import FeatureClass

   # Local example
   from .decorators import RetryDecorator
   ```

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
✅ Phase 2: Design
🔄 Phase 3: Implementation [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Testing
⏸️ Phase 5: Documentation
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 3: Implementing example code
Status: Writing main implementation
Started: [TIME]

IMPLEMENTATION PROGRESS:
✅ Directory structure created
🔄 Main code implementation (decorators.py)
⏸️ Demo script (demo.py)
⏸️ Import ordering verified
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Working implementation code.

---

### Phase 4: Testing

**Purpose:** Ensure example works correctly.

**Steps:**

1. **Write Tests (if applicable)**
   ```python
   # tests/test_decorators.py
   import pytest
   from examples.example12_behavior_tree_decorators.decorators import RetryDecorator

   def test_retry_decorator():
       """Test retry behavior."""
       # Test implementation
       pass
   ```

2. **Test Example Manually**
   ```bash
   # Run the example
   python -m examples.example12_behavior_tree_decorators.demo

   # OR if integrated with main app:
   streamlit run app.py example12
   ```

3. **Verify Output**
   - Does it demonstrate the concept?
   - Is output clear and educational?
   - Are there any errors?

4. **Test Integration**
   - If example registers with main app, test registration works
   - Verify example appears in app menu/list

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
✅ Phase 2: Design
✅ Phase 3: Implementation
🔄 Phase 4: Testing [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Documentation
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 4: Testing example functionality
Status: Running example and verifying output
Started: [TIME]

TESTING PROGRESS:
✅ Tests written (if applicable)
✅ Example runs successfully
🔲 Output verified as educational
🔲 Integration tested
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Passing tests and working example.

---

### Phase 5: Documentation

**Purpose:** Create comprehensive README and inline documentation.

**Steps:**

1. **Write README.md**
   ```markdown
   # Example 12: Behavior Tree Decorators

   Demonstrates how to use decorator pattern with behavior trees.

   ## Concept

   [Explain the concept being demonstrated]

   ## Features

   - Retry decorator for fault tolerance
   - Timeout decorator for time limits
   - Logging decorator for observability

   ## Usage

   ```python
   [Code example showing how to use]
   ```

   ## Running the Example

   ```bash
   streamlit run app.py example12
   # OR
   python -m examples.example12_behavior_tree_decorators.demo
   ```

   ## Key Takeaways

   - [Lesson 1]
   - [Lesson 2]
   - [Lesson 3]
   ```

2. **Add Inline Documentation**
   - Docstrings for all classes and functions
   - Comments explaining non-obvious code
   - Type hints for clarity

3. **Create Visuals (if helpful)**
   - Diagrams showing structure
   - Screenshots of output
   - Flowcharts of behavior

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
✅ Phase 2: Design
✅ Phase 3: Implementation
✅ Phase 4: Testing
🔄 Phase 5: Documentation [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Integration
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 5: Creating documentation
Status: Writing README and inline docs
Started: [TIME]

DOCUMENTATION PROGRESS:
🔄 README.md (writing usage section)
✅ Inline docstrings added
⏸️ Visuals (if needed)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Complete documentation.

---

### Phase 6: Integration

**Purpose:** Register example with main application.

**Steps:**

1. **Update Main App Registration**
   - Add example to app.py or equivalent
   - Follow existing registration pattern

   Example for Streamlit:
   ```python
   # In app.py
   from examples.example12_behavior_tree_decorators import demo as ex12

   EXAMPLES = {
       # ...
       "example12": {
           "name": "Behavior Tree Decorators",
           "description": "Demonstrates decorator pattern",
           "module": ex12
       }
   }
   ```

2. **Test Integration**
   ```bash
   # Verify example appears in app
   streamlit run app.py

   # Select example from menu
   # OR
   streamlit run app.py example12
   ```

3. **Verify Navigation**
   - Example appears in correct order
   - Links work correctly
   - Description is clear

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
✅ Phase 2: Design
✅ Phase 3: Implementation
✅ Phase 4: Testing
✅ Phase 5: Documentation
🔄 Phase 6: Integration [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 7: Grievances (optional)

CURRENT TASK:
Phase 6: Integrating example with main app
Status: Updating registration
Started: [TIME]

INTEGRATION PROGRESS:
✅ Registration code added
🔄 Testing integration
⏸️ Verifying navigation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Integrated and accessible example.

---

### Phase 7: Grievances (Optional)

**Purpose:** Document friction points for framework improvement.

**Steps:**

1. **Create Grievances Document**
   ```bash
   # If project has grievances process
   touch ${EXAMPLES_DIR}/example12_behavior_tree_decorators/GRIEVANCES.md
   ```

2. **Document Issues Encountered**
   ```markdown
   # Grievances: Example 12

   ## Friction Points

   ### 1. Decorator API is verbose
   **Issue:** Creating decorators requires boilerplate
   **Example:**
   ```python
   # Current (verbose):
   class MyDecorator(Decorator):
       def __init__(self, child: Task):
           super().__init__(child)
           # More boilerplate...
   ```
   **Suggestion:** Provide decorator helper or simplified API

   ### 2. Documentation Gap
   **Issue:** Decorator pattern not documented in framework README
   **Suggestion:** Add section on common patterns
   ```

3. **Be Constructive**
   - Focus on friction, not blame
   - Provide specific examples
   - Suggest improvements where possible
   - Acknowledge good parts too

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Intake
✅ Phase 1: Research
✅ Phase 2: Design
✅ Phase 3: Implementation
✅ Phase 4: Testing
✅ Phase 5: Documentation
✅ Phase 6: Integration
🔄 Phase 7: Grievances [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 7: Documenting friction points (optional)
Status: Creating grievances document
Started: [TIME]

GRIEVANCES PROGRESS:
✅ Document created
🔄 Friction points documented
⏸️ Review for constructiveness
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Grievances document (if applicable).

---

## Completion

When all phases complete:

```markdown
## Example Creation Complete ✅

**Example**: example12_behavior_tree_decorators
**Location**: ${EXAMPLES_DIR}/example12_behavior_tree_decorators/

### Components Created
- ✓ Implementation code
- ✓ Tests passing
- ✓ README documentation
- ✓ Integrated with main app
- ✓ Grievances documented (optional)

### Verification
- ✓ Example runs successfully
- ✓ Demonstrates concept clearly
- ✓ Code follows project conventions
- ✓ Documentation is complete

### Next Steps
1. Commit example to repository
2. Test in clean environment
3. Consider follow-up examples if needed
```

---

## Notes

- Examples should be self-contained and runnable
- Focus on clarity over cleverness
- Demonstrate one concept well rather than many concepts poorly
- Progressive complexity (simple examples first, advanced later)
- Clear learning objectives in README
```

---

## Skill 2: create-tutorial

**Purpose:** Create step-by-step tutorial documentation with progressive learning.

**File:** `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`

```markdown
---
name: create-tutorial
description: Create step-by-step tutorial documentation
disable-model-invocation: true
argument-hint: "[tutorial topic]"
---

# Create Tutorial

Create comprehensive step-by-step tutorial content for progressive learning.

## When to Use

Use this skill when:
- Creating educational tutorials
- Teaching concepts step-by-step
- Building learning paths
- Need detailed explanations with code
- Want to guide learners through a process

Do NOT use when:
- Need working code without explanation (use create-example)
- Implementing production features (use feature-impl)
- Creating API documentation (use different tool)

## Input

- Tutorial topic description
- Learning objectives
- Target audience level

## Output

Complete tutorial document with:
- Clear learning objectives
- Step-by-step instructions
- Working code samples
- Explanations and context
- Visuals (if helpful)

## Process

### Phase 1: Define Learning Objectives

**Purpose:** Clarify what learners will gain from the tutorial.

**Steps:**

1. **Identify Learning Goals**
   - What will readers be able to do after completing this tutorial?
   - What concepts will they understand?
   - What skills will they acquire?

2. **Define Prerequisites**
   - What should readers know beforehand?
   - What tools/setup is required?

3. **Determine Scope**
   - What's included in this tutorial?
   - What's explicitly out of scope?

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 CREATE-TUTORIAL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 1: Define learning objectives [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Outline tutorial steps
⏸️ Phase 3: Write tutorial content
⏸️ Phase 4: Create code samples
⏸️ Phase 5: Add visuals
⏸️ Phase 6: Review and test

CURRENT TASK:
Phase 1: Defining what learners will achieve
Status: Identifying learning goals
Started: [TIME]

CHECKLIST:
🔲 Learning goals identified
🔲 Prerequisites defined
🔲 Scope determined
🔲 Target audience confirmed
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:**
```markdown
## Learning Objectives

**After completing this tutorial, you will be able to:**
- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]

**Prerequisites:**
- [Required knowledge]
- [Required tools/setup]

**Scope:**
- In scope: [What's covered]
- Out of scope: [What's not covered]
```

---

### Phase 2: Outline Tutorial Steps

**Purpose:** Break tutorial into sequential, buildable steps.

**Steps:**

1. **Design Step Sequence**
   - Start with simplest case
   - Build complexity progressively
   - Each step builds on previous

2. **Define Milestones**
   - What working state after each step?
   - What can learner verify?

3. **Plan Code Evolution**
   - How does code grow through tutorial?
   - What gets added at each step?

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Define learning objectives
🔄 Phase 2: Outline tutorial steps [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Write tutorial content
⏸️ Phase 4: Create code samples
⏸️ Phase 5: Add visuals
⏸️ Phase 6: Review and test

CURRENT TASK:
Phase 2: Outlining sequential steps
Status: Designing step progression
Started: [TIME]

OUTLINE PROGRESS:
✅ Step sequence designed
🔲 Milestones defined
🔲 Code evolution planned
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:**
```markdown
## Tutorial Outline

1. **Step 1: Basic Setup**
   - Milestone: Hello world running
   - Code: Basic structure

2. **Step 2: Add Feature X**
   - Milestone: Feature X working
   - Code: Extend with X

3. **Step 3: Handle Edge Cases**
   - Milestone: Robust implementation
   - Code: Add error handling

[... more steps ...]
```

---

### Phase 3: Write Tutorial Content

**Purpose:** Create clear explanations with code examples.

**Steps:**

1. **Write Introduction**
   - Hook readers with motivation
   - Present learning objectives
   - Preview what they'll build

2. **Write Each Step**
   - Clear instructions
   - Explain WHY, not just WHAT
   - Include code snippets
   - Explain tricky parts
   - Note common mistakes

3. **Add Checkpoints**
   - "At this point, you should see..."
   - "Verify by running..."
   - Help readers confirm progress

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Define learning objectives
✅ Phase 2: Outline tutorial steps
🔄 Phase 3: Write tutorial content [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Create code samples
⏸️ Phase 5: Add visuals
⏸️ Phase 6: Review and test

CURRENT TASK:
Phase 3: Writing explanations and instructions
Status: Writing step 3 of 6
Started: [TIME]

WRITING PROGRESS:
✅ Introduction written
✅ Step 1 complete
✅ Step 2 complete
🔄 Step 3 in progress
⏸️ Step 4-6 pending
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Draft tutorial content with explanations.

---

### Phase 4: Create Code Samples

**Purpose:** Provide working, tested code for each step.

**Steps:**

1. **Write Complete Code**
   - Code for each step
   - Must be runnable
   - Must be tested

2. **Add Comments**
   - Explain non-obvious parts
   - Highlight key concepts
   - Note variations

3. **Create Checkpoints**
   - Code that readers can verify against
   - Expected outputs
   - Test commands

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Define learning objectives
✅ Phase 2: Outline tutorial steps
✅ Phase 3: Write tutorial content
🔄 Phase 4: Create code samples [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Add visuals
⏸️ Phase 6: Review and test

CURRENT TASK:
Phase 4: Creating working code samples
Status: Writing code for step 4
Started: [TIME]

CODE PROGRESS:
✅ Step 1 code complete and tested
✅ Step 2 code complete and tested
✅ Step 3 code complete and tested
🔄 Step 4 code in progress
⏸️ Step 5-6 pending
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Working code samples for all steps.

---

### Phase 5: Add Visuals

**Purpose:** Enhance understanding with diagrams and screenshots.

**Steps:**

1. **Identify Visual Opportunities**
   - Complex concepts → diagrams
   - UI interactions → screenshots
   - Data flow → flowcharts
   - Architecture → architecture diagrams

2. **Create Visuals**
   - Keep simple and clear
   - Label everything
   - Use consistent style

3. **Integrate with Text**
   - Reference visuals in explanations
   - Place near relevant content
   - Add captions

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Define learning objectives
✅ Phase 2: Outline tutorial steps
✅ Phase 3: Write tutorial content
✅ Phase 4: Create code samples
🔄 Phase 5: Add visuals [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Review and test

CURRENT TASK:
Phase 5: Adding visual aids
Status: Creating architecture diagram
Started: [TIME]

VISUALS PROGRESS:
✅ Identified visual opportunities
🔄 Creating diagrams
⏸️ Adding screenshots
⏸️ Integration with text
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Tutorial with integrated visuals.

---

### Phase 6: Review and Test

**Purpose:** Ensure tutorial works end-to-end.

**Steps:**

1. **Test Code Samples**
   - Run each code sample
   - Verify outputs match tutorial
   - Fix any issues

2. **Check Step Progression**
   - Can a beginner follow this?
   - Are there gaps in explanation?
   - Is progression smooth?

3. **Verify Learning Objectives**
   - Does tutorial achieve stated goals?
   - Are all objectives covered?

4. **Proofread**
   - Fix typos
   - Improve clarity
   - Check formatting

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 1: Define learning objectives
✅ Phase 2: Outline tutorial steps
✅ Phase 3: Write tutorial content
✅ Phase 4: Create code samples
✅ Phase 5: Add visuals
🔄 Phase 6: Review and test [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 6: Final review and testing
Status: Testing code samples
Started: [TIME]

REVIEW PROGRESS:
✅ All code samples tested
🔄 Checking step progression
⏸️ Verifying learning objectives
⏸️ Proofreading
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** Polished, tested tutorial.

---

## Completion

When all phases complete:

```markdown
## Tutorial Creation Complete ✅

**Tutorial**: [Title]
**Location**: [Path]

### Components Created
- ✓ Learning objectives defined
- ✓ Sequential steps outlined
- ✓ Clear explanations written
- ✓ Working code samples
- ✓ Visuals integrated
- ✓ End-to-end tested

### Verification
- ✓ All code samples run successfully
- ✓ Progression is smooth and logical
- ✓ Learning objectives achieved
- ✓ No gaps in explanation

### Next Steps
1. Have someone follow the tutorial
2. Collect feedback
3. Iterate based on learner experience
```

---

## Best Practices

### Progressive Complexity
- Start with simplest possible case
- Add one concept at a time
- Don't introduce multiple new ideas simultaneously
- Build on what was already taught

### Clear Checkpoints
- After each step, learner should see results
- Provide expected outputs
- Include verification commands
- Help learners know they're on track

### Explain Why, Not Just What
- Don't just show code, explain reasoning
- Discuss alternatives and trade-offs
- Explain when to use this approach
- Share common mistakes and how to avoid them

### Working Code
- Every code sample must run
- Test all code yourself
- Include complete examples, not fragments
- Provide setup instructions

### Visual Aids
- Use diagrams for complex concepts
- Screenshot UI interactions
- Flowcharts for decision logic
- Keep visuals simple and focused
```

---

## Phase 3: Integration

### How Skills Integrate

Both skills create educational content but serve different purposes:

```
┌─────────────────────────────────────────────────────────────┐
│                   Educational Content                        │
└─────────────────────────────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
    ┌──────────────┐              ┌──────────────┐
    │create-example│              │create-tutorial│
    │              │              │              │
    │Working code  │              │Step-by-step  │
    │first, docs   │              │teaching, code│
    │explain it    │              │illustrates   │
    └──────────────┘              └──────────────┘
          │                               │
          ▼                               ▼
    ┌──────────────┐              ┌──────────────┐
    │Examples dir  │              │Docs or       │
    │with runnable │              │tutorials dir │
    │demos         │              │              │
    └──────────────┘              └──────────────┘
```

### Integration with Main Application

Examples typically integrate with the main app:

```python
# app.py or main.py
from examples.example12_name import demo

# Register example
EXAMPLES = {
    "example12": {
        "name": "Feature Name",
        "description": "What it demonstrates",
        "module": demo
    }
}
```

### Integration with Framework

Examples demonstrate framework features:

```python
# Import from framework
from framework.module import FeatureClass

# Use in example
demo_instance = FeatureClass(params)
```

### Integration with Documentation

Tutorials and examples can reference each other:

```markdown
# In tutorial
See [Example 12](../examples/example12_name/) for a working implementation.

# In example README
For a detailed tutorial on this pattern, see [Tutorial: Decorators](../docs/tutorials/decorators.md).
```

### Cross-Referencing Pattern

```
Tutorial (concepts) ──references──> Example (working code)
                    <──implements──
Example (demo)     ──references──> Tutorial (explanation)
                    <──explained by──
```

---

## Phase 4: Testing & Validation

### Validation Strategy

Test each skill individually to ensure they produce quality educational content.

### Test 1: Simple Example

**Scenario:** Create a basic, single-file example

**Steps:**
1. Invoke: `/create-example "Simple hello world example"`
2. Verify research phase reads existing examples
3. Check design is simple and appropriate
4. Verify implementation follows conventions
5. Check documentation explains concept
6. Test integration with app

**Expected Output:**
- Single-file or simple multi-file example
- Clear README
- Runs successfully
- Integrated with main app
- Follows project conventions

### Test 2: Complex Example

**Scenario:** Create an example with multiple components

**Steps:**
1. Create spec file: `${SPEC_DIR}/YYYY-MM-DD-example12-spec.md`
2. Invoke: `/create-example ${SPEC_DIR}/YYYY-MM-DD-example12-spec.md`
3. Verify all phases complete
4. Check multiple files created
5. Verify tests included (if applicable)
6. Check comprehensive README
7. Test integration

**Expected Output:**
- Multiple related files
- Tests passing (if applicable)
- Comprehensive documentation
- Integrated with app
- Demonstrates concept clearly

### Test 3: Tutorial Creation

**Scenario:** Create a step-by-step tutorial

**Steps:**
1. Invoke: `/create-tutorial "Introduction to behavior trees"`
2. Verify learning objectives are clear
3. Check step progression is logical
4. Verify code samples are complete and working
5. Check explanations are clear
6. Verify visuals enhance understanding

**Expected Output:**
- Clear learning objectives
- Progressive step sequence
- All code samples run
- Good explanations (why, not just what)
- Helpful visuals (if applicable)
- Achieves stated learning goals

### Test 4: Example Numbering

**Scenario:** Verify example numbering is automatic and correct

**Steps:**
1. Check existing examples: `ls ${EXAMPLES_DIR}/`
2. Note highest number (e.g., example11)
3. Create new example
4. Verify it uses next number (example12)

**Expected Output:**
- Correct automatic numbering
- No conflicts with existing examples
- Consistent naming pattern

### Test 5: Framework Integration

**Scenario:** Verify example uses framework correctly

**Steps:**
1. Create example that uses framework features
2. Verify framework documentation is read
3. Check correct framework APIs are used
4. Verify example follows framework patterns
5. Test grievances capture friction points (if applicable)

**Expected Output:**
- Framework APIs used correctly
- Patterns match framework conventions
- Grievances document constructive feedback (optional)

---

## Phase 5: Best Practices

### Self-Contained Examples

**Why it matters:**
- Learners can run examples independently
- No hidden dependencies
- Clear what's needed to make it work

**How to implement:**
- Include all necessary code in example directory
- Document external dependencies in README
- Provide setup instructions
- Include sample data if needed

### Progressive Complexity

**Why it matters:**
- Beginners can start simple
- Advanced users can find complex examples
- Clear learning path through examples

**How to implement:**
- Number examples by complexity (roughly)
- Simple examples first (example1, example2)
- Advanced examples later (example10, example11)
- Note difficulty in README
- Reference prerequisite examples

### Clear Learning Objectives

**Why it matters:**
- Learners know what they'll gain
- Can choose appropriate content
- Can verify they learned correctly

**How to implement:**
- Start README with "What you'll learn"
- List specific skills/concepts
- Include "Prerequisites" section
- Add "Key Takeaways" at end

### Working Code Always

**Why it matters:**
- Learners can verify examples work
- Builds confidence
- Catches errors early

**How to implement:**
- Test all code before documenting
- Include run instructions
- Show expected output
- Provide troubleshooting tips

### Explain Why, Not Just What

**Why it matters:**
- Deep understanding vs. cargo cult
- Learners can apply to new situations
- More valuable long-term

**How to implement:**
- Add comments explaining reasoning
- Discuss alternatives and trade-offs
- Explain when to use this pattern
- Note common mistakes

### Grievances for Framework Improvement

**Why it matters:**
- Identifies friction points
- Improves framework over time
- Shows developers where to focus

**How to implement:**
- Document pain points encountered
- Be specific and constructive
- Suggest improvements
- Keep separate from main README

---

## Summary

### Skills Created

After following this guide, you will have:

- `${TOOL_CONFIG}/skills/create-example/SKILL.md`
- `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`

### Key Capabilities

**Create Examples:**
- Working code demonstrations
- Self-contained and runnable
- Integrated with main application
- Clear documentation
- Progressive complexity
- Framework pattern demonstrations

**Create Tutorials:**
- Step-by-step learning content
- Clear learning objectives
- Progressive complexity
- Working code samples
- Visual aids
- Achievement verification

### Educational Content Patterns

**Example Pattern:**
```
Research framework → Design structure → Implement code
  → Test functionality → Document usage → Integrate with app
```

**Tutorial Pattern:**
```
Define objectives → Outline steps → Write content
  → Create code samples → Add visuals → Test end-to-end
```

### Quality Standards

**Examples Must Have:**
- Working, runnable code
- Clear README explaining concept
- Usage instructions
- Key takeaways
- Integration with main app (if applicable)

**Tutorials Must Have:**
- Clear learning objectives
- Progressive step sequence
- Working code samples
- Explanations of WHY
- Verification checkpoints

### Next Steps

1. **Create skills** using templates in this guide
2. **Customize** for your project (paths, conventions)
3. **Test** with simple example first
4. **Create** a few examples to validate
5. **Iterate** based on learner feedback

### Related Guides

- **Feature Development:** For building production features
- **Bugfixing:** For fixing bugs in examples
- **Refactoring:** For improving example code quality

---

## Appendix: Quick Reference

### Invocation Commands

```bash
# Create example from description
/create-example "Demonstrate retry pattern with decorators"

# Create example from spec
/create-example ${SPEC_DIR}/YYYY-MM-DD-example12-spec.md

# Create tutorial
/create-tutorial "Introduction to behavior trees"
```

### File Locations

```
${EXAMPLES_DIR}/
├── example1_basic/
│   ├── __init__.py
│   ├── basic.py
│   ├── README.md
│   └── tests/
├── example12_advanced/
│   ├── __init__.py
│   ├── advanced.py
│   ├── demo.py
│   ├── README.md
│   ├── tests/
│   ├── assets/
│   └── GRIEVANCES.md (optional)
└── ...

${TOOL_CONFIG}/skills/
├── create-example/SKILL.md
└── create-tutorial/SKILL.md

docs/tutorials/  (or similar)
└── tutorial-name.md
```

### Example Structure Template

```
example{N}_{name}/
├── __init__.py          # Module initialization
├── {name}.py            # Main implementation
├── demo.py              # Demo script (optional)
├── README.md            # Documentation
├── tests/               # Tests (optional)
│   └── test_{name}.py
├── assets/              # Images, data (optional)
└── GRIEVANCES.md        # Framework feedback (optional)
```

### Tutorial Structure Template

```markdown
# Tutorial: [Title]

## What You'll Learn
- [Objective 1]
- [Objective 2]

## Prerequisites
- [Required knowledge]
- [Required setup]

## Step 1: [Title]
[Explanation]
```code
[Code sample]
```
**Checkpoint:** You should see [expected result]

## Step 2: [Title]
...

## Key Takeaways
- [Lesson 1]
- [Lesson 2]
```

### Decision Tree

```
Do you need educational content?
├─ YES → Choose between example and tutorial
└─ NO → Use feature-impl or other skills

Is it primarily code demonstration?
├─ YES → Use create-example
└─ NO → Use create-tutorial

Does it need to integrate with main app?
├─ YES → Use create-example with Phase 6
└─ NO → create-tutorial or standalone example

Is it teaching a process step-by-step?
├─ YES → Use create-tutorial
└─ NO → Use create-example
```

---

**Last Updated:** 2026-02-14
**Related:**
- [Feature Development](feature-development.md)
- [Bugfixing](bugfixing.md)
- [Core Conventions](../setup-guidelines/core-conventions.md)
