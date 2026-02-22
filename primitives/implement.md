---
name: implement
description: >
  Use when writing code, tests, or other artifacts to realize a design. Triggers on:
  "implement this", "write the code", "build this", "code this up", "create the feature",
  "add the tests", "implement the design", "write the function", "make this work".
argument-hint: "[what to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Implement

**Goal:** Produce working code or artifacts that realize a design and meet the specified requirements.

**Intent:** Turn plans and designs into reality. Good implementation follows the design, fits project conventions, handles edge cases, and produces code that is correct, readable, and maintainable.

**Scope:** Writing and modifying code, tests, configuration, and other artifacts. Includes: translating designs into code, following project conventions and patterns, handling error cases and edge cases, writing tests alongside implementation, integrating with existing code, and iterating based on feedback. Implementation is about producing artifacts — it answers "does the code exist and work?" not "is it the right approach?"

---

## Key Results

1. Implementation matches the design and requirements
2. Code follows project conventions and is consistent with existing codebase
3. Edge cases and error conditions are handled appropriately
4. Code is readable — clear names, reasonable structure, appropriate comments where needed
5. Tests are written or updated as needed
6. Implementation integrates cleanly with existing code

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress during multi-file or long implementations
- `recovery.md` — Resume from interruption without losing work
- `reasoning.md` — Reason about implementation approach before writing code, verify against requirements after
- `goals_and_objectives.md` — Understand the success criteria being implemented against
- `modularity.md` — Produce modular code with clear boundaries and minimal coupling
- `quality.md` — Ensure code meets quality dimensions: correctness, clarity, maintainability
- `documentation.md` — Update documentation when implementation changes behavior or APIs

---

## Preconditions

**Must be provided:**
- what to implement: clear description of the feature, fix, or artifact — ask if not clear
- design or approach: how it should be built — if not provided, may need design first
- project conventions: understand the repo's patterns, style, and structure BEFORE writing any code

**Self-satisfiable:**
- existing codebase: understand what exists and how to integrate
- dependencies: understand what libraries, APIs, or modules are available
- prior implementation: if implementation already exists, read it as starting point

**Non-essential:**
- tests to satisfy: if existing tests define expected behavior, use them as specification
- examples: similar code in the codebase to use as reference

---

## Postconditions

**Success:**
- Code or artifacts exist that implement the specified functionality
- Implementation matches the design
- Code follows project conventions and patterns
- Edge cases and error conditions are handled
- Code is readable and maintainable
- Tests pass (if applicable)

**Failure:**
- Requirements are unclear — need definition first
- Design is missing or insufficient — need design first
- Implementation blocked by external factors (missing dependencies, access issues)
- Cannot satisfy requirements within existing constraints

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Preparation**
- **review conventions**: read existing code, style guides, CLAUDE.md to understand patterns, style, and structure
- **review design**: re-read the design to ensure the implementation matches intent
- **identify integration points**: understand where the new code connects to existing code
- **set up scaffolding**: create files, basic structure, imports before writing logic

**Design Feedback**
- **flag design issues**: if implementation reveals problems with the design, surface them before proceeding
- **propose design adjustments**: for minor issues, propose a light touch-up and confirm with user

**Implementation Techniques**
- **incremental implementation**: implement in small pieces, verifying each works before moving on
- **skeleton first**: write the structure before filling in logic
- **test-driven**: write a failing test first, then implement to make it pass
- **copy-adapt-modify**: find similar code in the codebase, copy it, adapt to the new context

**Core Implementation**
- **write core logic**: implement the main functionality
- **handle edge cases**: add handling for boundary conditions, empty inputs, invalid states
- **handle errors**: implement error handling, validation, and recovery
- **refactor as needed**: clean up code as you go
- **verify conventions after writing**: re-check that code follows project conventions

**Testing**
- **write tests**: create unit tests, integration tests as appropriate
- **run tests**: verify tests pass
- **test edge cases**: ensure edge case handling works correctly
- **test error paths**: verify error handling behaves as expected

**Integration**
- **integrate with existing code**: connect to existing modules, APIs, data flows
- **update dependencies**: modify imports, exports, configuration as needed
- **verify integration**: ensure the new code works within the larger system

**Quality**
- **review own code**: read through the implementation for clarity and correctness
- **check conventions**: verify code matches project style and patterns
- **clean up**: remove dead code, improve names, add comments where logic is non-obvious

---

## Confirm

Before declaring done, verify against each key result:
- Does the implementation match the design?
- Does it follow project conventions?
- Are edge cases handled?
- Do tests pass?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Use Cases

- Implementing a feature after design is complete
- Writing a bug fix
- Adding tests to existing code
- Creating a new module or component
- Implementing an API endpoint
- Writing a script or utility

---

## Tools

Write, Edit, Bash

## Hooks

None
