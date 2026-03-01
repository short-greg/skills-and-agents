# Software Quality

Systematic quality assessment across complementary dimensions.

Assess quality as multi-objective optimization with tradeoffs, not checklist compliance.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Quality Assessment, Dimension Priorities, Issue Classification, Tradeoff Record
- [Core Approaches](#core-approaches) — Functional Dimensions, Structural Dimensions, Operational Dimensions
- [Example Patterns](#example-patterns) — Assessing Artifact Quality, Context-Based Prioritization

---

## Goal

Enable evaluation of artifacts across quality dimensions to identify strengths, weaknesses, and improvement opportunities.

---

## Intent

Quality is multi-dimensional—artifacts can excel in one dimension while failing in another. Without explicit criteria, assessments become subjective and miss critical issues. Different contexts prioritize different dimensions: production systems need reliability and security, prototypes need correctness, libraries need usability and maintainability.

---

## Scope

Techniques for assessing quality dimensions across functional, structural, and operational categories. Analyzing tradeoffs between dimensions and prioritizing improvements based on context. Applies to code, documentation, workflows, and designs.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Quality Assessment** | Evaluation results across quality dimensions | When reviewing artifacts for quality | Output |
| **Dimension Priorities** | Context-based ranking of which dimensions matter | When determining quality focus for artifact type | Output |
| **Issue Classification** | Issues categorized by severity (critical/high/medium/low) | When identifying what needs improvement | Output |
| **Tradeoff Record** | Documented accepted tradeoffs with rationale | When quality in one dimension is sacrificed for another | Either |

---

## Core Approaches

### Functional Dimensions

Assess functional dimensions to evaluate what the artifact does.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Correctness** | Evaluating if artifact does what it should | To identify functional failures | Check requirements met, verify edge cases handled, test expected behavior, confirm no incorrect outputs | Quality Assessment | Does it do what it should? Are edge cases handled? | Are there incorrect outputs? Missing requirements? |
| **Completeness** | Evaluating coverage of requirements | To identify gaps | Check necessary cases covered, verify requirements addressed, identify critical gaps, confirm scope fulfilled | Quality Assessment | Are necessary cases covered? Are critical gaps absent? | Are common cases missing? Are requirements unaddressed? |
| **Robustness** | Evaluating handling of unexpected inputs | To identify fragility | Check invalid input handling, verify graceful degradation, test boundary conditions, confirm no crashes on bad data | Quality Assessment | Does it handle unexpected inputs gracefully? | Does it crash or corrupt on invalid input? |
| **Security** | Evaluating resistance to vulnerabilities | To identify attack vectors | Check input validation, verify no injection vulnerabilities, assess authentication/authorization, confirm sensitive data protected | Quality Assessment | Are inputs validated? Is sensitive data protected? | Are there injection risks? Exposed credentials? |

### Structural Dimensions

Assess structural dimensions to evaluate how the artifact is built.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Readability** | Evaluating understandability | To identify comprehension barriers | Check names are descriptive, verify structure is obvious, assess if intent is clear, test if newcomer understands quickly | Quality Assessment | Is it understandable quickly? Are names clear? | Is it confusing? Are names cryptic? Is intent hidden? |
| **Simplicity** | Evaluating unnecessary complexity | To identify over-engineering | Check for unnecessary abstraction, verify no premature optimization, assess if solution matches problem complexity | Quality Assessment | Is complexity proportionate to problem? Is it as simple as possible? | Is there unnecessary abstraction? Over-engineering? |
| **Consistency** | Evaluating pattern adherence | To identify unpredictable behavior | Check naming conventions followed, verify similar things done similarly, assess error handling patterns consistent | Quality Assessment | Are patterns consistent? Is behavior predictable? | Do similar things work differently? Are conventions violated? |
| **Maintainability** | Evaluating ease of modification | To identify change barriers | Check changes localize, verify low coupling between parts, assess if modifications are safe, confirm refactoring is feasible | Quality Assessment | Can it be modified safely? Do changes localize? | Do changes ripple? Is it afraid to touch? |

### Operational Dimensions

Assess operational dimensions to evaluate how the artifact performs.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Reliability** | Evaluating consistent operation | To identify failure modes | Check failure handling, verify recovery from errors, test under adverse conditions, confirm consistent behavior | Quality Assessment | Does it work consistently? Does it recover from errors? | Does it fail silently? Are errors unhandled? |
| **Efficiency** | Evaluating resource usage | To identify waste | Check resource use reasonable for context, identify obvious waste, verify no unnecessary operations | Quality Assessment | Is resource use reasonable? Is there no obvious waste? | Is there premature optimization? Unnecessary complexity? |
| **Testability** | Evaluating verifiability | To identify testing barriers | Check behavior is verifiable, confirm testable in isolation, verify observable outcomes | Quality Assessment | Is behavior verifiable? Is it testable in isolation? | Is testing harder than implementation? |
| **Usability** | Evaluating ease of correct use | To identify misuse risks | Check API is intuitive, verify hard to use incorrectly, assess error messages helpful, confirm documentation sufficient | Quality Assessment | Is it easy to use correctly? Hard to misuse? | Is it easy to use incorrectly? Are errors cryptic? |

---

## Example Patterns

Structured example sequences for quality assessment. Far more are possible.

### Assessing Artifact Quality

Use when reviewing code, documentation, or designs.

1. Identify artifact type and context (production, prototype, library, tool)
2. Determine critical dimensions for this context
3. Assess each critical dimension systematically
4. Identify tradeoffs between dimensions
5. Classify issues by severity (high: correctness/security/reliability failures)
6. Prioritize improvements by impact vs effort
7. Document accepted tradeoffs and rationale

**Exit Conditions:**
- Critical dimension unclear → stop, clarify requirements and context
- All dimensions seem equally important → stop, identify actual use case
- Too many issues to address → stop, focus on high-severity only

### Context-Based Prioritization

Use when deciding which dimensions matter most.

1. Identify artifact type:
   - Production system → Correctness, Reliability, Security
   - Prototype → Correctness (others less critical)
   - Library/API → Usability, Maintainability, Testability
   - Internal tool → Readability, Simplicity
2. Identify operational context (frequency, failure cost, modification rate)
3. Prioritize dimensions based on context
4. Accept lower quality in non-critical dimensions
5. Document prioritization rationale

**Exit Conditions:**
- Context unclear → stop, clarify how artifact will be used
- All dimensions critical → stop, scope may be too broad
- Prioritization feels wrong → stop, revisit context assumptions

---

## References

- [ISO 25010 Software Quality Model](https://iso25000.com/en/iso-25000-standards/iso-25010)
- [Software Quality - Wikipedia](https://en.wikipedia.org/wiki/Software_quality)
- [OWASP Top 10 Security Risks](https://owasp.org/www-project-top-ten/)
