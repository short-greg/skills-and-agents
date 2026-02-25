---
name: bookkeeping
description: >
  Keeps records in sync with reality. Use when documentation drifts from implementation,
  decisions need recording, or changelogs need updating.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "update the docs", "sync the documentation", "record this", "log this change",
  "update the spec", "document what we did", "update the changelog", "capture this decision".
argument-hint: "[what to record or sync]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit
---

# Bookkeeping

**Goal:** Ensure records accurately reflect current reality — no drift between documentation and what exists.

**Intent:** Prevent confusion from stale records. When documentation drifts, people make decisions on wrong information or duplicate existing work.

**Scope:** Keeping records accurate: updating docs to reflect code changes, recording decisions with rationale, maintaining changelogs, syncing specs with implementation. Answers "do our records match reality?"

---

## Key Results - KR

1. Records accurately reflect reality — documentation matches implementation, no contradictions, correct locations per conventions
2. Changes are traceable with rationale — what changed, why, and history preserved

## Requirements and Constraints - REQ

1. Read existing records before updating
2. Follow project conventions for record locations
3. Preserve history — don't overwrite without context

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **documentation.md** - Must use for updating documentation
- **tracking.md** - Use when tracking progress across multiple records
- **checklists.md** - Must use to ensure completeness of updates
- **recovery.md** - Use when resuming after interruption
- **reasoning.md** - Use when resolving contradictions between records

---

## Preconditions

**Required:** What changed — the change or state that needs recording

**Elicit if not provided:**
- Where to record — which documents need updating (often inferred from conventions)

**Optional:** Change history, related records that might reference the changed item

## Postconditions

**Success:** All relevant records updated, no contradictions, changes traceable

**Failure:** Cannot determine what changed, cannot locate records, unresolvable conflicts

---

## Actions

Select based on context. Each action shows which KR it serves.

### Identify Affected Records (→ KR1)
All impacted records must be identified. Determine which docs, specs, or logs need updating.

### Read Current State (→ KR1)
Record content must be known. Before updating, review what records say.

### Update Documentation (→ KR1)
Docs must match current behavior. Sync with implementation.

### Update Specifications (→ KR1)
Specs must match what was built. Sync design docs with implementation.

### Record Decisions (→ KR2)
Decisions must be preserved with rationale. Capture what, why, and alternatives considered.

### Update Changelog (→ KR2)
Changes must be version-tracked. Record what changed.

### Check Cross-References (→ KR1)
Referencing docs must remain accurate. Verify other docs that reference the updated item.

### Resolve Contradictions (→ KR1)
Records must not conflict. Determine which is correct and update the others.

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Write the Docs - Documentation Maintenance](https://www.writethedocs.org/guide/)
