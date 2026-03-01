# Discipline Protocol

Techniques for systematic, thorough processing to prevent skipping and incomplete coverage.

Apply techniques to process exhaustively.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Coverage Tracker, Enumeration List
- [Core Approaches](#core-approaches) — Enumeration, Coverage Tracking, Verification

---

## Goal

Ensure systematic processing of all items with explicit verification that nothing is skipped.

---

## Intent

LLMs frequently skip items, process subsets, or make assumptions about completeness without verifying. These failures stem from automatic processing that favors shortcuts over thoroughness. Explicit enumeration and tracking forces controlled processing, preventing tunnel vision, primacy/recency bias, and premature completion.

---

## Scope

Systematic processing patterns including exhaustive enumeration, coverage tracking, and completeness verification. Applies whenever processing lists, evaluating multiple items, or working through sets of requirements.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Enumeration List** | Explicit numbered list of all items to process | Before processing any multi-item task | Output |
| **Coverage Tracker** | Record of which items processed with status | During and after multi-item processing | Output |

**Enumeration List Format:** `1. [item] 2. [item] ... N. [item]`

**Coverage Tracker Format:**
| Item | Status | Notes |
|------|--------|-------|
| [item] | ✓ Done / ⏳ Pending / ✗ Blocked | [observations] |

---

## Core Approaches

### Enumeration Techniques

Use enumeration to establish complete scope before processing.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **MECE Enumeration** | Multi-item task with potential for missed items | To ensure mutually exclusive, collectively exhaustive coverage | List all items explicitly, number each item, verify no overlap (mutually exclusive), verify no gaps (collectively exhaustive), state total count | Enumeration List | Is every item numbered? Is total count stated? Are items non-overlapping? | Are you starting to process before enumeration completes? |
| **Source-Based Enumeration** | Items come from specific source (list, table, requirements) | To prevent selective reading or assumption-based filtering | Read entire source, extract each item verbatim, number in order found, cross-reference against source for completeness | Enumeration List | Is each item traceable to source? Is source fully consumed? | Are you paraphrasing or summarizing instead of extracting? |
| **Decomposition Enumeration** | Single complex item needs breakdown into sub-items | To ensure all aspects of complex items are addressed | Break item into constituent parts, enumerate sub-items, verify decomposition is complete, state sub-item count | Enumeration List | Are sub-items exhaustive? Can they be recombined to the original? | Are you decomposing beyond useful granularity? |

### Coverage Tracking Techniques

Use coverage tracking to maintain awareness of progress through enumerated items.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Explicit Progress Marking** | Processing enumerated list item by item | To prevent losing track of which items are done | Process item, mark status immediately after, update tracker before proceeding to next item, output tracker periodically | Coverage Tracker | Is each item marked immediately after processing? Is tracker current? | Are you batching status updates instead of marking immediately? |
| **Sequential Processing** | Order matters or dependencies exist between items | To ensure correct processing order and no skipping | Process in enumerated order, complete current item before advancing, mark completion before moving to next, do not skip ahead | Coverage Tracker | Is processing order matching enumeration order? Is each step completed before next? | Are you jumping ahead to "easy" or "interesting" items? |
| **Randomized Sampling Check** | Long list where sequential bias may cause later items to get less attention | To counteract primacy bias (more attention to early items) | After sequential pass, select random items from middle and end, verify equal quality of processing, re-process if quality differs | Coverage Tracker | Are later items processed with equal rigor as earlier items? | Are early items getting more thorough treatment? |

### Verification Techniques

Use verification to confirm completeness after processing.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Count Verification** | After processing enumerated list | To confirm all items were processed | Count processed items, compare to enumeration total, identify any discrepancy, resolve missing items | — | Does processed count match enumeration count? | Are you assuming completion without counting? |
| **Coverage Audit** | After claiming completion | To detect items skipped or partially processed | Review each item in enumeration, verify processing evidence exists for each, flag items with no evidence, re-process flagged items | Coverage Tracker | Is evidence documented for each item? Are all items marked complete? | Are you marking items complete without processing them? |
| **Boundary Check** | After processing when source has clear boundaries | To verify first and last items received equal attention | Verify first item processed, verify last item processed, verify boundary items not truncated or summarized | — | Are first and last items fully processed? | Did you trail off or rush at boundaries? |

---

## Example Discipline Patterns

Structured example sequences for systematic processing. Far more are possible.

### Processing a List Systematically

Use when given a list of items to evaluate, process, or address.

1. Enumerate all items explicitly with numbers
2. State total count: "N items to process"
3. Process item 1, mark complete immediately
4. Process item 2, mark complete immediately
5. Continue until all items processed
6. Verify: count completed items matches N
7. Output final coverage tracker

**Exit Conditions:**
- Item cannot be processed (blocked, missing info) → mark as blocked, continue to next, return to blocked items after
- Count mismatch at verification → identify missing items, process them, re-verify

### Evaluating Against Multiple Criteria

Use when assessing work against a set of requirements or criteria.

1. Enumerate all criteria from source explicitly
2. Create coverage tracker with all criteria
3. For each criterion in order:
   - State criterion explicitly
   - Evaluate against criterion
   - Document evidence (pass/fail with reasoning)
   - Mark criterion complete
4. After all criteria evaluated, count completed
5. Verify count matches total criteria
6. Summarize: which passed, which failed, which blocked

**Exit Conditions:**
- Criterion ambiguous → attempt interpretation, document uncertainty, continue
- Cannot evaluate criterion (missing artifact) → mark blocked, continue to next

---

## References

**MECE Principle:**
- [The Minto Pyramid Principle - Barbara Minto](https://www.barbaraminto.com/)
- [MECE Framework - McKinsey & Company](https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights)
- [Mutually Exclusive Collectively Exhaustive - Wikipedia](https://en.wikipedia.org/wiki/MECE_principle)

**Checklist Methodology:**
- [The Checklist Manifesto - Atul Gawande](https://atulgawande.com/book/the-checklist-manifesto/)
- [Checklist-based evaluation methods - PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9082971/)

**Cognitive Bias and Debiasing:**
- [Thinking, Fast and Slow - Daniel Kahneman](https://www.penguinrandomhouse.com/books/89308/thinking-fast-and-slow-by-daniel-kahneman/)
- [Cognitive Biases in Decision Making - Behavioral Economics](https://www.behavioraleconomics.com/resources/mini-encyclopedia-of-be/cognitive-bias/)
- [Debiasing through controlled processing - Journal of Consumer Research](https://academic.oup.com/jcr/)
