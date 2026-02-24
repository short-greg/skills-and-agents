# Quality Protocol

Systematic quality assessment across complementary dimensions.

---

## Goal

Enable evaluation of artifacts across quality dimensions to identify strengths, weaknesses, and improvement opportunities.

---

## Intent

Quality is multi-dimensional—artifacts can excel in one dimension while failing in another. Without explicit criteria, assessments become subjective and miss critical issues. Quality assessment is multi-objective optimization with tradeoffs, not checklist compliance.

---

## Scope

**Addresses:** Quality assessment across correctness, clarity, reliability, completeness, efficiency, and testability for code, documentation, workflows, and designs

**Does not address:** Domain-specific quality standards, team processes, automated tooling configuration

---

## Core Concepts

**Quality dimensions - evaluation criteria:**

| Dimension | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Correctness** | Does what it should, edge cases handled, requirements met | Over-validation adds complexity without value |
| **Clarity** | Understandable quickly, obvious structure, descriptive names | Verbose explanations obscure the essentials |
| **Reliability** | Works consistently, handles failures gracefully, recovers from errors | Defensive programming adds complexity |
| **Completeness** | Covers necessary cases, addresses requirements, no critical gaps | Gold-plating solves problems that don't exist |
| **Efficiency** | Reasonable resource use for context, no obvious waste | Premature optimization obscures intent |
| **Testability** | Verifiable behavior, testable in isolation, observable outcomes | Test infrastructure more complex than code |

**Tradeoffs:**
- Correctness vs Efficiency: Validation adds overhead
- Clarity vs Efficiency: Optimizations obscure intent
- Completeness vs Simplicity: Edge cases add complexity
- Reliability vs Performance: Recovery mechanisms add latency

Balance based on: use case, failure modes, constraints, team knowledge.

---

## Techniques

**Quality assessment process:**

1. **Identify critical dimensions** - Which dimensions matter for this artifact's use case?
   - Production system: Correctness, Reliability
   - Library: Clarity, Testability, Completeness
   - Prototype: Correctness (others less critical)
   - Internal tool: Clarity

2. **Assess each dimension** - Use evaluation criteria from Core Concepts table

3. **Identify tradeoffs** - When dimensions conflict, make explicit choices:
   - State what you're optimizing
   - State what you're sacrificing
   - Document why (for future maintainers)

4. **Prioritize improvements:**
   - High: Correctness failures, reliability failures, critical missing functionality
   - Medium: Inefficiency in frequent operations, completeness gaps in common cases
   - Low: Inefficiency in rare operations, clarity in obvious code

**Evaluation criteria:**
- Are quality issues identified systematically (not accidentally)?
- Are tradeoffs explicit and intentional?
- Do improvement efforts focus on dimensions that matter?
- Are assessments objective (specific evidence, not feelings)?

---

## Use Cases and Triggers

Apply when:
- Designing new systems (identify critical dimensions)
- Reviewing code or documentation (assess systematically)
- Debugging quality issues (identify failing dimension)
- Refactoring (understand tradeoffs)
- Setting quality standards (decide priorities)

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Dimension-Appropriate Quality:** Optimize what matters for the use case. Production API needs correctness/reliability; prototype needs correctness but can defer others.

**Evidence-Based Assessment:** "Handles null case correctly" vs "This feels wrong". Use specific criteria, not feelings.

**Explicit Tradeoffs:** Document what you're optimizing and sacrificing. "Optimized for speed (reduced clarity) because profiling showed bottleneck."

**Progressive Quality:** Start with correctness, add dimensions as needed. Don't prematurely maximize all dimensions.

### Anti-Patterns (❌)

**Uniform Quality:** Trying to maximize all dimensions simultaneously. Results in over-engineering. **Fix:** Prioritize dimensions that matter.

**Premature Optimization:** Optimizing efficiency before measuring. **Fix:** Optimize when measurements prove it matters.

**Subjective Assessment:** "This doesn't feel right" without criteria. **Fix:** Use dimension-specific evaluation criteria.

**Quality as Checklist:** Treating quality as boxes to check. **Fix:** Think in terms of multi-objective optimization.

---

## Examples

### Example: REST API Endpoint

**Context:** Designing GET /users/{id} endpoint

**Assessment:**
- **Correctness:** ✅ Returns correct data, handles 404 | ❌ Missing pagination
- **Clarity:** ✅ Clear naming, obvious structure | ❌ Error codes undocumented
- **Reliability:** ✅ Handles DB failures, timeouts | ❌ No retry logic
- **Completeness:** ✅ Primary use case | ❌ Missing filtering, field selection
- **Efficiency:** ✅ Indexed queries | ❌ No caching (tradeoff: stale data)
- **Testability:** ✅ Unit/integration testable, observable behavior

**Result:** Prioritize pagination (completeness) and error docs (clarity). Defer caching until usage data shows need.

---

## References

- See `protocols/modularity.md` - Modularity assessment overlaps with quality
- [ISO 25010 Software Quality Model](https://iso25000.com/en/iso-25000-standards/iso-25010)
