# Risk Management Protocol

Systematic techniques for identifying and mitigating complexity, uncertainty, and risk in workflows.

---

## Goal

Enable workflows to systematically identify and mitigate complexity, uncertainty, and risk before they cause failures.

---

## Intent

Workflows face three interrelated challenges: **complexity** (many parts, dependencies, intricate logic), **uncertainty** (unclear requirements or approach), and **risk** (potential for failure). Without systematic techniques, workflows fail, skip steps, or produce incorrect results. This protocol provides strategies to recognize and handle each challenge.

---

## Scope

**Addresses:** Identifying and mitigating complexity (essential vs accidental), uncertainty (requirements, technical, outcome), and risk (technical, requirements, process, quality, operational) in workflows. Includes risk assessment, monitoring, and mitigation strategies.

**Does not address:** Project management, team coordination, resource allocation, business risk assessment

---

## Core Concepts

**Complexity, Uncertainty, Risk - evaluation criteria:**

| Concept | Check for strength | Check for excess | Key relationship |
|---------|-------------------|------------------|------------------|
| **Complexity** | Decomposed into modules, dependencies explicit, <7 steps per module | Accidental complexity (nested conditionals, unclear dependencies) | Increases likelihood of errors, makes diagnosis harder |
| **Uncertainty** | Requirements clarified, approach validated, unknowns surfaced explicitly | Guessing/assuming instead of investigating, vague success criteria | Hidden risks lurk in unknowns |
| **Risk** | Assessed (impact × probability × detectability), mitigation strategy chosen, monitoring in place | Unidentified risks, no fail-fast, no quality gates | Increased by both complexity and uncertainty |

**Types:**

| Type | Complexity | Uncertainty | Risk |
|------|-----------|-------------|------|
| **Categories** | Essential (inherent to problem) vs Accidental (solution-introduced) | Requirements, Technical, Outcome | Technical, Requirements, Process, Quality, Operational |
| **Identification** | >7 steps, deep dependencies, conditionals, parallel execution, external deps | Vague requirements, "probably", "should work", missing success criteria | Side effects, external deps, novel approaches, irreversible actions |
| **Assessment** | Count steps, dependencies, conditionals, state space | Known vs unknown, assumptions made | Impact (low/med/high/critical), Probability (low/med/high), Detectability (easy/mod/hard) |

**Key relationships:**
- Complexity → Risk: More failure points, harder recovery
- Uncertainty → Risk: Unknown requirements/approaches increase failure potential
- Complexity + Uncertainty → High Risk: Most dangerous combination
- Strategy: Reduce uncertainty first (investigate), then simplify complexity, then mitigate remaining risk

---

## Techniques

**Risk management process (follows academic frameworks):**

1. **Identify** complexity, uncertainty, and risks in workflow design
2. **Assess** priority: High impact + High probability = Address immediately
3. **Mitigate** using appropriate strategies (see below)
4. **Monitor** during execution for unexpected issues
5. **Document** remaining risks and accepted tradeoffs

**Mitigation strategies:**

**For Complexity:**
- **Decomposition:** Break into sub-workflows (<7 steps each), single responsibility per module
- **Abstraction:** Hide complexity inside primitives with clear contracts
- **Simplification:** Remove unnecessary steps, eliminate conditionals where possible
- **Explicit dependencies:** Document data flow, order steps to minimize backtracking

**For Uncertainty:**
- **Investigation:** Use `investigate` primitive, read docs/code, surface unknowns explicitly
- **Spikes/Prototypes:** Time-boxed throwaway implementations to test feasibility
- **Defer commitment:** Use declarative workflows, avoid premature optimization
- **Clarify requirements:** Use `define` primitive for SMARB criteria, interview users
- **Escalate to user:** Surface decision points with options/tradeoffs (don't guess)

**For Risk:**
- **Avoidance:** Choose approach that eliminates risk (proven library vs custom)
- **Reduction:** Add validation, testing, quality gates to decrease probability/impact
- **Transfer:** Use external services for risky operations (managed DB vs self-hosted)
- **Acceptance:** Document risk, have mitigation plan ready if occurs
- **Fail-fast:** Order risky items first, surface failures before investing effort
- **Defensive measures:** Retry logic, compensation/rollback, timeouts, input validation
- **Quality gates:** Use `validate` and `critique` primitives between steps
- **Incremental approach:** Break into smaller steps, validate each (staging before production)
- **Monitoring:** Add logging/metrics for detectability

**Evaluation criteria:**
- Complexity, uncertainty, and risks identified during workflow design?
- Assessment done (complexity: step count/dependencies, uncertainty: knowns vs unknowns, risk: impact × probability)?
- Appropriate mitigation strategy chosen and applied?
- Uncertainty reduced before tackling complexity?
- High-impact risks addressed with fail-fast, quality gates, or defensive measures?
- Remaining risks documented with acceptance rationale?

---

## Use Cases and Triggers

Apply when:
- Designing adaptive workflows (identify upfront)
- Workflows have >6-7 steps (complexity management critical)
- External dependencies involved (APIs, services, databases)
- Requirements unclear or changing (systematic uncertainty reduction)
- Failures occur with complex root causes (diagnose and mitigate)
- Deciding between workflow approaches (assess tradeoffs)

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Uncertainty First:** Reduce uncertainty (investigate, clarify) before tackling complexity or risk. Hidden risks lurk in unknowns.

**Fail-Fast:** Order risky/uncertain items first. Surface failures early before investing effort.

**Quality Gates:** Validate outputs between steps using `validate` and `critique` primitives. Catch issues before they cascade.

**Decompose Complexity:** Break workflows >7 steps into modules with single responsibilities. Makes diagnosis easier.

**Explicit Tradeoffs:** Document accepted risks with rationale. "API might be slow, but timeout handling added."

### Anti-Patterns (❌)

**Guessing Under Uncertainty:** Assuming instead of investigating. **Fix:** Use `investigate` primitive, escalate to user.

**Accidental Complexity:** Nested conditionals, unclear dependencies. **Fix:** Simplify with lookup tables, make dependencies explicit.

**Ignoring Risk:** Proceeding without assessment. **Fix:** Assess impact × probability, apply appropriate mitigation.

**Late Validation:** Testing only at end. **Fix:** Quality gates between steps, incremental validation.

**Premature Optimization:** Locking in decisions under uncertainty. **Fix:** Defer commitment, use declarative workflows.

---

## Examples

**Complexity:** Workflow with 12 steps → Decompose into 3 modules of 4 steps each, add quality gates between modules.

**Uncertainty:** "Make it faster" requirement → Clarify: "Response time <200ms for 95th percentile under 1000 concurrent users"

**Risk assessment:** Database migration - Impact: Critical (data loss), Probability: Medium (untested schema) → Mitigation: Fail-fast (validate migration on copy first), defensive (automated backups, rollback plan)

**Fail-fast:** External API integration → Order: 1. Validate API access, 2. Test with sample data, 3. Build integration, 4. Full implementation (surface auth/compatibility issues early)

**Uncertainty + Complexity:** Novel algorithm in 10-step workflow → 1. Reduce uncertainty (spike to test algorithm feasibility), 2. Simplify (decompose into 3 modules), 3. Mitigate risk (quality gates, incremental validation)

---

## References

- `primitives/investigate.md` - Reducing uncertainty through exploration
- `primitives/define.md` - Clarifying requirements and success criteria
- `primitives/validate.md` - Quality gates and verification
- `primitives/critique.md` - Identifying quality issues
- [Risk Management Framework Components - MetricStream](https://www.metricstream.com/learn/risk-management-framework-and-components.html)
- [Understanding Risk Assessment Methodology - DataGuard](https://www.dataguard.com/blog/understanding-risk-assessment-methodology/)
