# Prerequisites Validation (Phase 1)

Common prerequisites validation process used by all skill setup guides.

---

## Purpose

After repository detection (Phase 0), validate that all prerequisites are met before creating skill templates.

---

## Step 1: Verify Spec Directory Exists

Create the spec directory if it doesn't exist:

```bash
# Create if doesn't exist
mkdir -p ${SPEC_DIR}

# Verify creation
ls -la ${SPEC_DIR}
```

---

## Step 2: Verify Tool Skills Directory Exists

Create the skills directory structure:

```bash
# Create base skills directory
mkdir -p ${TOOL_CONFIG}/skills

# Create skill-specific directories (customize per suite)
# Example for feature-define suite:
# mkdir -p ${TOOL_CONFIG}/skills/feature-define
# mkdir -p ${TOOL_CONFIG}/skills/feature-define-review
```

---

## Step 3: Test File Access

Verify read/write access to required directories:

```bash
# Test spec directory access
touch ${SPEC_DIR}/.test-access && rm ${SPEC_DIR}/.test-access

# Test skills directory access
touch ${TOOL_CONFIG}/skills/.test-access && rm ${TOOL_CONFIG}/skills/.test-access
```

---

## Step 4: Verify Project Documentation Access

Ensure project documentation is readable:

```bash
# Test reading project files
cat CLAUDE.md README.md 2>/dev/null || echo "No project documentation found"

# Check for framework documentation if applicable
ls framework/README.md framework/docs/ 2>/dev/null
```

---

## Step 5: Identify Skill Consumers

Document who/what will use the skills being configured:

| Consumer Type | Examples | Implications |
|---------------|----------|--------------|
| **Other skills** | feature-plan consumes feature-define output | Output format must match expected input |
| **Workflow orchestrators** | feature-workflow calls atomic skills | Skills must support orchestration mode |
| **Users directly** | Developer runs /feature-define | Skills need clear invocation instructions |
| **Stakeholders** | Product manager reviews PRDs | Output must be human-readable |

---

## Output: Prerequisites Summary

After validation, document status:

```markdown
## Prerequisites Validation

- [x] Spec directory exists: ${SPEC_DIR}
- [x] Tool skills directory exists: ${TOOL_CONFIG}/skills
- [x] File access verified
- [x] Project documentation accessible
- [x] Skill consumers identified
```

---

## Gate

**Do not proceed to Phase 2 (Skill Definition) until:**
- All directories exist and are writable
- Project documentation is accessible
- Skill consumers are understood

---

## Usage in Setup Guides

Each skill setup guide should reference this document:

```markdown
## Phase 1: Validate Prerequisites

Follow the standard prerequisites validation from [shared/prerequisites.md](../shared/prerequisites.md).

**Suite-specific prerequisites:**
- [Any additional prerequisites specific to this skill suite]
- [e.g., "PRDs must exist before running feature-plan"]

**Skill directories to create:**
- ${TOOL_CONFIG}/skills/[skill-name]
- ${TOOL_CONFIG}/skills/[skill-name-review]
```

This ensures consistent validation across all skill setups while allowing suite-specific additions.
