---
name: bookkeep
description: >
  Use when records need to be updated to reflect reality. Triggers on:
  "update the docs", "sync the documentation", "record this", "log this change",
  "update the spec", "keep track of", "document what we did", "update the changelog",
  "make sure the docs reflect", "capture this decision".
argument-hint: "[what to record or sync]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit
---

# Bookkeep

**Goal:** Ensure records accurately reflect current reality — no drift between documentation and what actually exists.

**Intent:** Prevent confusion and errors caused by stale or inaccurate records. When documentation drifts from reality, people make decisions based on wrong information, duplicate work that was already done, or miss constraints that exist but aren't documented.

**Scope:** Keeping records accurate and in sync with reality. Includes: updating documentation to reflect code changes, recording decisions and their rationale, maintaining changelogs and version history, syncing specs with implementation, updating diagrams and architecture docs, and ensuring all written artifacts match current state. Bookkeeping is about accuracy of records — it answers "do our records match reality?" not "is the system healthy?"

---

## Preconditions

**Must be provided:**
- what changed: the change or state that needs to be recorded — ask if not clear
- where to record: which documents or records need updating — can often be inferred from project conventions

**Self-satisfiable:**
- current documentation state: read existing docs to understand what needs updating
- project conventions: understand where different types of information are recorded

**Non-essential:**
- change history: understanding what changed helps identify what to update
- related records: other documents that might reference the changed item

---

## Postconditions

**Success:**
- All relevant records are updated to reflect current reality
- No contradictions exist between documentation and implementation
- Decisions and rationale are captured for future reference
- Changes are traceable

**Failure:**
- Cannot determine what changed or what needs recording
- Cannot locate or access the records that need updating
- Conflicting information that cannot be resolved without user input

---

## Key Results

1. Documentation matches implementation — no drift
2. Decisions are recorded with rationale — not just what, but why
3. Changes are traceable — can follow the history
4. No contradictions exist between different records
5. Records are in the right place per project conventions

---

## Required Actions

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in documentation and record-keeping
would approach this task. Cover: their strategy, what records they would check first and why,
how they would ensure completeness (nothing missed), how they would avoid introducing
new inconsistencies, and what thorough bookkeeping looks like for this type of change.
This is not bookkeeping — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**
Before declaring done, verify against each key result:
- Does documentation match implementation?
- Are decisions recorded with rationale?
- Are changes traceable?
- Are there any remaining contradictions?

Report outcome explicitly: state whether bookkeeping is complete, and list what was updated.

---

## Possible Actions

Select and sequence based on context and expert reasoning. Others may be used.

**Discovery**
- **identify affected records**: determine which documents, specs, or logs need updating based on what changed
- **read current state**: understand what the records currently say
- **identify gaps**: find where records are missing or incomplete

**Recording Changes**
- **update specifications**: sync specs with implementation — what was designed vs. what was built
- **update documentation**: sync user-facing or developer docs with current behavior
- **record decisions**: capture what was decided and why — ADRs, design notes, comments
- **update changelog**: record what changed for version tracking
- **update diagrams**: sync visual representations with current architecture

**Ensuring Consistency**
- **check cross-references**: verify other docs that reference the changed item are still accurate
- **resolve contradictions**: when records conflict, determine which is correct and update the others
- **verify completeness**: ensure nothing was missed

**Organization**
- **follow conventions**: put records in the right place per project structure
- **link related records**: connect related documentation for discoverability
- **clean up obsolete records**: remove or archive records that no longer apply

---

## Use Cases

- After implementing a feature, updating specs and docs to match
- Recording an architectural decision
- Updating changelog before release
- Syncing documentation after refactoring
- Capturing meeting decisions or design discussions
- Updating API documentation after endpoint changes

---

## Tools
Write, Edit

## Hooks
None
