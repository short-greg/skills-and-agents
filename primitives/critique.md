---
name: critique
description: >
  Use when reviewing work for quality, identifying issues, and suggesting improvements.
  Triggers on: "critique this", "review this", "what's wrong with this", "how can I improve",
  "code review", "give me feedback", "what could be better", "find issues",
  "what am I missing", "red team this", "poke holes".
argument-hint: "[what to critique]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Critique

**Goal:** Identify issues, weaknesses, and opportunities for improvement in a piece of work.

**Intent:** Surface problems before they cause harm. Good critique catches issues that the creator missed — bugs, design flaws, unclear code, missing edge cases, security vulnerabilities, maintainability concerns. It provides actionable feedback, not just complaints.

**Scope:** Reviewing work for quality and identifying improvements. Includes: finding bugs and logic errors, identifying design issues, evaluating code clarity and maintainability, checking for security vulnerabilities, assessing adherence to conventions, and suggesting specific improvements with rationale. Critique is qualitative assessment — it answers "how good is this and how could it be better?" rather than "does this meet the specified criteria?"

---

## Key Results

1. Issues are specific — exact location, what's wrong, why it matters
2. Each issue has a suggested improvement — actionable, not just complaints
3. Issues are prioritized — critical vs. minor
4. Feedback is balanced — strengths noted alongside weaknesses
5. Critique is constructive — aimed at improvement, not just finding faults

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which aspects have been critiqued vs remain
- `recovery.md` — Resume critique from where it was interrupted
- `reasoning.md` — Reason about critique approach before starting, verify thoroughness after
- `goals_and_objectives.md` — Assess whether work achieves its intended outcomes
- `quality.md` — Evaluate against quality dimensions: correctness, clarity, maintainability
- `documentation.md` — Flag documentation issues discovered during critique

---

## Preconditions

**Must be provided:**
- what to critique: the code, design, document, or artifact being reviewed — ask if not clear

**Self-satisfiable:**
- context: understand what the work is supposed to do, project conventions, and quality standards
- related code: read surrounding code to understand integration and consistency

**Non-essential:**
- specific concerns: if the user has areas they want focus on, prioritize those
- prior critiques: if this has been reviewed before, focus on new or unresolved issues

---

## Postconditions

**Success:**
- Issues are identified with specific locations and descriptions
- Each issue includes a suggested fix or improvement
- Issues are prioritized by severity
- Strengths are noted alongside weaknesses (balanced feedback)

**Failure:**
- Cannot access the work to review
- Work is too large to critique meaningfully — needs scoping
- Insufficient context to evaluate quality

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Review Techniques**
- **line-by-line review**: read through the code carefully, noting issues as you go
- **structural review**: examine overall structure, organization, and architecture
- **trace execution**: mentally execute the code with different inputs
- **adversarial thinking**: actively try to break it — what inputs would cause failures?
- **checklist review**: systematically check against known issue categories

**Issue Categories to Check**
- **correctness**: bugs, logic errors, off-by-one errors, race conditions
- **edge cases**: boundary conditions, empty inputs, null values, error conditions
- **security**: injection vulnerabilities, authentication issues, data exposure, input validation
- **performance**: inefficient algorithms, unnecessary work, N+1 queries, memory leaks
- **clarity**: unclear names, confusing logic, missing comments where needed, overly complex code
- **maintainability**: tight coupling, code duplication, lack of tests, hard-to-extend structure
- **conventions**: violations of project style, inconsistency with existing code

**Feedback Formulation**
- **locate specifically**: identify exact file, line, or section
- **explain the issue**: what's wrong and why it matters
- **suggest improvement**: provide a concrete fix or alternative approach
- **prioritize**: distinguish critical issues from minor issues
- **note strengths**: acknowledge what's done well

---

## Confirm

Before declaring done, verify against each key result:
- Are issues specific with locations?
- Do issues have suggested fixes?
- Is feedback prioritized?
- Is feedback balanced?

Report outcome explicitly: state whether the critique is complete, and summarize key findings.

---

## Use Cases

- Code review before merging
- Reviewing a design before implementation
- Getting feedback on documentation
- Security review of sensitive code
- Improving code quality before release
- Self-review before submitting for others

---

## Tools

None

## Hooks

None
