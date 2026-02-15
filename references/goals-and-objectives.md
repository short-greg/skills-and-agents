# Goals and Objectives (OKRs)

How to set clear goals and measurable objectives (OKRs) for features, plans, and implementations.

---

## Overview

Good goals and OKRs ensure everyone understands what success looks like. They should be:
- **Clear:** Unambiguous what you're trying to achieve
- **Measurable:** You can tell if you succeeded
- **Outcome-oriented:** Focus on results, not implementation steps

---

## What Makes a Good Goal

**A goal is a single sentence stating what to accomplish:**

✅ **Good Goals:**
- "Enable users to reset their passwords securely"
- "Improve application response time"
- "Add export functionality for user data"

❌ **Poor Goals:**
- "Add code" (too vague)
- "Make it better" (not specific)
- "Implement 5 new features and refactor 3 modules" (too many things)

**Characteristics of good goals:**
- Single, focused objective
- Clear and specific
- Achievable within the scope
- States the "what" not the "how"

---

## OKR Structure

**OKR = Objective + Key Results**

### Objective

**Aspirational statement of the "what" and "why":**
- What are we trying to achieve?
- Why does it matter?
- Should inspire and motivate

**Format:** "Enable [who] to [do what] [how/why]"

### Key Results

**Measurable, outcome-oriented metrics:**
- **MUST be measurable** (numbers, percentages, boolean outcomes)
- **MUST be outcomes** (not implementation steps)
- **3-5 key results** (not too many)
- Each KR should contribute to the objective

**Format:** "[Metric] reaches/achieves [target]"

---

## Examples

### Example 1: Password Reset Feature

✅ **Good OKR (Outcome-oriented):**

```markdown
**Objective:** Enable users to securely reset their passwords

**Key Results:**
1. Password reset completion rate >95%
2. Average reset time <2 minutes
3. Zero password reset security incidents
4. User satisfaction score ≥4.5/5 for reset flow
```

**Why this is good:**
- Objective is clear and focused
- KRs are all measurable numbers
- KRs focus on outcomes (completion rate, time, security, satisfaction)
- Can objectively determine if succeeded

❌ **Bad OKR (Implementation-focused):**

```markdown
❌ **Objective:** Add password reset feature

❌ **Key Results:**
1. Create reset password endpoint  # Implementation step
2. Write email template  # Implementation step
3. Add database migration  # Implementation step
4. Write tests  # Implementation step
```

**Why this is bad:**
- "Key Results" are just TODO items
- Not measurable outcomes
- Doesn't tell you if you succeeded (endpoint exists, but does it work well?)
- Doesn't focus on user value

---

### Example 2: Performance Improvement

✅ **Good OKR (Outcome-oriented):**

```markdown
**Objective:** Improve application responsiveness for better user experience

**Key Results:**
1. P95 response time <100ms (down from 300ms)
2. Time to interactive <2 seconds on 3G connections
3. Zero timeout errors during peak hours
4. User-reported "slow performance" complaints reduced by 80%
```

**Why this is good:**
- Specific performance targets
- Measurable improvements
- Focused on user experience outcomes
- Clear success criteria

❌ **Bad OKR (Implementation-focused):**

```markdown
❌ **Objective:** Optimize the application

❌ **Key Results:**
1. Add caching layer  # How, not what
2. Refactor database queries  # How, not what
3. Minify JavaScript  # How, not what
4. Use CDN  # How, not what
```

**Why this is bad:**
- KRs are implementation steps (the "how")
- Not measurable outcomes
- Could do all these things and performance might not improve
- Doesn't define success

---

### Example 3: API Development

✅ **Good OKR (Outcome-oriented):**

```markdown
**Objective:** Provide a stable, developer-friendly API for third-party integrations

**Key Results:**
1. API uptime ≥99.9%
2. Average API response time <50ms
3. Zero breaking changes without 3-month deprecation notice
4. Time-to-first-successful-call <15 minutes for new developers
5. API satisfaction score ≥4.5/5 from developer survey
```

**Why this is good:**
- Measurable reliability and performance targets
- Outcome-focused (stability, speed, ease of use)
- Considers developer experience
- Clear success metrics

❌ **Bad OKR (Implementation-focused):**

```markdown
❌ **Objective:** Build an API

❌ **Key Results:**
1. Design REST endpoints  # Implementation step
2. Add authentication  # Implementation step
3. Write API documentation  # Implementation step
4. Create SDK  # Implementation step
```

---

## Anti-Patterns

### Anti-Pattern 1: Implementation Steps as KRs

❌ **Bad:** "Create login page, add database table, write tests"
✅ **Good:** "Login success rate >95%, average login time <2 seconds"

**Why:** Implementation steps are tasks, not outcomes. You can complete all the tasks and still have a terrible feature.

### Anti-Pattern 2: Non-Measurable KRs

❌ **Bad:** "Improve security", "Make it faster", "Better UX"
✅ **Good:** "Zero security incidents", "Response time <100ms", "User satisfaction ≥4/5"

**Why:** Can't objectively determine if you succeeded without measurements.

### Anti-Pattern 3: Too Many KRs

❌ **Bad:** 10+ key results
✅ **Good:** 3-5 key results

**Why:** Too many KRs dilute focus. Pick the most important outcomes.

### Anti-Pattern 4: KRs Disconnected from Objective

❌ **Bad:**
```markdown
Objective: Improve user onboarding
Key Results:
1. Database query time <10ms  # Not related to onboarding
2. Test coverage >90%  # Not an outcome
```

✅ **Good:**
```markdown
Objective: Improve user onboarding
Key Results:
1. Onboarding completion rate >80%
2. Time to complete onboarding <5 minutes
3. Day-1 retention rate >70%
```

**Why:** Every KR should directly relate to the objective.

---

## Context-Specific Examples

### For PRD (feature-define)

**Focus on user outcomes and business metrics:**

```markdown
**Objective:** Enable customers to track their order status in real-time

**Key Results:**
1. Customer "where's my order?" support tickets reduced by 60%
2. Order status page views >10,000/day
3. Customer satisfaction with order visibility ≥4.5/5
4. Average time to find order status <30 seconds
```

### For Technical Plan (feature-plan)

**Focus on technical feasibility and risk mitigation:**

```markdown
**Objective:** Design a scalable architecture for real-time order tracking

**Key Results:**
1. System can handle 100,000 concurrent users
2. Data latency <500ms from warehouse to customer
3. Zero single points of failure in design
4. Cost per order tracked <$0.01
5. Technical design review approved by senior engineers
```

### For Implementation (feature-impl)

**Focus on code quality and technical metrics:**

```markdown
**Objective:** Implement high-quality, maintainable order tracking feature

**Key Results:**
1. All PRD requirements implemented (100% complete)
2. Test coverage ≥85% with all tests passing
3. Zero critical or high-severity bugs in testing
4. Code complexity <10 per function (radon cc grade A-B)
5. Code review approved by tech lead
```

---

## Tips for Writing Good OKRs

### Start with the Outcome

**Ask:** "If this succeeds, what will be different?"
- Users will be able to...
- Performance will improve to...
- Errors will decrease to...

### Make It Measurable

**Ask:** "How will I know if I succeeded?"
- Numbers: percentages, counts, times
- Comparisons: X% better than baseline
- Boolean: zero incidents, all tests pass

### Focus on Value, Not Activity

**Bad:** "Write 10 functions" (activity)
**Good:** "Feature works correctly with 90% test coverage" (value)

### Keep It Simple

**Bad:** "Improve the system's overall performance characteristics across multiple dimensions while maintaining backward compatibility and ensuring security compliance"
**Good:** "Response time <100ms with zero security incidents"

---

## Verification Checklist

Before finalizing OKRs, check:

- [ ] **Objective is clear:** Can explain it in one sentence
- [ ] **Objective is focused:** One main thing, not multiple things
- [ ] **3-5 Key Results:** Not too few, not too many
- [ ] **Each KR is measurable:** Includes a number or clear pass/fail
- [ ] **Each KR is outcome-oriented:** Not an implementation step
- [ ] **Each KR relates to objective:** All KRs support the objective
- [ ] **Can determine success:** Can objectively say if each KR was met
- [ ] **Appropriate for context:** PRD focuses on user value, plan on feasibility, impl on quality

---

## Common Contexts

### PRD (Product Requirements Document)

**Objective:** User value and business outcomes
**Key Results:** User metrics, business metrics, satisfaction scores

### Technical Plan

**Objective:** Technical feasibility and architecture
**Key Results:** Performance targets, scalability, risk mitigation, design approval

### Implementation

**Objective:** Code quality and completeness
**Key Results:** Requirements met, test coverage, code quality metrics, review approval

### Refactoring

**Objective:** Improve code quality without changing behavior
**Key Results:** Complexity reduction, test coverage maintained, no regressions, performance improved

### Bug Fix

**Objective:** Resolve specific issue reliably
**Key Results:** Bug no longer reproducible, tests prevent regression, no new bugs introduced

---

## Summary

**Good OKRs are:**
- **Clear:** Everyone understands what success looks like
- **Measurable:** Can objectively determine if succeeded
- **Outcome-oriented:** Focus on results, not tasks
- **Achievable:** Realistic within scope
- **Valuable:** Meaningful outcomes that matter

**Remember:**
- Implementation steps ≠ Key Results
- "Better/Improved/Enhanced" ≠ Measurable
- 10+ KRs = Unfocused
- Ask "So what?" to find outcomes behind tasks

**Skills should reference this document:**
- `[See references/goals-and-objectives.md for OKR guidance]`
- Link to specific sections for context-appropriate examples
