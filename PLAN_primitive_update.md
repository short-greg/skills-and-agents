# Plan: Primitive Update

Temporary plan for updating primitives to match current protocols and template.

---

## Context

Protocols have been updated/renamed. Primitives reference old protocol names and are missing template sections. The primitive_creation_guideline.md also has issues.

---

## Phase 1: Fix Supporting Files

### 1.1 Create primitives/base.md ✅ DONE

Created `primitives/base.md` — execution pattern all primitives inherit.
Removed `protocols/primitive_execution.md` — was misplaced as protocol.

Key decisions:
- Inline tracking per `tracking_and_recovery.md` (not file-based)
- Lightweight checklist derived from KRs
- Must output explicit success/failure result

### 1.2 Fix primitive_creation_guideline.md

| Line | Issue | Fix |
|------|-------|-----|
| 29 | References deleted `critiquing` | Change to `evaluating` |
| 33 | `[TODO: Explain]` incomplete | Complete or remove |
| 50 | Says "protocol" instead of "primitive" | Fix typo |
| 38 vs 121 | "Exactly 2 KRs" vs "2 or 3 KRs" contradiction | Decide: recommend 2 |
| 87, 182 | References `primitive_execution.md` | Change to `base.md` |
| 186-190, 227-229 | Duplicate Terms sections | Consolidate into one |
| 243 | Example references `reasoning` | Change to `thinking` |
| 26-30 | Only 4 example primitives | Update list to current primitives |
| 60-64 | Decision table missing protocol list | Add list of available protocols |

---

## Phase 2: Update All Primitives

### 2.1 Protocol Reference Fixes (ALL primitives)

| Old Reference | New Reference |
|---------------|---------------|
| `reasoning.md` | `thinking.md` |
| `tracking.md` | `tracking_and_recovery.md` |
| `recovery.md` | `tracking_and_recovery.md` |
| `documentation.md` | `transparency.md` |
| `goals_and_objectives.md` | `goal_setting.md` |
| `modularity.md` | `system_modularity.md` |
| `quality.md` | `software_quality.md` |
| `instructions.md` | `instruction_giving.md` |

### 2.2 Add Missing Sections (ALL primitives) ✅ DONE

All primitives now have:
- [x] `## Steps` section referencing `base.md`
- [x] Keywords in frontmatter (where applicable)

### 2.3 Individual Primitive Fixes

| Primitive | Issues | Status |
|-----------|--------|--------|
| orienting.md | Protocol refs, Steps section, added interviewing.md | ✅ DONE |
| investigating.md | Protocol refs, Steps section, added interviewing.md | ✅ DONE |
| defining.md | Protocol refs, Steps section, added thinking.md, criteria_setting.md, interviewing.md | ✅ DONE |
| designing.md | Protocol refs, Steps section, clarified action mappings | ✅ DONE |
| implementing.md | Protocol refs, Steps section, keywords | ✅ DONE |
| evaluating.md | Protocol refs, Steps section, reduced to 2 KRs | ✅ DONE |
| brainstorming.md | Protocol refs, Steps section | ✅ DONE |
| planning.md | Protocol refs, Steps section, keywords, added goal_setting.md | ✅ DONE |
| maintaining.md | Protocol refs, Steps section, added thinking.md | ✅ DONE |

---

## Phase 3: Consider New Protocols ✅ DONE

Protocol decisions made via rigorous protocol → action mapping:

| Protocol | Decision | Used By |
|----------|----------|---------|
| `criteria_setting.md` | ✅ Added | defining.md (Specify Acceptance Criteria) |
| `interviewing.md` | ✅ Added | defining.md (Elicit Requirements), orienting.md (Locate Information), investigating.md (Assess Uncertainty) |
| `pragmatics.md` | ✅ Added | defining.md (Elicit Requirements, Confirm with User), brainstorming.md (Describe Each Idea, Group and Label), evaluating.md (Output Evaluation, Provide Recommendations), planning.md (Confirm with User), investigating.md (Synthesize Findings and Recommend) |
| `frame_of_mind.md` | ❌ Deferred | Workflows only — Option B: workflows set roles, primitives execute |

Key insight:
- pragmatics.md belongs in primitives with user-facing communication actions (directness calibration, confidence signaling, framing, option presentation)
- frame_of_mind.md belongs in workflows because setting roles/mindsets is an orchestration-level concern

---

## Execution Order

1. ✅ Create `primitives/base.md` (dependency for all primitives)
2. Fix `primitive_creation_guideline.md` (reference during updates)
3. Update primitives one by one, validating each against guideline
4. Delete or populate `maintenance.md`

---

## Lightweight Checklist Requirement

Primitives should create lightweight checklists by default. This should be:
- Specified in `primitive_execution.md` step 2
- Format: Simple markdown checklist derived from KRs
- Purpose: Track progress without overhead

Example format:
```markdown
## Checklist
- [ ] KR1: [description]
- [ ] KR2: [description]
```

---

## Status

- [x] Phase 1.1: Create primitives/base.md
- [x] Phase 1.2: Fix primitive_creation_guideline.md
- [x] Phase 2: Update primitives (9/9 done)
- [x] Phase 3: Consider new protocol references
