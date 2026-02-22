# Checklist Management Protocol

This protocol defines how to create and manage dynamic checklists in workflows and skills.

---

## Overview

Checklists are a primary mechanism for tracking progress in workflows. Unlike static todo lists, workflow checklists are **dynamic** — they can expand, contract, and adapt based on what's discovered during execution.

**Key principles:**
- Checklists are living documents that evolve during execution
- Items can be added, modified, or removed as needs become clear
- Progress must be visible and auditable
- Checklists enable resumption after interruption

---

## Tools

**For real-time display:** Use `TodoWrite` tool (if available) to show progress to the user
- Call TodoWrite at workflow start with all checklist items (status: `pending`)
- Update status to `in_progress` when starting an item
- Update status to `completed` when item is done
- Keep exactly ONE item `in_progress` at a time

**For persistence:** Use `Write`/`Edit` tools to maintain checklist in trace file
- Write to `${SPEC_DIR}/trace.md` or `${TASK_DIR}/trace.md`
- This enables recovery after compaction or session interruption
- See `tracking` protocol for trace file format

**Both are needed:** TodoWrite provides real-time visibility; trace file enables recovery.

---

## Purpose

**Goal:** Enable workflows to track progress reliably and adapt to discovered needs.

**Intent:** Prevent workflows from skipping steps, losing track of progress, or becoming rigid when reality differs from initial plans.

**Scope:** This protocol covers checklist structure, dynamic modification, progress tracking, and integration with the tracking protocol. It does NOT cover validation logic (see primitives) or workflow design (see create_workflow).

---

## Building Checklists from Key Results

**Key Results are the checklist source, not Tasks.**

Tasks are *how* you achieve Key Results. If checklists are built from Tasks alone, you can miss Key Results that aren't explicitly covered. Key Results are the success criteria — the checklist must ensure every one is achieved.

### Checklist Construction Process

1. **Start with Key Results** — List all Key Results from the skill/workflow
2. **Create preliminary checklist** — Before starting work (or interviewing), create a checklist with tasks that achieve each Key Result
3. **Map each Key Result → Task(s)** — Every Key Result must have at least one task that achieves it
4. **Verify coverage** — Check that no Key Result is orphaned (has no corresponding task)
5. **Add missing tasks** — If a Key Result has no task, create one
6. **Interview/discovery refines** — Update checklist based on what you learn (add, remove, reorder), but don't create from scratch

### Example: Key Results → Checklist Mapping

**Key Results:**
1. User requirements understood through interview
2. Repository structure analyzed
3. Framework components installed
4. Setup validated and working

**Preliminary Checklist (before interview):**
```markdown
- [ ] 1. Interview user about requirements (→ KR1)
- [ ] 2. Analyze repository structure (→ KR2)
- [ ] 3. Install framework components (→ KR3)
- [ ] 4. Validate setup works (→ KR4)
```

**After interview (refined):**
```markdown
- [x] 1. Interview user about requirements (→ KR1)
- [ ] 2. Analyze repository structure (→ KR2)
  - [ ] 2a. Run orient primitive
  - [ ] 2b. Create analysis report
- [ ] 3. Install framework components (→ KR3)
  - [ ] 3a. Install primitives
  - [ ] 3b. Install protocols
  - [ ] 3c. Install selected workflows
- [ ] 4. Validate setup works (→ KR4)
```

### Why This Matters

Without Key Results → Tasks mapping:
- AI may cherry-pick tasks it knows how to do
- Cognitive tasks (analyze, critique) get skipped in favor of mechanical tasks (mkdir, copy)
- Key Results get orphaned — no task achieves them
- Checklist becomes AI's interpretation, not workflow's requirements

With Key Results → Tasks mapping:
- Every Key Result is visible as a checklist item
- Omissions require conscious removal with justification
- Refinement is less error-prone than creation from scratch
- Audit trail shows which Key Results each task serves

---

## Checklist Structure

### Standard Format

**Basic pattern:**

```markdown
## Phase N: [Phase Name]

**Checklist:**
- [ ] 1. [First action]
- [ ] 2. [Second action]
- [ ] 3. [Third action]
```

**Elements:**
- **Numbered items** — enables referencing ("add item after step 2")
- **Checkbox format** — `- [ ]` for incomplete, `- [x]` for complete
- **Clear actions** — verb-first, specific, measurable
- **Phase context** — checklist belongs to a specific workflow phase

### Dynamic Items

Checklists can expand during execution. When a high-level item turns out to require multiple sub-tasks, add them dynamically:

**Initial checklist:**

```markdown
- [ ] 1. Analyze requirements
- [ ] 2. Implement features (details TBD)
- [ ] 3. Validate implementation
```

**After step 1, step 2 expands:**

```markdown
- [x] 1. Analyze requirements
- [ ] 2. Implement features
  - [ ] 2a. Add authentication
  - [ ] 2b. Create user dashboard
  - [ ] 2c. Implement API endpoints
- [ ] 3. Validate implementation
```

**Rules for dynamic items:**
- Use letter suffixes (2a, 2b, 2c) for sub-items
- Add sub-items when scope becomes clear, not before
- Parent item (2) remains as organizational anchor
- Sub-items are concrete and actionable

**When to use:**
- Workflow step that depends on earlier discovery
- Implementation phases where scope emerges from design
- Any step where "it depends" is the honest answer upfront

---

## Conditional Logic

### Implicit Conditionals

Some items only apply in certain contexts. Make this explicit:

```markdown
- [ ] 1. Run tests
- [ ] 2. (If production) Run security scans
- [ ] 3. (If coverage <85%) Add more tests
```

**Pattern:** `(If condition) Action`

**Execution:**
- Condition met → execute item
- Condition not met → skip item (no need to mark SKIP, condition explains it)

### Common Conditions

**Context-based:**
```markdown
- [ ] (If production repo) Full quality checks
- [ ] (If prototype) Basic functionality only
- [ ] (If library) API documentation required
```

**State-based:**
```markdown
- [ ] (If tests fail) Fix and retry
- [ ] (If coverage <threshold) Add tests
- [ ] (If vulnerabilities found) Address issues
```

**Change-based:**
```markdown
- [ ] (If breaking changes) Write migration guide
- [ ] (If new API) Add documentation
- [ ] (If database schema changed) Create migration
```

---

## Loop-Back Pattern

When validation or quality checks fail, loop back to fix and retry rather than duplicating checklist items:

**Pattern:**

```markdown
- [ ] 1. Run tests
- [ ] 2. Check coverage ≥85%
- [ ] 3. (If coverage <85%) Add tests and return to step 1
- [ ] 4. Commit changes
```

**How it works:**
- Step 3 conditionally triggers
- When triggered, unchecks and returns to step 1
- Avoids duplication: "Run tests", "Run tests again", "Run tests one more time"

**When to use:**
- Test → verify → fix if needed → re-test
- Build → validate → fix if needed → rebuild
- Any check-fix-retry cycle

---

## Progress Tracking

### Marking Progress

As work completes, mark items:

```markdown
**Before:**
- [ ] 1. Read requirements
- [ ] 2. Design approach
- [ ] 3. Implement

**After step 1:**
- [x] 1. Read requirements
- [ ] 2. Design approach (in progress)
- [ ] 3. Implement

**All done:**
- [x] 1. Read requirements
- [x] 2. Design approach
- [x] 3. Implement
```

### Reporting Progress

Workflows should periodically report checklist status:

```markdown
**Phase 3 Progress:**

Checklist: 5/8 items complete (62%)

✅ Completed:
- Read requirements
- Design approach
- Implement core logic

🔄 In progress:
- Add error handling

⏸️ Remaining:
- Write tests
- Run validation
- Update documentation
```

---

## Adding and Removing Items

### Adding Items

**When:** Discovery reveals new necessary work

**How:** Insert at appropriate position with clear numbering

```markdown
**Original:**
- [x] 1. Design API
- [ ] 2. Implement API
- [ ] 3. Test API

**After discovering auth requirement:**
- [x] 1. Design API
- [ ] 1a. Add authentication to design (NEW)
- [ ] 2. Implement API
- [ ] 3. Test API
```

**Pattern:**
- Use letter suffixes to insert between existing numbers
- Preserve existing item numbers (don't renumber)
- Mark new items as "(NEW)" on first appearance

### Removing Items

**When:** Item becomes irrelevant or was based on incorrect assumption

**How:** Mark as removed with brief explanation

```markdown
**Original:**
- [x] 1. Design API
- [ ] 2. Add caching layer
- [ ] 3. Implement API

**After deciding caching not needed:**
- [x] 1. Design API
- ~~[ ] 2. Add caching layer~~ (REMOVED: out of scope per user)
- [ ] 3. Implement API
```

**Pattern:**
- Strike through with `~~item~~`
- Add brief note: "(REMOVED: reason)"
- Don't delete — maintains audit trail

---

## Integration with Tracking Protocol

Checklists live in the trace file (see `tracking.md`):

**Trace file structure:**

```markdown
# Trace: Feature Workflow

## Current Phase: 3. Implement

### Phase 3 Checklist:
- [x] 1. Read design
- [x] 2. Set up project structure
- [ ] 3. Implement core logic (IN PROGRESS)
  - [x] 3a. Database models
  - [ ] 3b. API endpoints
  - [ ] 3c. Business logic
- [ ] 4. Write tests
- [ ] 5. Update documentation

## History

### 2025-01-15 14:30 - Started Phase 3
Checklist initialized with 5 items

### 2025-01-15 15:00 - Expanded item 3
Discovered need for 3 sub-components, added 3a-3c

### 2025-01-15 15:45 - Completed item 3a
Database models implemented and tested
```

**Rules:**
- Checklist appears under "Current Phase"
- History records checklist modifications
- Timestamps track progress
- Audit trail shows why items were added/removed

---

## Best Practices

### Keep Items Actionable

✅ **Good:**
```markdown
- [ ] Run `pytest tests/ --cov` and verify ≥85% coverage
- [ ] Update CHANGELOG.md with breaking changes
```

❌ **Bad:**
```markdown
- [ ] Do testing stuff
- [ ] Documentation
```

### Use Appropriate Granularity

✅ **Good:**
```markdown
- [ ] 1. Design authentication system
  - [ ] 1a. Choose auth method (JWT vs sessions)
  - [ ] 1b. Design token refresh flow
  - [ ] 1c. Plan session storage
```

❌ **Too granular:**
```markdown
- [ ] 1. Open design doc
- [ ] 2. Read design doc
- [ ] 3. Think about auth
- [ ] 4. Write down auth thoughts
```

### Make Conditions Specific

✅ **Good:**
```markdown
- [ ] (If coverage <85%) Add tests for uncovered code
```

❌ **Vague:**
```markdown
- [ ] (If needed) Maybe add some tests
```

### Order Logically

**Standard order:**
1. Prerequisites / context gathering
2. Main work
3. Verification / quality checks
4. Fixes (with loop-backs if needed)
5. Documentation / progress updates

```markdown
- [ ] 1. Read requirements (prerequisite)
- [ ] 2. Implement feature (main work)
- [ ] 3. Run tests (verification)
- [ ] 4. (If tests fail) Fix and return to step 3 (fix loop)
- [ ] 5. Update progress doc (documentation)
```

---

## Common Patterns

### The Discovery Pattern

When you don't know what work is needed until you investigate:

```markdown
- [ ] 1. Analyze codebase
- [ ] 2. Implement needed changes (expand after step 1)
```

After step 1:
```markdown
- [x] 1. Analyze codebase
- [ ] 2. Implement needed changes
  - [ ] 2a. Refactor auth module (discovered)
  - [ ] 2b. Update API contracts (discovered)
  - [ ] 2c. Fix integration tests (discovered)
```

### The Validation Pattern

Work → verify → fix if needed → re-verify:

```markdown
- [ ] 1. Implement feature
- [ ] 2. Run validation
- [ ] 3. (If validation fails) Fix issues and return to step 2
- [ ] 4. Mark complete
```

### The Incremental Pattern

Build in small pieces, verify each:

```markdown
- [ ] 1. Implement component A
- [ ] 2. Test component A
- [ ] 3. (If tests fail) Fix A and return to step 2
- [ ] 4. Implement component B
- [ ] 5. Test component B
- [ ] 6. (If tests fail) Fix B and return to step 5
- [ ] 7. Integration test
```

---

## Anti-Patterns to Avoid

### ❌ Static Checklists That Don't Adapt

**Problem:** Reality doesn't match initial plan, but checklist doesn't change

**Bad:**
```markdown
- [ ] 1. Add feature X
- [ ] 2. Add feature Y
- [ ] 3. Add feature Z
# (Later discover Y and Z aren't needed, but items remain)
```

**Good:**
```markdown
- [x] 1. Add feature X
- ~~[ ] 2. Add feature Y~~ (REMOVED: out of scope)
- ~~[ ] 3. Add feature Z~~ (REMOVED: duplicate of X)
```

### ❌ Duplicating Items Instead of Loop-Backs

**Bad:**
```markdown
- [ ] 1. Run tests
- [ ] 2. Fix tests
- [ ] 3. Run tests again
- [ ] 4. Fix tests again
- [ ] 5. Run tests one more time
```

**Good:**
```markdown
- [ ] 1. Run tests
- [ ] 2. (If tests fail) Fix and return to step 1
```

### ❌ No Audit Trail

**Bad:** Silently delete/modify items without explanation

**Good:** Strike through with reason, preserve history

---

## Summary

**Checklists are dynamic:**
- Add items when needs become clear
- Remove items when they become irrelevant
- Expand high-level items into concrete sub-tasks

**Checklists are auditable:**
- Mark progress as you go
- Record why items were added/removed
- Maintain in trace file with timestamps

**Checklists enable recovery:**
- Numbered items make resumption clear
- Current state visible at a glance
- History shows what was attempted

**Reference this protocol:**
- When creating workflows (`create_workflow.md`)
- When implementing tracking (`tracking.md`)
- When designing adaptive workflows that evolve during execution
