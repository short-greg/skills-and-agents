Purpose: This document defines the template for an individual skill.

[TEMPLATE]

---
name: [skill-name]
description: [One-line description]
argument-hint: "[expected input format]"
protocols:
  - [protocol-name]  # Use case: when/why this protocol applies to this skill
---

## Protocol References

List protocols this skill uses with a one-sentence use case explaining when/why:

```yaml
protocols:
  - reasoning_patterns  # Reason about approach before starting, verify completion after
  - goals_and_objectives  # Write SMARB criteria when defining success for the task
  - project_quality  # Assess quality dimensions when evaluating code
  - doc_maintenance  # Update documentation when implementation changes behavior
  - manage_complexity_uncertainty_risk  # Identify and mitigate risks during design or planning
  - modularity  # Ensure designs are modular and loosely coupled
```

**Guidelines:**
- Only include protocols that genuinely apply to this skill
- The use case statement should explain a specific situation when the protocol is used
- Don't add protocols just because they might tangentially relate

# Skill: [Skill Display Name]

---

# Background
[Context and motivation. Why does this skill exist? What problems led to its creation?]

---

# Intent
[What this skill accomplishes - the "why"]

---

# Role
[Persona that defines reasoning approach]

**Reasoning Approach:**
- [How this persona thinks about problems]
- [What this persona prioritizes]
- [What expertise this persona brings]

**Example Personas:**
- `Senior Developer` - Prioritizes code quality, patterns, maintainability
- `Security Engineer` - Prioritizes vulnerabilities, validation, secrets
- `QA Engineer` - Prioritizes edge cases, coverage, regression prevention

---

# When to Use

**Use this skill when:**
- [Use case 1]
- [Use case 2]

**When NOT to use:**
- [Anti-use case 1]
- [Anti-use case 2]

---

# Command

**Name:** /[skill-name]
**Arguments:** [argument-hint]
**Examples:**
```
/skill-name "example input"
/skill-name path/to/file.md
```

---

# OKRs

[See [protocols/goals_and_objectives.md](../protocols/goals_and_objectives.md)]

**Objective:** [Aspirational outcome]

**Key Results:**
1. [Measurable outcome with threshold]
2. [Measurable outcome with threshold]
3. [Measurable outcome]

---

# Prerequisites

**Required:**
- [Prerequisite 1 - file, directory, dependency]
- [Prerequisite 2]

**Validated By:**
- Check: [How to verify prerequisite exists]

---

# Risks

**Inherent risks associated with this skill:**

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | [Low/Med/High] | [Low/Med/High] | [How to prevent/handle] |
| [Risk 2] | [Low/Med/High] | [Low/Med/High] | [How to prevent/handle] |

**Destructive potential:**
- [What could be overwritten/deleted]
- [What requires careful handling]

---

# Process

**How this skill creates its checklist on execution:**

[See [protocols/checklist_management.md](../protocols/checklist_management.md)]

## Checklist Generation Rules

**Base items (always included):**
- [Item that always runs]
- [Item that always runs]

**Conditional items:**
```yaml
- condition: "repo_type == 'production'"
  items:
    - "Run security scans"
    - "Run modularity assessment"

- condition: "repo_type == 'library'"
  items:
    - "Verify API documentation"
    - "Check backward compatibility"
```

**Loop gates:**
```yaml
- trigger: "tests_fail"
  action: "go to step [run_tests]"
- trigger: "coverage < threshold"
  action: "go to step [add_tests]"
```

**Final items (always last):**
- Update progress in ${SPEC_DIR}/[doc].md

## Phase Structure

**Phases for this skill:**
1. [Phase name] - [Purpose]
2. [Phase name] - [Purpose]
3. [Phase name] - [Purpose]

**Gates between phases:**
- After Phase 1: [What must pass]
- After Phase 2: [What must pass]

**User interaction points:**
- `confirm`: [What to confirm with user]
- `ask`: [What to ask user if unclear]

## Input Handling

**Expected input:**
- [Input type and format]

**Validation:**
- [How to validate input]

**If input missing/unclear:**
- Ask user: "[Question to ask]"

## Error Recovery

**When steps fail:**
- [Recovery strategy 1]
- [Recovery strategy 2]

**When to escalate to user:**
- [Condition requiring user input]

## Documentation Updates

**Update when:**
- New patterns introduced
- API changes made
- Conventions established

**What to update:**
- Project documentation (if project-wide pattern)
- Agent documentation (if agent-related)
- Subpackage docs (keep in sync with main)

[See [protocols/tracking.md](../protocols/tracking.md)]

---

# Deliverables

**What this skill produces:**

| Output | Location | Format |
|--------|----------|--------|
| [Output 1] | ${SPEC_DIR}/[file].md | Markdown |
| [Output 2] | [path] | [format] |

---

# Reporting/Transparency

**Progress tracking format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 [SKILL-NAME] PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ROLE: [Persona Name]

PHASES:
✅ Phase 1: [Name]
🔄 Phase 2: [Name] [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: [Name]

CHECKLIST (Phase 2):
[x] 1. [Completed item]
[x] 2. [Completed item]
[ ] 3. [Current item] ◀── CURRENT
[ ] 4. [Pending item]

CONTEXT:
- Repo type: ${repo_type}
- [Other relevant context]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Checklist markers:**
- `[ ]` = pending
- `[x]` = completed
- `[-]` = skipped (condition not met)
- `!` after number = loop gate

---

# Fault Tolerance

**Handling failures gracefully:**

**Rollback strategy:**
- [What can be rolled back]
- [How to restore previous state]

**Partial completion:**
- [What state is saved]
- [How to resume from failure]

**Recovery:**
- [Steps to recover from common failures]

---

# Related Skills

**Works with:**
- [Skill that this calls or is called by]

**Alternative to:**
- [Skill that solves similar problem differently]

**Part of workflow:**
- [Orchestrator that includes this skill]
