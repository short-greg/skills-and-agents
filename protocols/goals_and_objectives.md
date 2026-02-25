# Goals and Objectives Protocol

Enable clear goal-setting and measurable outcomes through OKRs.

---

## Goal

Ensure artifacts have clear, measurable, outcome-oriented success criteria.

---

## Intent

Without explicit goals and measurable outcomes, work drifts, success becomes subjective, and expectations misalign. OKRs (Objectives and Key Results) define success before work begins, enabling objective assessment.

---

## Scope

**Addresses:** OKR structure (clear objectives, measurable key results), context-appropriate goals (PRDs, plans, implementations), outcome vs process vs implementation distinction

**Does not address:** Project management methodologies, team goal-setting processes, performance reviews

---

## Core Concepts

**OKR components - evaluation criteria:**

| Component | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Objective** | Clear, focused, single goal with purpose stated | Multiple goals combined, vague aspirations |
| **Key Results** | Outcomes measured (what changed), 3-5 total, context-appropriate | >5 KRs (diluted), implementation steps, disconnected from objective |
| **Evaluation type** | Subjective (judgment) OR objective (metrics), chosen appropriately | Gaming metrics, insufficient proxies, ignoring judgment |
| **Constraints** | Stated explicitly (time, resources, scope) | Overconstrained (no flexibility), underconstrained (drift) |

**Key Result types (preference order):**
1. **Outcomes** (preferred): What changed ("Success rate >95%", "User satisfaction ≥4/5", "Zero incidents")
2. **Leading indicators** (acceptable): Predictive measures (survey scores, engagement, early signals)
3. **Process** (last resort): Activities completed ("File exists", "Template present") - only when outcome unmeasurable

**Evaluation methods:**
- **Subjective**: Judgment-based (code quality, satisfaction, experience) - valid when expertise matters, numbers mislead
- **Objective**: Metric-based (counts, %, time) - valid when measurable, not gaming-prone
- **Hybrid**: Combine both (metrics + expert review) - often best, balances rigor with insight

**SMARB criteria for each key result:**
| Criterion | Check | Example |
|-----------|-------|---------|
| **Specific** | Unambiguous, one interpretation | "Response time <100ms" not "Fast response" |
| **Measurable** | Pass/fail verifiable | "Coverage >85%" not "Good coverage" |
| **Achievable** | Within scope and resources | Realistic given constraints |
| **Relevant** | Addresses the objective | Directly supports the goal |
| **Bounded** | Clear stopping condition | "By end of sprint" not "Soon" |

**Context-appropriate focus:**
- **PRD:** User outcomes (satisfaction, adoption, value delivered)
- **Plan:** Feasibility (performance, scalability, risk mitigation)
- **Implementation:** Code quality (coverage, complexity, requirements met)
- **Refactoring:** Quality gains (complexity down, no behavior change)
- **Bug Fix:** Resolution (not reproducible, regressions prevented)

---

## Techniques

**Writing OKRs:**

1. **Start with outcome** - "If this succeeds, what changes?" (users can X, performance is Y, errors are Z)
2. **Choose evaluation** - Subjective (judgment), objective (metrics), or hybrid based on what's measurable without gaming
3. **Make measurable** - Numbers (>95%, <100ms), comparisons (50% better), boolean (zero incidents), or judgment (expert review)
4. **State constraints** - Time (by when), resources (with what), scope (what's excluded)
5. **Keep focused** - One objective, 3-5 key results that directly support it

**Evaluation criteria:**
- Objective clear (one sentence explanation)?
- KRs are outcomes (what changed), not implementation steps (what you'll do)?
- Each KR passes SMARB (Specific, Measurable, Achievable, Relevant, Bounded)?
- Evaluation method appropriate (won't cause gaming, captures what matters)?
- Constraints explicit (time, scope, resources)?
- Context-appropriate (PRD→user value, plan→feasibility, impl→quality)?

---

## Use Cases and Triggers

Apply when:
- Defining requirements (PRD phase)
- Planning technical approach
- Starting implementation
- Setting refactoring goals
- Scoping bug fixes
- Evaluating whether work succeeded

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Outcome-focused:** Transform "what you'll do" into "what will change". Example: Not "Create login page" but "Login success >95%, avg time <2s, zero auth bugs".

**Subjective when appropriate:** "Code quality meets team standards (expert review)" valid when metrics mislead. Combine with objective: "Coverage >85% AND passes code review".

**Constraints explicit:** "Response <100ms (within 2 weeks, using current infrastructure, excluding admin panel)". Prevents scope creep.

**Context-appropriate:** PRD→user value (satisfaction, adoption). Plan→feasibility (performance, scale). Impl→quality (coverage, complexity).

### Anti-Patterns (❌)

**Implementation steps as KRs:** "Create X, write Y, add Z" - can finish all and still fail. **Fix:** "Success rate >95%, time <2s, zero bugs".

**Insufficient proxies:** Test coverage ≠ quality. Lines of code ≠ value. **Fix:** Use subjective judgment or multiple indicators.

**Gaming metrics:** Optimizing for metric, not outcome. **Fix:** Hybrid evaluation (metrics + judgment).

**Too many KRs:** >5 dilutes focus. **Fix:** 3-5 key results max.

---

## Examples

**Bad (implementation):** "Add password reset. KRs: 1) Create endpoint 2) Write email template 3) Add DB migration 4) Write tests"

**Good (outcome):** "Enable secure password reset. KRs: 1) Completion rate >95% 2) Avg time <2min 3) Zero security incidents 4) Satisfaction ≥4/5"

**Subjective evaluation:** "Improve code quality. KRs: 1) Complexity reduced (expert review: satisfactory) 2) Coverage >85% 3) No regressions (tests pass)"

**With constraints:** "Improve response time (within Q1, using current infrastructure, excluding batch jobs). KRs: 1) P95 <100ms 2) Zero timeouts 3) Cost unchanged"

**Context-appropriate:**
- PRD: "Order tracking. KRs: Tickets -60%, satisfaction ≥4/5, adoption >50%"
- Plan: "Scalable tracking. KRs: 100k users, <500ms latency, zero SPOFs"
- Impl: "Quality tracking. KRs: 100% requirements, 85% coverage, zero critical bugs"

---

## References

- [OKRs: The Ultimate Guide - Atlassian](https://www.atlassian.com/agile/agile-at-scale/okr)
- [What are OKRs? Guide - Asana](https://asana.com/resources/okr-meaning)
- [OKRs: The Only Guide You Need - Mooncamp](https://mooncamp.com/okr)
- [Practical Guidance on Developing Indicators - CRS](https://www.crs.org/sites/default/files/2025-03/indicator_guidance_final_low_res.pdf)
