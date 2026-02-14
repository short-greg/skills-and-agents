# Core Conventions

Essential patterns and conventions for creating reliable AI coding assistant skills.

---

## Overview

This document defines the core patterns that make skills:
1. **Complete reliably** (progress tracking)
2. **Follow conventions** (documentation maintenance)
3. **Adapt to projects** (customizable templates)

These conventions apply to ALL skills in this framework.

---

## Progress Tracking Pattern

### Problem

AI skills often skip steps or don't complete workflows because:
- No visibility into what step they're on
- Easy to forget phases in complex workflows
- No audit trail of what happened

### Solution

Skills report their progress as they work using:
1. **State variable** - Tracks current phase
2. **Status log** - Audit trail of completed work
3. **Visual progress indicator** - Shows where skill is in the workflow

### Implementation

#### 1. State Variable

Each skill maintains a state variable showing:
- Current phase
- Next phase
- Any blocking issues

Example:
```markdown
**CURRENT STATE:**
- Phase: 3 (Write Tests)
- Status: In Progress
- Next: Phase 4 (Implement Feature)
- Blocking: None
```

#### 2. Status Log

Append-only log of what happened:
```markdown
**STATUS LOG:**
- [10:30] Phase 0: Read documentation - COMPLETE
- [10:35] Phase 1: Research existing code - COMPLETE
- [10:45] Phase 2: Design structure - COMPLETE
- [10:50] Phase 3: Write tests - IN PROGRESS
  - Created test_main_function
  - Created test_helper_function
  - Next: Verify tests fail
```

#### 3. Visual Progress Indicator

Display progress after each phase:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
🔄 Phase 3: Write tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Implement feature
⏸️ Phase 5: Run tests
⏸️ Phase 6: Update documentation

CURRENT TASK:
Phase 3: Writing tests before implementation (TDD)
Status: Creating test cases for main functions
Started: 10:50

CHECKLIST:
✅ Write happy path tests
🔲 Write edge case tests
🔲 Write error handling tests
🔲 Verify tests fail
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### When to Update Progress

**MUST update after:**
- Completing a phase
- Starting a new phase
- Encountering blockers

**MAY update during:**
- Long-running phases (show incremental progress)
- User review gates (show what's being reviewed)

### Benefits

1. **Less likely to skip steps** - Actively reporting makes skipping obvious
2. **Creates audit trail** - Status log shows what happened
3. **User visibility** - Users see progress without asking
4. **Resumability** - Can resume from state variable

### User Approval Gates (OPTIONAL)

Skills CAN include review gates where user approves before proceeding:
```markdown
### Phase 2: Review Requirements

**User Review Gate**

Present the PRD to the user:
1. Read the generated PRD
2. Summarize key requirements
3. Ask: "Does this PRD capture the requirements correctly?"

**If changes requested:** Update and return to this phase
**If approved:** Proceed to Phase 3
```

**IMPORTANT:** Review gates are OPTIONAL. Skills should ask the user if they want to review:
- "Would you like to review the PRD before I create the plan?"
- "Should I proceed with implementation or would you like to review the design first?"

Don't force reviews on every step - let users opt in.

---

## Documentation Maintenance Pattern

### Problem

Documentation drift causes:
- Convention violations (skills don't know the patterns)
- Code duplication (skills don't know what exists)
- Tool misuse (skills don't know the standard tools)
- Architectural inconsistency (skills don't know the structure)

### Solution

Implementation skills MUST:
1. **Read documentation** before starting (Phase 0)
2. **Follow conventions** during implementation
3. **Update documentation** when done (Final Phase)

### Implementation

#### Phase 0: Read Documentation (MANDATORY)

Before starting work, read:
```markdown
### Phase 0: Read Documentation

**Purpose:** Understand project conventions before starting.

**READ REQUIRED FILES:**
1. Read CLAUDE.md (or equivalent project conventions file)
2. Read relevant module READMEs
3. Review coding standards
4. Note patterns to follow

**CONFIRM UNDERSTANDING:**
- What conventions apply to this feature?
- What existing patterns should I use?
- What tools/libraries are standard?
- What testing practices are required?

**Gate:** Conventions understood.
```

#### During Implementation: Follow Conventions

As you implement:
- Use patterns from Phase 0
- Follow naming conventions
- Use standard tools/libraries
- Match existing structure

Example:
```markdown
**FOLLOWING CONVENTIONS:**
- Using BatchProcessor for bulk operations (from CLAUDE.md)
- Batch size: 100 (project standard)
- All operations atomic (project requirement)
```

#### Final Phase: Update Documentation (MANDATORY)

After implementation, update docs:
```markdown
### Final Phase: Update Documentation

**Purpose:** Maintain project conventions and prevent documentation drift.

**UPDATE REQUIRED FILES:**

1. **Update CLAUDE.md**
   - Add new patterns introduced
   - Document new conventions
   - Add usage examples

2. **Update Module READMEs**
   - Document new functions/classes
   - Add usage examples
   - Note dependencies

3. **Check for Drift**
   - Did implementation follow conventions?
   - Were existing patterns used?
   - Any new patterns that should be documented?
```

### Example: Before and After

**Before Implementation:**
```markdown
## Phase 0: Read Documentation

READ CLAUDE.md:
- Found: "Use BatchProcessor for bulk operations"
- Found: "Batch sizes must not exceed 100"
- Found: "All batch operations must be atomic"

✅ I understand the conventions
✅ I will use existing BatchProcessor
✅ I will not create duplicate batch logic
```

**During Implementation:**
```markdown
## Implementation

Using BatchProcessor from utils.batch:
```python
from utils.batch import BatchProcessor

def approve_batch(items):
    processor = BatchProcessor(batch_size=100)  # Following convention
    return processor.process_atomic(items)       # Atomic as required
```
```

**After Implementation:**
```markdown
## Final Phase: Update Documentation

**CLAUDE.md UPDATED:**
- Added: Example usage of BatchProcessor for moderation
- No new patterns needed - used existing ones

**moderation/README.md UPDATED:**
- Added: approve_batch function documentation
- Added: Dependency on utils.batch
- Added: Usage example
```

### Benefits

1. **Prevents code duplication** - Skills know what exists
2. **Enforces conventions** - Skills read before starting
3. **Keeps docs current** - Skills update after finishing
4. **Reduces drift** - Conventions stay accurate over time

---

## Naming Conventions

### Skills

Use the `{task}-{action}` pattern:
- `feature-define` (task: feature, action: define)
- `feature-plan` (task: feature, action: plan)
- `feature-impl` (task: feature, action: implement)
- `bugfix-impl` (task: bugfix, action: implement)
- `refactor-impl` (task: refactor, action: implement)

**Exception:** Content creation skills use `{action}-{noun}`:
- `create-example` (create educational content)
- `create-tutorial` (create educational content)

These group naturally under "create content" category.

### Workflow Skills

Append `-workflow` to orchestrators:
- `feature-workflow` - Orchestrates feature-define → feature-plan → feature-impl
- `bugfix-workflow` - Orchestrates bugfix investigation and implementation
- `refactor-workflow` - Orchestrates refactoring safely

### Review Skills

Append `-review` to review/validation skills:
- `feature-define-review` - Reviews PRD
- `feature-plan-review` - Reviews development plan
- `code-review` - Reviews implementation

### Parallel Orchestration

Use `parallel-` prefix:
- `parallel-orchestrate` - Sets up parallel work
- `parallel-task-workflow` - Runs in each parallel worktree

---

## File Structure Conventions

### Setup Guide Files

Location: `skill-guidelines/`

Naming:
- `{task-category}.md` - e.g., `feature-development.md`, `bugfixing.md`

Content:
- All related skills in ONE file
- Complete SKILL.md templates
- Integration section
- Testing section

### Templates

Skills use placeholder variables:
- `${TOOL_CONFIG}` - AI tool's config directory (`.claude/`, `.cursor/`, etc.)
- `${SPEC_DIR}` - Specification directory (`specs/`, `dev-docs/`, etc.)

These get customized per project via interview protocol.

### Artifact Naming

Skills produce artifacts with consistent naming:
- PRDs: `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`
- Plans: `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`
- Date prefix enables chronological sorting
- Feature name matches across artifacts

---

## Quality Standards

### Skill Templates Must Include

1. **Metadata header:**
   ```markdown
   ---
   name: skill-name
   description: One-line description
   disable-model-invocation: true
   argument-hint: "[expected argument]"
   ---
   ```

2. **When to Use section:**
   - Clear use cases
   - Explicit "Do NOT use when" guidance

3. **Input/Output specification:**
   - What the skill takes
   - What it produces

4. **Phase breakdown:**
   - Numbered phases
   - Clear gates between phases
   - Progress tracking in each phase

5. **Best practices section:**
   - Common patterns
   - Pitfalls to avoid

### Setup Guide Files Must Include

1. **Overview section:**
   - What skills are in this guide
   - When to use them
   - What they produce

2. **Phase 0: Repository Detection:**
   - Reference `setup-guidelines/repo-detection.md`
   - Suite-specific detection steps

3. **Phase 1: Prerequisites:**
   - Reference `setup-guidelines/prerequisites.md`
   - Suite-specific prerequisites

4. **Complete SKILL.md templates:**
   - Ready to copy and customize
   - All phases included
   - Progress tracking examples

5. **Integration section:**
   - How skills work together
   - Workflow diagrams
   - Data flow

6. **Testing section:**
   - How to validate skills work
   - Test scenarios

---

## Anti-Patterns

### Don't:

❌ **Skip Phase 0** - Always read documentation first
❌ **Skip Final Phase** - Always update documentation after
❌ **Hardcode paths** - Use `${TOOL_CONFIG}` and `${SPEC_DIR}` placeholders
❌ **Create nested folders** - Keep structure flat (3 main folders only)
❌ **Split related skills** - Keep orchestrator + atomic + review in ONE file
❌ **Skip progress tracking** - Always report current phase
❌ **Force reviews** - Make review gates optional, ask user
❌ **Assume conventions** - Read CLAUDE.md in Phase 0

### Do:

✅ **Read before starting** (Phase 0)
✅ **Report progress** (after each phase)
✅ **Update docs** (Final Phase)
✅ **Use placeholders** (`${TOOL_CONFIG}`, `${SPEC_DIR}`)
✅ **Keep structure flat** (setup-guidelines/, skill-guidelines/, agent-guidelines/)
✅ **Consolidate related skills** (one file per task category)
✅ **Ask about reviews** (don't force them)
✅ **Follow detected conventions** (from Phase 0)

---

## Summary

**Three core patterns:**
1. **Progress Tracking** - State variable + status log + visual indicator
2. **Documentation Maintenance** - Read Phase 0 → Follow → Update Final Phase
3. **Naming Conventions** - `{task}-{action}` pattern (with content creation exception)

**These patterns ensure:**
- Skills complete reliably (less likely to skip steps)
- Conventions stay current (no documentation drift)
- Skills adapt to projects (via customization interview)

**Apply to ALL skills in this framework.**
