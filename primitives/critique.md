---
name: critique
description: >
  Use when reviewing work for quality, identifying issues, and suggesting improvements. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "critique this", "review this", "what's wrong with this", "how can I improve", "code review", "give me feedback", "what could be better", "find issues", "what am I missing", "red team this", "poke holes".
argument-hint: "[what to critique]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Critique

**Goal:** Identify issues and improvements in work by evaluating quality across multiple dimensions.

**Intent:** Surface problems before they cause harm. Good critique catches issues the creator missed — bugs, design flaws, unclear code, missing edge cases, security vulnerabilities.

**Scope:** Reviewing work for quality and identifying improvements. Includes finding bugs and logic errors, identifying design issues, evaluating code clarity and maintainability, checking security vulnerabilities, assessing adherence to conventions, and suggesting specific improvements with rationale. Critique is qualitative assessment — it answers "how good is this and how could it be better?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Issues are specific (exact location, what's wrong, why it matters) with actionable improvements, prioritized by severity
2. Overall quality grade assigned with evidence for multiple possible grades before deciding final grade

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Find evidence for multiple quality grades (excellent, good, fair, poor) before deciding on final grade
2. Each issue must have exact location and concrete improvement suggestion
3. Prioritize issues by severity (critical, non-blocking, suggestion) per `quality.md`
4. Track which aspects critiqued vs remain per `tracking.md`
5. Balance feedback — note strengths alongside weaknesses

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** What to critique (code, design, document, or artifact)

**Elicit if not provided:**
- Context (what work is supposed to do, project conventions, quality standards)
- Related code (surrounding code to understand integration and consistency)

**Optional:** Specific concerns to focus on, prior critiques

## Postconditions

The resulting state after the primitive is finished.

**Success:** Issues identified with locations and suggested improvements, prioritized by severity, overall quality grade with evidence.

**Failure:** Cannot access work to review, work too large to critique meaningfully, insufficient context to evaluate quality.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Review Work Systematically (→ KR1)

Check work against quality dimensions per `quality.md`:
- **Correctness:** bugs, logic errors, off-by-one errors, race conditions
- **Edge cases:** boundary conditions, empty inputs, null values, error conditions
- **Security:** injection vulnerabilities, authentication issues, data exposure, input validation
- **Performance:** inefficient algorithms, unnecessary work, memory leaks
- **Clarity:** unclear names, confusing logic, overly complex code
- **Maintainability:** tight coupling, code duplication, lack of tests, hard-to-extend structure
- **Conventions:** violations of project style, inconsistency with existing code

Use techniques: line-by-line review, structural review, trace execution, adversarial thinking, checklist review.

### Document Issues with Improvements (→ KR1)

For each issue found:
1. Locate specifically (file, line, section)
2. Explain the issue (what's wrong, why it matters)
3. Suggest improvement (concrete fix or alternative approach)
4. Categorize severity (critical, non-blocking, suggestion)

Note strengths alongside weaknesses for balanced feedback.

### Assess Overall Quality (→ KR2)

Find evidence for multiple quality grades:
1. Evidence for **Excellent** (why work could be considered excellent quality)
2. Evidence for **Good** (why work could be considered good quality)
3. Evidence for **Fair** (why work could be considered fair quality)
4. Evidence for **Poor** (why work could be considered poor quality)
5. Decide final grade based on weight of evidence

---

## Additional Notes and Terms

**Critique is qualitative assessment** — answers "how good is this?" not "does it meet criteria?" For objective verification, use `validate` primitive.

**Evidence-based grading** — REQ1 requires finding evidence for multiple grades before deciding. This prevents anchoring bias and ensures comprehensive quality assessment.
