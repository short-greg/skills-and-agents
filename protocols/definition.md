# Definition Protocol

**Type:** Protocol

Formulating rigorous definitions for vague or ambiguous criteria.

---

## Goal

Convert vague criteria into observable, measurable conditions with unambiguous verification methods.

---

## Intent

Vague criteria like "fast", "good quality", or "user-friendly" cannot be validated. Rigorous definitions enable objective verification.

---

## Scope

**Addresses:** Converting vague criteria into rigorous definitions (binary, fuzzy, categorical), specifying observable behavior, thresholds, verification methods

**Does not address:** Validation against criteria (use `validation.md`), requirement elicitation (use `defining` primitive)

---

## Techniques

### Rigorous Definition Template

A criterion is rigorous when it specifies:
1. **Observable behavior or state** - What can be measured or observed
2. **Unambiguous threshold or condition** - Clear pass/fail or measurement target
3. **Method to verify** - How to check if criterion is met

### Definition Types

**Binary (pass/fail):**
- Vague: "Form should work"
- Rigorous: "Submit button has `disabled` attribute when any required field is empty"

**Fuzzy (scale):**
- Vague: "API should be fast"
- Rigorous: "API response time < 200ms for 95th percentile under 100 concurrent users"

**Categorical (levels):**
- Vague: "Security should be good"
- Rigorous: "Security risk level (low/medium/high) based on OWASP Top 10 coverage"

### Steps to Formulate

1. **Identify vague terms** - "fast", "good", "user-friendly", "should work"
2. **Ask what observable** - What behavior or state indicates this?
3. **Specify threshold** - What's the cutoff? What value is acceptable?
4. **Define verification method** - How do we measure or check this?
5. **Output rigorous definition** - Document the full specification

---

## Use Cases

- Converting user requirements into testable acceptance criteria
- Making design goals measurable
- Establishing objective success criteria before implementation

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
