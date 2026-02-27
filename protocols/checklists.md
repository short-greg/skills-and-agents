[TODO: Combine with tracking
I think this 
]

# Checklist Management Protocol

Dynamic checklist patterns for tracking workflow progress and adapting to discovered needs.

---

## Goal

Enable workflows to track progress reliably and adapt when reality differs from initial plans.

---

## Intent

Prevent workflows from skipping steps, losing track of progress, or becoming rigid. Checklists must be living documents that evolve during execution, not static todo lists.

---

## Scope

**Addresses:** Checklist structure (format, numbering), dynamic modification (add/remove items), conditional logic, loop-back patterns, progress tracking, Key Results → Tasks mapping

**Does not address:** Validation logic (see primitives), workflow design (see create_workflow), recovery mechanisms (see recovery protocol)

---

## Core Concepts

**Checklist components - evaluation criteria:**

| Component | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Format** | `<Skill> - KR<num> - <task>` provides full traceability | No KR mapping, tasks disconnected from objectives |
| **Dynamic adaptation** | Items added/removed as needs become clear | Static checklist despite changed requirements |
| **Progress visibility** | Current state obvious, one item `in_progress` at a time | No status updates, unclear what's done |
| **Numbering** | Sequential with letter suffixes (2a, 2b) for sub-items | Renumbering on changes breaks references |
| **Audit trail** | Removed items struck through with reason | Silent deletion loses context |

**Key principles:**
- Build from Key Results, not Tasks (ensures all success criteria covered)
- Use TodoWrite for real-time display, trace file for persistence (both needed)
- Conditionals explicit: `(If condition) Action`
- Loop-backs prevent duplication: "return to step N" vs "run tests again, run tests one more time"

---

## Techniques

**Building checklists:**

1. **List all Key Results** from workflow/skill (before starting work)
2. **Create preliminary items** - one per KR, task TBD if unclear: `- [ ] skill - KR1 - (clarify requirements)`
3. **Interview/investigate** to understand what's needed
4. **Add specific tasks** for each KR: `- [ ] skill - KR2 - Analyze repository structure`
5. **Verify coverage** - every Key Result has at least one task

**Dynamic modification:**

- **Add items:** Use letter suffixes to insert between numbers: `2a, 2b, 2c`. Mark `(NEW)` on first appearance.
- **Remove items:** Strike through with reason: `~~[ ] item~~ (REMOVED: out of scope)`
- **Expand items:** Add sub-items when scope becomes clear, parent remains as anchor
- **Conditionals:** `(If condition) Action` - execute if condition met, skip otherwise
- **Loop-backs:** `(If coverage <85%) Add tests and return to step 1` - prevents duplication

**Progress tracking:**

- Mark `[x]` when complete, keep exactly ONE `in_progress` at a time
- Update TodoWrite tool (if available) when status changes
- Update trace file after each item completes
- Record modifications in history with timestamp and reason

**Evaluation criteria:**
- Every Key Result mapped to at least one task?
- Format includes skill name and KR number for traceability?
- Items added/removed during execution with reasons documented?
- Loop-backs used instead of duplicating "retry" items?
- Progress visible in both TodoWrite and trace file?

---

## Use Cases and Triggers

Apply when:
- Implementing workflows - build checklist from Key Results
- Starting multi-step tasks - create preliminary checklist
- Discovering new requirements - add items dynamically
- Resuming after interruption - use checklist state to continue

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Key Results → Tasks Mapping:** Build checklist from KRs, not tasks. Prevents cherry-picking and orphaned objectives.

**Dynamic Expansion:** Start with high-level items, expand when scope becomes clear. Example: `2. Implement features (details TBD)` → `2a. Add authentication, 2b. Create dashboard, 2c. API endpoints`

**Loop-Back Pattern:** `- [ ] 1. Run tests` → `- [ ] 2. (If tests fail) Fix and return to step 1` instead of duplicating test steps.

**Audit Trail:** `~~[ ] Add caching layer~~ (REMOVED: out of scope per user)` preserves context.

### Anti-Patterns (❌)

**Static Checklists:** Requirements change but checklist doesn't adapt. **Fix:** Add/remove items as needs become clear, document why.

**Task-First Build:** Building from tasks alone misses Key Results. **Fix:** Start with all KRs, then map tasks to each.

**Duplicating Retry Items:** "Run tests, Run tests again, Run tests one more time". **Fix:** Use loop-back pattern.

**Silent Deletion:** Removing items without explanation. **Fix:** Strike through with reason, preserve audit trail.

---

## Examples

**Format:** `- [ ] setup-skill-env - KR2 - Analyze repository structure`

**Dynamic expansion:**
```markdown
Before: - [ ] 2. Implement features (details TBD)
After:  - [ ] 2. Implement features
          - [ ] 2a. Add authentication (NEW)
          - [ ] 2b. Create user dashboard (NEW)
```

**Conditional:** `- [ ] (If coverage <85%) Add tests for uncovered code`

**Loop-back:** `- [ ] 3. (If validation fails) Fix issues and return to step 2`

**Removal:** `~~[ ] Add caching layer~~ (REMOVED: out of scope per user)`

---

## References

- `protocols/tracking.md` - Trace file format for checklist persistence
- `protocols/recovery.md` - How checklists enable resumption after interruption
