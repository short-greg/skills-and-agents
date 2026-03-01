# Criteria Setting

Systematic techniques for establishing clear, verifiable criteria.

Establish criteria to enable objective verification.

---

## Goal

Establish clear, verifiable criteria for concepts, goals, requirements, and standards.

---

## Intent

Without clear criteria, you cannot determine if something applies, meets standards, is complete, or achieves goals. Vague or missing criteria lead to ambiguous requirements, subjective evaluations, and unclear success conditions.

---

## Scope

Techniques for creating criteria when none exist, making vague criteria concrete, defining concepts, setting quality standards, establishing classification rules, and determining completion conditions.

---

## Core Approaches

### Specification Techniques

Use specification techniques to make criteria explicit, measurable, and verifiable.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Observable Behavior** | Criteria must be verifiable through observation | To ensure criteria can be objectively checked | Identify what can be measured or observed, specify exact behavior or state, define how to observe it, document observation method | Is behavior/state observable? Is observation method specified? | Are you specifying internal states that cannot be observed? |
| **Threshold Setting** | Criteria need clear pass/fail or acceptance boundaries | To enable unambiguous pass/fail decisions | Identify measurable dimension, determine acceptable range or cutoff value, specify units and precision, document rationale for threshold | Is threshold explicit? Are units specified? | Is threshold vague (e.g., "fast", "good")? |
| **Operational Definition** | Abstract concepts need concrete measurement procedures | To translate concepts into measurable procedures | Define characteristic to measure, specify test method with start/finish points, state decision criteria (pass/fail), document degree of accuracy | Are measurement procedures explicit? Is decision criteria clear? | Are you using concepts without defining how to measure them? |
| **SMART Criteria** | Goals or requirements lack specificity | To make criteria testable and time-bound | Make Specific (clear scope), Measurable (quantifiable), Achievable (realistic), Relevant (aligned to goals), Time-bound (deadline specified) | Are all SMART elements present? | Are criteria vague, unmeasurable, or open-ended? |

### Categorization Techniques

Use categorization techniques to establish classification criteria and structure.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Binary Criteria** | Classification is simple yes/no decision | To create unambiguous pass/fail criteria | Define condition that must be met, specify exactly what constitutes pass vs fail, provide examples of each, test with edge cases | Is pass/fail boundary clear? Are edge cases handled? | Are there gray areas where pass/fail is ambiguous? |
| **Scale Criteria** | Quality exists on a continuum | To enable gradations of quality assessment | Define scale points (e.g., 1-5, poor-excellent), specify indicators for each level, provide examples at each level, establish how to assign ratings | Are scale points defined? Is each level distinguishable? | Are adjacent levels indistinguishable from each other? |
| **Categorical Criteria** | Multiple distinct categories exist | To classify into discrete types or levels | List all possible categories, define membership criteria for each, ensure categories are mutually exclusive, specify how to handle edge cases | Are categories mutually exclusive? Is classification unambiguous? | Do items fit multiple categories or none? |
| **Rubric Criteria** | Multiple dimensions need assessment | To evaluate across multiple independent criteria | Identify evaluation dimensions, define criteria for each dimension, assign weights if needed, specify scoring method, create assessment template | Are all dimensions covered? Are criteria independent? | Are dimensions overlapping or redundant? |
| **Aggregation Logic** | Multiple criteria must combine into overall verdict | To specify how individual criteria combine | Choose logic: AND (all must pass), OR (any can pass), weighted (some matter more), threshold (X of Y must pass), document aggregation rule explicitly | Is aggregation logic explicit? Is overall verdict determinable? | Is it unclear how criteria combine? Can different people reach different overall verdicts? |

### Refinement Techniques

Use refinement techniques to transform vague criteria into concrete specifications.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Example-Based** | Criteria too abstract to specify directly | To ground criteria in concrete examples | Provide positive examples (meets criteria), provide negative examples (fails criteria), identify distinguishing features, extract pattern from examples | Are both positive and negative examples provided? Do examples cover edge cases? | Are examples themselves ambiguous? |
| **Boundary Analysis** | Unclear where criteria applies or stops | To clarify inclusion/exclusion boundaries | Identify edge cases between pass and fail, determine classification for each edge case, document boundary rules explicitly, test with ambiguous cases | Are boundary rules explicit? Are edge cases resolved? | Do boundary cases remain ambiguous? |
| **Decomposition** | Criteria too complex to specify as single condition | To break complex criteria into simpler parts | Break criteria into independent sub-criteria, specify each sub-criteria separately, define how to combine results (AND/OR), validate completeness | Are sub-criteria independent? Is combination logic clear? | Do sub-criteria overlap or conflict? |
| **Concretization** | Criteria uses vague qualitative terms | To replace vague terms with measurable indicators | Identify vague terms (fast, good, user-friendly), determine what makes something satisfy the term, specify measurable proxy indicators, define thresholds for indicators | Are vague terms eliminated? Are indicators measurable? | Do criteria still contain unmeasurable terms? |

---

## Example Patterns

Structured example sequences for establishing criteria in different contexts. Far more are possible.

### Establishing Acceptance Criteria

Use when defining requirements or acceptance conditions for deliverables.

1. Identify what needs criteria (requirement, feature, deliverable)
2. Determine criteria type needed (binary, scale, categorical, rubric)
3. Specify observable behavior or measurable outcomes
4. Set thresholds or decision boundaries
5. Provide positive and negative examples
6. Test criteria with edge cases
7. Document criteria in standard format (Given-When-Then or checklist)

**Exit Conditions:**
- Cannot identify observable behavior → stop, refine requirement to be more concrete
- Criteria remains subjective after multiple attempts → stop, request stakeholder clarification
- Too many dimensions to capture in single criteria → stop, decompose into multiple criteria

### Making Vague Criteria Concrete

Use when refining ambiguous or qualitative criteria.

1. Identify vague terms in existing criteria
2. Generate positive examples that meet criteria
3. Generate negative examples that fail criteria
4. Extract distinguishing features from examples
5. Specify measurable indicators for features
6. Set thresholds for each indicator
7. Test refined criteria against original examples
8. Verify stakeholders agree criteria captures intent

**Exit Conditions:**
- No measurable indicators can be found → stop, criteria may be inherently subjective
- Stakeholders disagree on examples → stop, resolve disagreement before proceeding
- Refined criteria excludes valid cases or includes invalid ones → stop, revise indicators

---

## References

**Anthropic/Claude documentation:**
- [Customer support agent - Claude API Docs](https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat)

**Requirements engineering:**
- [Requirements Engineering Process - GeeksforGeeks](https://www.geeksforgeeks.org/software-engineering/software-engineering-requirements-engineering-process/)
- [Requirements Engineering Best Practices - Perforce](https://www.perforce.com/blog/alm/requirements-engineering-examples)
- [Good Practices for Requirements Engineering - InformIT](https://www.informit.com/articles/article.aspx?p=3172443&seqNum=2)

**Acceptance criteria and SMART:**
- [What is acceptance criteria? - ProductPlan](https://www.productplan.com/glossary/acceptance-criteria/)
- [Acceptance Criteria Guide - Parallel](https://www.parallelhq.com/blog/what-acceptance-criteria)
- [SMART criteria - Wikipedia](https://en.wikipedia.org/wiki/SMART_criteria)

**Operational definitions:**
- [Operational Definitions in Software Engineering - Springer](https://link.springer.com/chapter/10.1007/978-3-032-04200-2_6)
- [Software Operational Requirements - FlyLib](https://flylib.com/books/en/4.66.1.58/1/)
- [Operational definition - Advantive](https://www.advantive.com/solutions/spc-software/quality-advisor/data-collection-tools/operational-definition/)
