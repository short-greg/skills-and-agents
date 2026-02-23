---
name: implement
description: >
  Use when writing code, tests, or other artifacts to realize a design. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "implement this", "write the code", "build this", "code this up", "create the feature", "add the tests", "implement the design", "write the function", "make this work".
argument-hint: "[what to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Implement

**Goal:** Produce working code or artifacts that realize design and meet specified requirements.

**Intent:** Turn plans and designs into reality. Good implementation follows design, fits project conventions, handles edge cases, and produces code that is correct, readable, and maintainable.

**Scope:** Writing and modifying code, tests, configuration, and other artifacts. Includes translating designs into code, following project conventions and patterns, handling error cases and edge cases, writing tests alongside implementation, integrating with existing code, and iterating based on feedback. Implementation answers "does the code exist and work?" not "is it the right approach?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Implementation matches design and requirements (follows conventions, handles edge cases, code is readable, tests written, integrates cleanly)
2. Tests pass (or failures documented with specifics)

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Track progress during multi-file or long implementations per `tracking.md`
2. Produce modular code with clear boundaries and minimal coupling per `modularity.md`
3. Ensure code meets quality dimensions: correctness, clarity, maintainability per `quality.md`
4. Update documentation when implementation changes behavior or APIs per `documentation.md`
5. Understand project conventions BEFORE writing any code (read existing code, style guides, CLAUDE.md)

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** What to implement, design or approach (how it should be built), project conventions (repo patterns, style, structure)

**Elicit if not provided:**
- Existing codebase (understand what exists and how to integrate)
- Dependencies (understand available libraries, APIs, modules)

**Optional:** Prior implementation (if exists, read as starting point), tests to satisfy, examples (similar code to use as reference)

## Postconditions

The resulting state after the primitive is finished.

**Success:** Code or artifacts exist that implement specified functionality. Implementation matches design. Code follows conventions and patterns. Edge cases and error conditions handled. Code is readable and maintainable. Tests pass.

**Failure:** Requirements unclear (need definition first), design missing or insufficient (need design first), implementation blocked by external factors (missing dependencies, access issues), cannot satisfy requirements within existing constraints.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Prepare and Implement (→ KR1)

**Preparation:** Review conventions (read existing code, style guides, CLAUDE.md). Review design (re-read to ensure implementation matches intent). Identify integration points (where new code connects to existing). Set up scaffolding (create files, basic structure, imports).

**Implementation:** Implement incrementally (small pieces, verifying each works). Write core logic. Handle edge cases (boundary conditions, empty inputs, invalid states). Handle errors (implement error handling, validation, recovery). Refactor as needed. Verify conventions after writing.

Flag design issues if implementation reveals problems. For minor issues, propose light touch-up and confirm with user.

### Test and Integrate (→ KR1, KR2)

**Testing:** Write tests (unit tests, integration tests as appropriate). Run tests to verify they pass. Test edge cases (ensure edge case handling works correctly). Test error paths (verify error handling behaves as expected).

**Integration:** Integrate with existing code (connect to existing modules, APIs, data flows). Update dependencies (modify imports, exports, configuration). Verify integration (ensure new code works within larger system).

**Quality:** Review own code (read through for clarity and correctness). Check conventions (verify code matches project style and patterns). Clean up (remove dead code, improve names, add comments where logic is non-obvious).

---

## Additional Notes and Terms

**Use cases:** Implementing feature after design is complete, writing bug fix, adding tests to existing code, creating new module or component, implementing API endpoint, writing script or utility.
