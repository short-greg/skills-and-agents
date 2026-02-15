# Example/Tutorial Skill Suite Setup

Setup instructions for skills that create framework examples and tutorials.

---

## Overview

The **Example/Tutorial Skill Suite** helps create educational content and framework examples that demonstrate concepts and patterns.

**This suite includes:**
- `create-example` - Create framework example with implementation and documentation
- `create-tutorial` - Create step-by-step tutorial content

**When to use:**
- Building framework examples
- Creating educational tutorials
- Demonstrating framework features
- Documenting usage patterns

**Output:** Complete example with code, tests, documentation, and integration

---

## Phase 0: Repository Detection

Before setting up the example/tutorial suite, detect and validate the repository structure.

### Step 1: Detect Examples Directory

```bash
# Find where examples are stored
for dir in examples src/examples lib/examples; do
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
```

### Step 2: Review Existing Examples

```bash
# List existing examples to understand pattern
ls -la ${EXAMPLES_DIR}/

# Read 1-2 examples to extract structure
# Typical structure:
# examples/
# ├── example1_name/
# │   ├── __init__.py or main.py
# │   ├── README.md
# │   └── tests/ (optional)
```

### Step 3: Detect Framework Directory

```bash
# Check if project has a framework subdirectory or submodule
ls -la framework/ 2>/dev/null && echo "Framework: framework/"

# Read framework docs
ls framework/README.md framework/docs/ 2>/dev/null
```

### Step 4: Detect Main Application Entry Point

```bash
# Find main application file
for file in app.py main.py __main__.py src/main.py; do
  if [ -f "$file" ]; then
    echo "Found main app: $file"
    MAIN_APP="$file"
    break
  fi
done
```

### Step 5: Read Project Conventions

```bash
# Read project documentation
cat CLAUDE.md README.md 2>/dev/null

# Look for:
# - Example naming conventions
# - Example structure requirements
# - Registration process (how examples integrate with main app)
# - Documentation standards
# - Grievance documentation process (if applicable)
```

### Step 6: Check Spec Directory

```bash
# Find spec directory for example specs
for dir in specs dev-docs docs .docs; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done
```

**Output Phase 0 Summary:**
```markdown
## Repository Detection Summary

- **Examples directory**: examples/ [or detected location]
- **Framework directory**: framework/ [if applicable]
- **Main app**: app.py [or detected]
- **Spec directory**: specs/ [or detected]
- **Tool config**: .claude/ [or detected]
- **Existing examples**: 10 [or count]
- **Example naming pattern**: example{N}_{name} [or detected]
- **Registration process**: [How examples integrate with main app]
```

**Gate:** Do not proceed until examples directory exists and conventions are understood.

---

## Phase 1: Validate Prerequisites

Ensure all prerequisites are met for example/tutorial creation.

### Step 1: Verify Examples Directory Exists

```bash
# Create if doesn't exist
mkdir -p ${EXAMPLES_DIR}
```

### Step 2: Verify Framework Access

```bash
# If project uses a framework, verify it's accessible
ls framework/README.md 2>/dev/null || echo "No framework found"

# Read framework docs to understand available features
cat framework/README.md
```

### Step 3: Verify Tool Skills Directory

```bash
# Create skills directories
mkdir -p ${TOOL_CONFIG}/skills/create-example
mkdir -p ${TOOL_CONFIG}/skills/create-tutorial
```

### Step 4: Verify Application Integration

```bash
# Check if main app can register examples
grep -q "examples" ${MAIN_APP} 2>/dev/null || echo "Main app may need example registration support"
```

### Step 5: Review Example Structure Pattern

Identify the standard example structure:

```
examples/
└── example{N}_{name}/
    ├── __init__.py          # Python: module initialization
    ├── {name}.py            # Main implementation
    ├── README.md            # Documentation
    ├── tests/               # Optional: example-specific tests
    │   └── test_{name}.py
    └── assets/              # Optional: images, data files
```

**Output Phase 1 Summary:**
```markdown
## Prerequisites Validation

- [✓] Examples directory exists: ${EXAMPLES_DIR}
- [✓] Framework accessible (if applicable)
- [✓] Tool skills directory exists
- [✓] Main app structure understood
- [✓] Example structure pattern identified
```

**Gate:** Do not proceed until all prerequisites are met.

---

## Phase 2: Define Skills in Suite

This suite includes two skills following the naming conventions.

### Skill 1: `create-example`

**Name:** `create-example` (follows `{category}-{action}` pattern)
**Alternative names:** `example-create`, `make-example`

**Purpose:** Create a complete framework example with implementation, tests, documentation, and integration.

**Phases:**
0. Intake - Understand example requirements
1. Research - Study framework docs and existing examples
2. Design - Plan example structure and features
3. Implementation - Build the example
4. Testing - Ensure example works correctly
5. Documentation - Create README and inline docs
6. Integration - Register example with main app
7. Grievances (optional) - Document friction points for framework improvement

**Output:** Complete example at `${EXAMPLES_DIR}/example{N}_{name}/` with all components

**Example Structure:**
```
examples/example12_feature_name/
├── __init__.py
├── feature_name.py
├── README.md
├── tests/
│   └── test_feature_name.py
└── assets/ (if needed)
```

### Skill 2: `create-tutorial`

**Name:** `create-tutorial` (follows `{category}-{action}` pattern)

**Purpose:** Create step-by-step tutorial documentation.

**Phases:**
1. Define Learning Objectives - What will readers learn?
2. Outline Tutorial Steps - Sequential, buildable steps
3. Write Tutorial Content - Clear explanations with code
4. Create Code Samples - Working, tested examples
5. Add Visuals - Diagrams, screenshots if helpful
6. Review and Test - Ensure tutorial works

**Output:** Tutorial document at appropriate location

---

## Phase 3: Provide SKILL.md Templates

Complete template for the create-example skill.

### Template: create-example

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
- Implementing production features (use impl-feature)
- Creating tutorials without code (use create-tutorial)
- Fixing existing examples (use impl-bugfix)

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

**Output:**
```markdown
## Example Spec Summary

- **Number**: 12
- **Name**: example12_behavior_tree_decorators
- **Goal**: Demonstrate BT decorator patterns
- **Features**: Retry decorator, timeout decorator, logging decorator
- **Directory**: examples/example12_behavior_tree_decorators/
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
   import streamlit as st

   # Framework (dachi)
   from dachi.act.bt import Task, Decorator

   # Local example
   from .decorators import RetryDecorator
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
       # Test retry behavior
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
   ```

   ## Key Takeaways

   - [Lesson 1]
   - [Lesson 2]
   ```

2. **Add Inline Documentation**
   - Docstrings for all classes and functions
   - Comments explaining non-obvious code
   - Type hints for clarity

3. **Create Visuals (if helpful)**
   - Diagrams showing structure
   - Screenshots of output
   - Flowcharts of behavior

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
```
<!-- End of create-example SKILL.md template -->

---

### Template: create-tutorial

**File:** `${TOOL_CONFIG}/skills/create-tutorial/SKILL.md`

```markdown
---
name: create-tutorial
description: Create step-by-step tutorial documentation
disable-model-invocation: true
argument-hint: "[tutorial topic]"
---

# Create Tutorial

Create comprehensive step-by-step tutorial content.

## Process

### Phase 1: Define Learning Objectives
What will readers be able to do after completing tutorial?

### Phase 2: Outline Tutorial Steps
Break into sequential, buildable steps.

### Phase 3: Write Tutorial Content
Clear explanations with code examples.

### Phase 4: Create Code Samples
Working, tested code for each step.

### Phase 5: Add Visuals
Diagrams, screenshots, flowcharts if helpful.

### Phase 6: Review and Test
Ensure tutorial works end-to-end.
```

---

## Phase 4: Integration Instructions

How the example/tutorial skills work with other parts of the project.

### Integration with Main Application

Examples integrate with the main app:

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

Examples referenced in docs:

```markdown
# In framework/docs/
See [Example 12](../../examples/example12_name/) for usage.
```

---

## Phase 5: Testing & Validation

Test the example creation workflow.

### Test 1: Simple Example

Create a basic example:

```bash
create-example "Simple greeting example"
```

**Expected:**
- Single file example
- README
- Works when run

### Test 2: Complex Example

Create an example with multiple components:

```bash
create-example ${SPEC_DIR}/2026-02-12-example12-spec.md
```

**Expected:**
- Multiple files
- Tests included
- Comprehensive README
- Integrated with app

### Test 3: End-to-End

Full workflow:

```bash
# 1. Create spec for example
# 2. Create example
create-example ${SPEC_DIR}/2026-02-12-example13-spec.md

# 3. Test example
streamlit run app.py example13

# 4. Verify integration
```

---

## Troubleshooting

### Issue: Example doesn't integrate with app

**Check:**
- Registration code in app.py correct?
- Import path correct?
- Example has required entry point?

### Issue: Framework imports fail

**Check:**
- Framework submodule initialized?
- Framework path in PYTHONPATH?
- Correct framework version?

### Issue: Example numbering conflicts

**Check:**
- Multiple examples with same number?
- Check ${EXAMPLES_DIR} for duplicates
- Renumber if needed

---

## Customization

### Adapting to Your Project

1. **Examples directory**: Replace `${EXAMPLES_DIR}` with your path
2. **Naming convention**: Adjust example naming pattern
3. **Registration process**: Customize for your app structure
4. **Grievances**: Optional - include if doing framework development

---

## Summary

**This suite provides:**
- ✅ Complete example creation workflow
- ✅ Research → Design → Implement → Document → Integrate
- ✅ Tutorial creation for step-by-step guides
- ✅ Framework demonstration patterns

**Key skills:**
- `create-example` - Create complete framework examples
- `create-tutorial` - Create tutorial documentation

**Prerequisites met:**
- Examples directory identified
- Framework accessible
- Integration pattern understood
- Tool skills directory configured

**Next steps:**
1. Copy SKILL.md templates to your tool's config directory
2. Customize with your project's paths
3. Test with a simple example
4. Use for educational content creation!

---

**Last Updated:** 2026-02-12
**Related:**
- [Skills and Agents Guidelines](skills-and-agents-setup-guidelines-and-conventions.md)
- [Implementation Skill Suite](implementation-skill-suite-setup.md) - Similar process but for production features
