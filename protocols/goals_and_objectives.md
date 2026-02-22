# Goals and Objectives Protocol

Enable clear goal-setting and measurable outcomes through well-structured OKRs.

---

## Goal

Ensure artifacts have clear success criteria that are measurable and outcome-oriented.

---

## Intent

Without explicit goals and measurable outcomes, work drifts, success becomes subjective, and stakeholders misalign on expectations. OKRs (Objectives and Key Results) provide a structured approach to defining what success looks like before work begins, enabling objective assessment of whether goals were achieved.

---

## Scope

This protocol addresses goal-setting and OKR structure: defining clear objectives, writing measurable key results, and applying OKRs appropriately across different contexts (PRDs, technical plans, implementations).

---

## Key Results

- Goals are clear and focused (single objective per OKR)
- Key results are measurable (numbers, percentages, boolean outcomes)
- Key results are outcomes, not implementation steps
- OKRs match their context (user value for PRDs, technical feasibility for plans, code quality for implementations)
- Success can be objectively determined

---

## Essential Concepts

### OKR Structure

**OKR = Objective + Key Results**

**Objective:** Aspirational statement of what and why
- What are we trying to achieve?
- Why does it matter?
- Single, focused goal (not multiple things)

**Key Results:** Measurable, outcome-oriented metrics
- MUST be measurable (numbers, percentages, boolean)
- MUST be outcomes (not implementation steps)
- 3-5 key results (not too many)
- Each KR contributes to the objective

**Key Result Priority Levels:**
- **Required:** Must be achieved for success
- **Recommended:** Should be achieved unless user opts out
- **Optional:** User chooses whether to pursue (e.g., "if user requested")
- **Conditional:** Applies only when a condition is met (e.g., "existing repos only", "if tests exist")

### Outcomes vs Process vs Implementation Steps

Key Results fall into three categories:

**Outcome-oriented (preferred, primary):** Evaluates quality of what was produced
- "Login success rate >95%"
- "Zero security incidents in production"
- "Primitive is atomic — single cognitive action"
- "User satisfaction ≥4.5/5"

**Process-oriented (acceptable, secondary):** Confirms steps/activities completed
- "File exists at expected path"
- "Template sections present"
- "Protocol referenced in frontmatter"

**Implementation steps (avoid as KRs):** Tasks you complete
- "Create login endpoint"
- "Write email template"
- "Add database migration"

**Preference:** Primary Key Results should be outcome-oriented. Process-oriented KRs are acceptable as secondary/supporting results but should not be the main measure of success.

You can complete all implementation steps and still fail to achieve outcomes. A login endpoint can exist but have terrible performance. Tests can pass but miss critical edge cases.

### Context-Appropriate OKRs

Different contexts require different focus:

**PRD (Product Requirements)**
- Focus: User value and business outcomes
- Example KRs: User satisfaction scores, support ticket reduction, feature adoption rates

**Technical Plan**
- Focus: Technical feasibility and risk mitigation
- Example KRs: Performance targets, scalability limits, design review approval

**Implementation**
- Focus: Code quality and completeness
- Example KRs: Test coverage, requirements completion, code complexity metrics

**Refactoring**
- Focus: Quality improvement without behavior change
- Example KRs: Complexity reduction, coverage maintained, no regressions

**Bug Fix**
- Focus: Issue resolution with regression prevention
- Example KRs: Bug not reproducible, regression tests added, no new bugs

---

## Processes

### Writing Good OKRs

**1. Start with the outcome**

Ask: "If this succeeds, what will be different?"
- Users will be able to...
- Performance will improve to...
- Errors will decrease to...

**2. Make it measurable**

Ask: "How will I know if I succeeded?"
- Numbers: percentages, counts, times
- Comparisons: X% better than baseline
- Boolean: zero incidents, all tests pass

**3. Focus on value, not activity**

Transform activity into value:
- Activity: "Write 10 functions"
- Value: "Feature works correctly with 90% test coverage"

**4. Keep it simple**

Avoid compound objectives:
- Complex: "Improve the system's overall performance characteristics across multiple dimensions while maintaining backward compatibility and ensuring security compliance"
- Simple: "Response time <100ms with zero security incidents"

### Verification Checklist

Before finalizing OKRs:

- [ ] Objective is clear (can explain in one sentence)
- [ ] Objective is focused (one main thing, not multiple)
- [ ] 3-5 Key Results (not too few, not too many)
- [ ] Each KR is measurable (includes number or clear pass/fail)
- [ ] Each KR is outcome-oriented (not an implementation step)
- [ ] Each KR relates to objective (all KRs support the objective)
- [ ] Can determine success (can objectively say if each KR was met)
- [ ] Appropriate for context (PRD → user value, plan → feasibility, impl → quality)

---

## Patterns and Anti-Patterns

### Patterns

**Pattern 1: Outcome-Focused KRs**

Transform "what you'll do" into "what will be different":

```markdown
**Objective:** Enable users to reset passwords securely

**Key Results:**
1. Password reset completion rate >95%
2. Average reset time <2 minutes
3. Zero password reset security incidents
4. User satisfaction ≥4.5/5 for reset flow
```

Why it works: Each KR measures a user-facing outcome, not an implementation task.

**Pattern 2: Context-Appropriate Focus**

Match KRs to the artifact type:

```markdown
# PRD OKRs (user value)
**Objective:** Enable order tracking
**KRs:** Support tickets reduced 60%, satisfaction ≥4.5/5

# Plan OKRs (technical feasibility)
**Objective:** Design scalable tracking architecture
**KRs:** Handle 100k concurrent users, latency <500ms

# Implementation OKRs (code quality)
**Objective:** Implement high-quality tracking feature
**KRs:** 100% requirements met, 85% coverage, zero critical bugs
```

**Pattern 3: Measurable Thresholds**

Always include specific numbers:

```markdown
# Vague (hard to assess)
"Improve performance"

# Measurable (clear success criteria)
"P95 response time <100ms (down from 300ms)"
```

### Anti-Patterns

**Anti-Pattern 1: Implementation Steps as KRs**

```markdown
# Bad
**Key Results:**
1. Create login page
2. Add database table
3. Write tests

# Good
**Key Results:**
1. Login success rate >95%
2. Average login time <2 seconds
3. Zero authentication bugs in production
```

Why it fails: You can complete all tasks and still have a broken feature.

**Anti-Pattern 2: Non-Measurable KRs**

```markdown
# Bad
"Improve security", "Make it faster", "Better UX"

# Good
"Zero security incidents", "Response time <100ms", "User satisfaction ≥4/5"
```

Why it fails: Cannot objectively determine success.

**Anti-Pattern 3: Too Many KRs**

```markdown
# Bad: 10+ key results (diluted focus)

# Good: 3-5 key results (focused)
```

Why it fails: Too many KRs means none get proper attention.

**Anti-Pattern 4: Disconnected KRs**

```markdown
# Bad
Objective: Improve user onboarding
Key Results:
1. Database query time <10ms  # Not related
2. Test coverage >90%  # Not an outcome

# Good
Objective: Improve user onboarding
Key Results:
1. Onboarding completion rate >80%
2. Time to complete onboarding <5 minutes
3. Day-1 retention rate >70%
```

Why it fails: KRs don't actually measure the objective.

---

## Examples

### Example 1: Feature Development

**Context:** Password reset feature

**Bad OKR (implementation-focused):**
```markdown
Objective: Add password reset feature

Key Results:
1. Create reset password endpoint
2. Write email template
3. Add database migration
4. Write tests
```

**Good OKR (outcome-focused):**
```markdown
Objective: Enable users to securely reset their passwords

Key Results:
1. Password reset completion rate >95%
2. Average reset time <2 minutes
3. Zero password reset security incidents
4. User satisfaction score ≥4.5/5 for reset flow
```

### Example 2: Performance Improvement

**Context:** Application responsiveness

**Bad OKR:**
```markdown
Objective: Optimize the application

Key Results:
1. Add caching layer
2. Refactor database queries
3. Minify JavaScript
4. Use CDN
```

**Good OKR:**
```markdown
Objective: Improve application responsiveness for better user experience

Key Results:
1. P95 response time <100ms (down from 300ms)
2. Time to interactive <2 seconds on 3G connections
3. Zero timeout errors during peak hours
4. User-reported "slow performance" complaints reduced by 80%
```

### Example 3: Technical Plan

**Context:** Real-time order tracking architecture

```markdown
Objective: Design a scalable architecture for real-time order tracking

Key Results:
1. System can handle 100,000 concurrent users
2. Data latency <500ms from warehouse to customer
3. Zero single points of failure in design
4. Cost per order tracked <$0.01
5. Technical design review approved by senior engineers
```

---

## Key Takeaways

- **Outcomes, not tasks**: Key results measure what will be different, not what you'll do
- **Measurable always**: If you can't measure it, you can't assess success
- **Context matters**: PRDs focus on user value, plans on feasibility, implementations on quality
- **3-5 KRs**: Enough to be comprehensive, few enough to maintain focus
- **Single objective**: One clear goal per OKR, not multiple combined
- **Ask "so what?"**: Transform tasks into the outcomes they produce

---

## When to Apply This Protocol

- When defining requirements (PRD phase)
- When planning technical approach
- When starting implementation
- When defining refactoring goals
- When scoping bug fixes
- When setting project milestones
- When evaluating whether work succeeded
