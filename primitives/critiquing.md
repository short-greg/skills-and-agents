---
name: critiquing
description: >
  Evaluating and recommending a solution, approach, or process. Reviews work for quality, identifies issues, and suggests improvements.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "critique this", "review this", "what's wrong with this", "how can I improve",
  "code review", "give me feedback", "find issues", "red team this", "poke holes".
argument-hint: "[what to critique]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Critiquing

**Goal:** Identify issues and improvements by evaluating quality across multiple dimensions.

**Intent:** Surface problems before they cause harm. Good critique catches issues the creator missed — bugs, design flaws, security vulnerabilities, unclear code.

**Scope:** Reviewing work for quality and suggesting improvements. Answers "how good is this and how could it be better?"

---

## Key Results - KR

1. Issues identified with location, explanation, severity, and actionable improvement
2. Overall quality assessed with evidence for multiple grades before deciding

## Requirements and Constraints - REQ

1. Output evidence for multiple quality grades before deciding final grade
2. Each issue must have exact location and concrete improvement
3. Prioritize by severity (critical, non-blocking, suggestion)
4. Balance feedback — note strengths alongside weaknesses

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to find evidence for multiple grades before deciding
- **quality.md** - Must use for severity prioritization
- **tracking.md** - Must use to ensure systematic coverage
- **modularity.md** - Use when critiquing design or architecture
- **tracking.md** - Use when tracking which aspects critiqued vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** What to critique (code, design, document, artifact)

**Elicit if not provided:**
- Context (what work is supposed to do, project conventions)

**Optional:** Assessment procedure (rubric, heuristics, checklist), specific concerns, prior critiques

## Postconditions

**Success:** Issues identified with locations and improvements, prioritized by severity, quality grade with evidence

**Failure:** Cannot access work, work too large, insufficient context

---

## Actions

Select based on context. Each action shows which KR it serves.

### Determine Assessment Procedure (→ KR1, KR2)
Assessment method must be established before critiquing. If not provided, select or create based on what's being critiqued:
- **Code:** correctness, edge cases, security, performance, clarity, maintainability, conventions
- **UI/UX:** Nielsen's heuristics (visibility, match real world, user control, consistency, error prevention, recognition over recall, flexibility, aesthetics, error recovery, help)
- **Design/Architecture:** SOLID principles, coupling/cohesion, extensibility, testability
- **Document:** clarity, completeness, accuracy, audience-appropriateness
- **Custom:** derive criteria from stated goals and context

### Review Against Procedure (→ KR1)
Each criterion in the assessment procedure must be applied. Work through systematically, noting findings for each.

### Document Issues (→ KR1)
Each issue must be actionable. For each: locate specifically, explain what's wrong and why it matters, suggest concrete fix, assign severity.

### Note Strengths (→ KR1)
Feedback must be balanced. Identify what works well alongside issues.

### Gather Evidence for Each Grade (→ KR2)
Multiple hypotheses must be evaluated before judging. Find evidence for:
- Why work could be **Excellent**
- Why work could be **Good**
- Why work could be **Fair**
- Why work could be **Poor**

### Decide Final Grade (→ KR2)
Grade must be evidence-based. Weigh evidence across grades, decide based on strongest case.

---

## Additional Notes and Terms

**Critique vs Validate:** Critique is qualitative ("how good?"). Validate is objective ("does it meet criteria?").

---

## References

- [Google Engineering Practices - Code Review](https://google.github.io/eng-practices/review/)
- [Google - What to Look For](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [Nielsen Norman Group - Heuristic Evaluation](https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/)
- [Nielsen's 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
- [Better Evaluation - Evaluation Framework](https://www.betterevaluation.org/methods-approaches/methods/evaluation-framework)
- [NCBI - Critical Appraisal](https://www.ncbi.nlm.nih.gov/books/NBK603121/)
