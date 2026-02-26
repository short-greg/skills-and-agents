# Validation Protocol

**Type:** Protocol

Evidence-based verification against success criteria.

---

## Goal

Produce evidence-based verdicts on whether work meets criteria — prevent shipping broken or incomplete work.

---

## Intent

Validation catches issues before they reach users. Proper validation requires evidence for both passing AND failing to prevent confirmation bias.

---

## Scope

**Addresses:** Verifying outputs against criteria (code, UI, APIs, docs), evidence-based decisions, binary/fuzzy/categorical validation

**Does not address:** Formulating criteria (use `definition.md`), quality assessment (use `quality.md`)

---

## Core Concepts

### Criterion Types

- **Binary:** Pass/fail (e.g., "All tests pass")
- **Fuzzy:** Scale (e.g., "Coverage: 73%")
- **Categorical:** Levels (e.g., "Risk: low/medium/high")

### Falsification Principle (Popper)

Seek evidence that would disprove, not just confirm. This is why validation requires evidence for BOTH passing AND failing.

---

## Techniques

### Vague Criteria

If criteria are vague, use protocol `definition.md` to formulate rigorous definitions first.

### Evidence-Based Validation

For each criterion:
1. **Get realistic outputs** — For UI: screenshots. For APIs: actual responses. For behavior: observed execution.
2. **Output evidence for both outcomes:**
   - Binary: evidence for pass, evidence for fail
   - Fuzzy: measure actual value
   - Categorical: evidence for each level
3. **Only after outputting evidence, weigh and decide verdict**
4. **Output verdict with reasoning**

### Overall Verdict

- **Pass:** ALL criteria pass
- **Fail:** ANY criterion fails — document what failed, expected vs actual, reproduction steps

---

## Use Cases

- Acceptance testing before release
- Verifying implementation meets spec
- Testing against requirements

---

## Patterns

### Patterns (✅)

**Evidence for both sides:** Output evidence for passing AND failing before deciding

**Realistic outputs:** For UI/APIs, get actual outputs, not just code inspection

### Anti-Patterns (❌)

**Confirmation bias:** Only seeking confirming evidence. **Fix:** Actively seek disconfirming evidence

**Vague criteria:** "Good enough", "fast". **Fix:** Use `definition.md` first

**Code inspection only:** Validating UI by reading code. **Fix:** Capture actual rendered output

---

## Examples

### Example 1: Binary Validation

**Context:** "Submit button disabled when form invalid"

**Application:** Get realistic output (screenshot), output evidence for PASS (button grayed, `disabled` attribute), output evidence for FAIL (button clickable), verdict: PASS

### Example 2: Fuzzy Validation

**Context:** "API should be fast" → use `definition.md` → "Response time < 200ms (95th percentile, 100 users)"

**Application:** Load test, measure 95th percentile = 185ms, verdict: PASS (185ms < 200ms)

---

## References

- [Karl Popper - Falsification Theory](https://plato.stanford.edu/entries/popper/)
- [Farnam Street - Confirmation Bias](https://fs.blog/confirmation-bias/)
- [Virtuoso QA - Acceptance Testing](https://www.virtuosoqa.com/testing-guides/what-is-acceptance-testing)
