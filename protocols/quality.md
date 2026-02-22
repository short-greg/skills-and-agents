# Project Quality Protocol

Enable systematic assessment of project artifacts across complementary quality dimensions.

---

## Goal

Enable evaluation of artifacts across essential quality dimensions to identify strengths, weaknesses, and improvement opportunities.

---

## Intent

Quality is multi-dimensional—an artifact can excel in one dimension while failing in another. Without explicit quality criteria, assessments default to subjective "looks good to me" reviews that miss critical issues. This protocol provides a framework for evaluating quality across complementary dimensions with inherent tradeoffs. Like modularity, quality assessment is multi-objective optimization, not checklist compliance.

---

## Scope

This protocol addresses quality assessment: correctness verification, clarity evaluation, reliability indicators, completeness assessment, efficiency considerations, and testability factors across code, documentation, workflows, and system designs.

---

## Key Results

- Quality issues are identified systematically, not discovered accidentally
- Tradeoffs between quality dimensions are explicit and intentional
- Assessment criteria are consistent across similar artifacts
- Improvement efforts focus on dimensions that matter for the use case
- Quality discussions are objective (based on criteria) rather than subjective

---

## Essential Concepts

### 1. Correctness

**What it is:** The degree to which an artifact behaves according to its specification or intent.

**Why it matters:** Incorrect artifacts produce wrong outcomes, break systems, mislead users, or violate requirements. Correctness is foundational—other quality dimensions don't matter if the artifact doesn't do what it should.

**Applies to:**
- **Code** - Does it implement requirements? Handle edge cases? Produce correct outputs?
- **Documentation** - Are facts accurate? Examples correct? Instructions complete?
- **Workflows** - Do steps produce intended outcomes? Handle failures appropriately?
- **Designs** - Do components satisfy requirements? Address constraints?

**Assessment questions:**
- Does this do what it's supposed to do?
- Are edge cases handled correctly?
- Do examples run and produce expected results?
- Are requirements satisfied?

**Tradeoff:** Achieving perfect correctness often conflicts with shipping quickly or keeping implementations simple.

### 2. Clarity

**What it is:** The ease with which someone can understand an artifact's purpose, behavior, and structure.

**Why it matters:** Unclear artifacts impose cognitive load, require extensive investigation to understand, increase modification time, and cause errors during maintenance. Clarity directly impacts maintainability and onboarding speed.

**Applies to:**
- **Code** - Readable variable names, clear control flow, obvious intent
- **Documentation** - Clear structure, plain language, well-organized information
- **Workflows** - Understandable steps, clear responsibilities, obvious sequence
- **Designs** - Clear component boundaries, explicit relationships, obvious data flow

**Assessment questions:**
- Can someone unfamiliar understand this quickly?
- Is the purpose immediately clear?
- Are names descriptive and consistent?
- Is the structure obvious or confusing?
- Does complexity obscure intent?

**Dimensions of clarity:**
- **Naming** - Do names reveal intent? (getUserById vs get vs fetch)
- **Structure** - Is organization logical? (related things together)
- **Abstraction level** - Consistent granularity? (not mixing high/low level)
- **Explicitness** - Are assumptions stated or hidden?

**Tradeoff:** Maximum clarity can conflict with brevity or performance optimization.

### 3. Reliability

**What it is:** The degree to which an artifact performs consistently under expected and unexpected conditions.

**Why it matters:** Unreliable artifacts fail unpredictably, require constant intervention, lose user trust, and create downstream failures. Reliability determines whether something can be depended on in production.

**Applies to:**
- **Code** - Handles errors gracefully, recovers from failures, validates inputs
- **Documentation** - Consistently accurate, regularly updated, trustworthy
- **Workflows** - Handles interruptions, recovers from failures, produces consistent results
- **Designs** - Tolerates component failures, degrades gracefully, handles load

**Assessment questions:**
- Does this work consistently?
- How does it behave under stress or failure?
- Are errors handled or do they cascade?
- Can it recover from interruption?
- Are failure modes understood?

**Reliability factors:**
- **Error handling** - Graceful degradation vs crashes
- **Input validation** - Defensive vs trusting
- **Fault tolerance** - Recovery mechanisms vs failure propagation
- **Consistency** - Predictable behavior vs surprising edge cases

**Tradeoff:** High reliability often requires more code, complexity, and defensive programming.

### 4. Completeness

**What it is:** The degree to which an artifact addresses all necessary aspects of its domain.

**Why it matters:** Incomplete artifacts require constant patching, miss critical use cases, create gaps that cause failures, and lead to workarounds. Completeness determines whether something actually solves the problem.

**Applies to:**
- **Code** - Handles all requirements, covers edge cases, includes necessary validations
- **Documentation** - Covers all features, explains all concepts, provides all examples
- **Workflows** - Includes all necessary steps, addresses all scenarios, handles all outcomes
- **Designs** - Addresses all requirements, considers all constraints, handles all interactions

**Assessment questions:**
- What's missing?
- Are there gaps in coverage?
- Do edge cases have solutions?
- Are requirements fully addressed?
- What scenarios are unhandled?

**Completeness vs scope:**
- **Over-complete** - Solves problems that don't exist (gold-plating)
- **Under-complete** - Missing critical functionality (gaps)
- **Right-sized** - Covers what matters for the use case

**Tradeoff:** Perfect completeness conflicts with simplicity and shipping speed.

### 5. Efficiency

**What it is:** The degree to which an artifact accomplishes its purpose with reasonable resource consumption.

**Why it matters:** Inefficient artifacts waste time, consume excessive resources, create bottlenecks, and degrade user experience. Efficiency determines scalability and operating cost.

**Applies to:**
- **Code** - Time complexity, space complexity, resource utilization
- **Documentation** - Information density, navigation speed, search effectiveness
- **Workflows** - Steps required, time to complete, effort needed
- **Designs** - Resource allocation, data flow efficiency, communication overhead

**Assessment questions:**
- Is this wasteful or economical?
- Could this accomplish the same goal with less?
- Are there obvious inefficiencies?
- Does performance matter here?
- What's the resource-to-value ratio?

**Efficiency dimensions:**
- **Time** - How long does it take?
- **Space** - How much memory/storage?
- **Effort** - How much work is required?
- **Complexity** - How much cognitive load?

**When efficiency matters:**
- High-frequency operations (called often)
- Resource-constrained environments (embedded, mobile)
- Scale-sensitive systems (large data, many users)
- Cost-critical applications (cloud resources)

**When efficiency doesn't matter:**
- One-time operations (setup, initialization)
- Low-frequency operations (admin tasks)
- Development/debugging tools (clarity > speed)
- Prototypes and experiments (learning > optimization)

**Tradeoff:** Premature optimization reduces clarity and maintainability. "Premature optimization is the root of all evil" applies—optimize when measurements prove it matters.

### 6. Testability

**What it is:** The ease with which an artifact can be verified, validated, or assessed for correctness.

**Why it matters:** Untestable artifacts cannot be verified reliably, require manual inspection for every change, create fear of modification, and accumulate undetected bugs. Testability enables confident iteration.

**Applies to:**
- **Code** - Unit testable, integration testable, observable behavior
- **Documentation** - Examples are runnable, facts are verifiable, links are checkable
- **Workflows** - Steps are verifiable, outcomes are measurable, checkpoints exist
- **Designs** - Components are independently verifiable, interfaces are testable, behavior is observable

**Assessment questions:**
- How would you verify this works correctly?
- Can this be tested in isolation?
- Is behavior observable?
- Are success criteria clear?
- Can tests be automated?

**Testability factors:**
- **Isolation** - Can parts be tested independently?
- **Observability** - Can you see what's happening?
- **Determinism** - Same inputs produce same outputs?
- **Controllability** - Can you set up test conditions?

**Patterns that enable testability:**
- Dependency injection (not global state)
- Pure functions (deterministic, no side effects)
- Clear interfaces (observable inputs/outputs)
- Separation of concerns (test logic independently from infrastructure)

**Patterns that reduce testability:**
- Global mutable state (unpredictable interactions)
- Hidden dependencies (untestable in isolation)
- Side effects everywhere (non-deterministic behavior)
- Tight coupling (cannot test parts independently)

**Tradeoff:** Designing for testability sometimes requires more abstraction or indirection.

---

## The Multi-Dimensional Problem

Quality involves optimizing across competing dimensions:

| Dimension | Increases | Decreases |
|-----------|-----------|-----------|
| **Correctness** | Reliability, trust | Simplicity (more validation) |
| **Clarity** | Maintainability, onboarding speed | Brevity, sometimes performance |
| **Reliability** | Trust, production readiness | Simplicity (more error handling) |
| **Completeness** | Problem coverage | Simplicity, focus |
| **Efficiency** | Speed, scalability | Clarity (optimizations obscure) |
| **Testability** | Confidence, changeability | Directness (indirection for testing) |

**There is no universally "best" artifact.** The right balance depends on:
- How the artifact will be used
- What failure modes matter most
- What resources are constrained
- What the team knows and values

**Example tradeoffs:**

**Correctness vs Efficiency:**
- Exhaustive validation ensures correctness but adds overhead
- Fast paths skip checks but risk incorrect behavior

**Clarity vs Efficiency:**
- Clear, straightforward code is easy to understand but may be slower
- Optimized code uses tricks that obscure intent

**Completeness vs Simplicity:**
- Handling every edge case creates complexity
- Simple solutions miss corner cases

**Reliability vs Performance:**
- Retries and fallbacks add latency
- Fast systems fail hard without recovery

---

## Processes

### 1. Identify What Matters

Not all dimensions matter equally for every artifact. Ask:

**What's the primary use case?**
- Production system → Correctness, Reliability critical
- Internal tool → Clarity, Efficiency matter less
- Library → Clarity, Testability, Completeness critical
- Prototype → Correctness matters, others less so

**What are the constraints?**
- Time-constrained → Efficiency matters
- Safety-critical → Correctness, Reliability paramount
- Beginner audience → Clarity essential
- Legacy system → Completeness, Testability help

**What can fail?**
- User-facing → Correctness, Reliability critical
- Developer-facing → Clarity, Testability critical
- High-volume → Efficiency matters
- Low-volume → Efficiency matters less

### 2. Assess Each Dimension

For each relevant dimension, ask the assessment questions:

**Correctness:**
- Does this do what it should?
- Are edge cases handled?
- Do examples work?

**Clarity:**
- Can someone understand this quickly?
- Is structure obvious?
- Are names clear?

**Reliability:**
- Does this work consistently?
- How does it fail?
- Can it recover?

**Completeness:**
- What's missing?
- Are there gaps?
- Are requirements met?

**Efficiency:**
- Is this wasteful?
- Does performance matter here?
- Could this be simpler?

**Testability:**
- How would you verify this?
- Can parts be tested independently?
- Is behavior observable?

### 3. Identify Tradeoffs

When dimensions conflict, make explicit choices:

**Example: Code optimization**
- Improves: Efficiency
- Degrades: Clarity, Testability
- Decision: Optimize only if measurements prove it matters

**Example: Comprehensive error handling**
- Improves: Reliability, Correctness
- Degrades: Simplicity, Clarity
- Decision: Error handling for failures that matter, not hypothetical scenarios

**Example: Exhaustive documentation**
- Improves: Completeness, Clarity
- Degrades: Efficiency (maintenance burden)
- Decision: Document what's actually confusing, not everything

### 4. Prioritize Improvements

Not all quality issues matter equally:

**High priority:**
- Correctness failures (produces wrong results)
- Reliability failures (crashes, data loss)
- Clarity issues blocking understanding
- Missing critical functionality

**Medium priority:**
- Inefficiency in high-frequency operations
- Completeness gaps in common scenarios
- Testability issues preventing verification

**Low priority:**
- Inefficiency in rare operations
- Clarity issues in obvious code
- Completeness gaps in edge cases
- Testability issues in stable code

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pattern 1: Dimension-Appropriate Quality**
- Optimize the dimensions that matter for the use case
- Don't over-optimize dimensions that don't matter
- Example: Production API needs correctness/reliability, internal script needs clarity

**Pattern 2: Measurable Assessment**
- Use concrete questions, not vague feelings
- "Does this handle the null case?" vs "This feels wrong"
- Enables objective discussions

**Pattern 3: Explicit Tradeoffs**
- State what you're optimizing and what you're sacrificing
- Documents intent for future maintainers
- Example: "Optimized for speed (reduced clarity) because profiling showed bottleneck"

**Pattern 4: Progressive Quality**
- Start with correctness, add other dimensions as needed
- Don't prematurely optimize all dimensions
- Example: Prototype → Correct implementation → Clear code → Reliable system → Efficient at scale

### Anti-Patterns (❌)

**Anti-Pattern 1: Uniform Quality Across All Dimensions**
- Trying to maximize every dimension simultaneously
- Results in over-engineering, wasted effort
- **Instead:** Prioritize dimensions that matter for the use case

**Anti-Pattern 2: Premature Optimization**
- Optimizing efficiency before measuring
- Sacrifices clarity for unmeasured gains
- **Instead:** Optimize when measurements prove it matters, not speculatively

**Anti-Pattern 3: Subjective Assessment**
- "This doesn't feel right" without specific criteria
- Leads to bikeshedding and endless debates
- **Instead:** Use dimension-specific questions for objective assessment

**Anti-Pattern 4: Ignoring Tradeoffs**
- Believing all quality dimensions can be maximized
- Creates unrealistic expectations
- **Instead:** Acknowledge tradeoffs explicitly and choose intentionally

**Anti-Pattern 5: Quality as Checklist**
- Treating quality as boxes to check
- Misses the multi-objective optimization nature
- **Instead:** Think in terms of balancing competing objectives

---

## Examples

### Example 1: REST API Endpoint

**Context:** Designing endpoint to retrieve user profile

**Assessment:**

**Correctness:**
- ✅ Returns correct user data
- ✅ Handles user not found (404)
- ✅ Validates user ID format
- ❌ Missing: pagination for related data

**Clarity:**
- ✅ Clear endpoint naming: GET /users/{id}
- ✅ Obvious response structure
- ❌ Missing: documentation of error codes

**Reliability:**
- ✅ Handles database failures (returns 503)
- ✅ Times out long queries
- ❌ Missing: retry logic for transient failures

**Completeness:**
- ✅ Covers primary use case
- ❌ Missing: filtering related data
- ❌ Missing: field selection

**Efficiency:**
- ✅ Indexed database queries
- ❌ Missing: caching for frequent requests
- Tradeoff: Caching would improve efficiency but reduce correctness (stale data)

**Testability:**
- ✅ Unit testable (mocked database)
- ✅ Integration testable
- ✅ Observable behavior (status codes, responses)

**Result:** Prioritize adding pagination (completeness) and error documentation (clarity). Defer caching (efficiency) until usage data shows need.

### Example 2: Documentation Page

**Context:** Explaining how to configure authentication

**Assessment:**

**Correctness:**
- ✅ Examples run and work
- ❌ Missing: explanation of required vs optional fields
- ❌ Missing: error scenarios

**Clarity:**
- ✅ Clear headings and structure
- ❌ Too much detail in overview
- ❌ Jargon not explained

**Reliability:**
- ✅ Regularly updated
- ❌ Links not checked (some broken)

**Completeness:**
- ✅ Covers OAuth and API keys
- ❌ Missing: JWT configuration
- ❌ Missing: troubleshooting section

**Efficiency:**
- ✅ Quick to scan
- ❌ Information scattered across multiple pages

**Testability:**
- ✅ Examples are testable
- ❌ Examples not run in CI

**Result:** Prioritize adding JWT documentation (completeness), fixing broken links (reliability), and consolidating scattered information (efficiency).

### Example 3: Data Processing Workflow

**Context:** ETL pipeline for daily reports

**Assessment:**

**Correctness:**
- ✅ Transforms data correctly
- ❌ Missing: validation of input data quality
- ❌ Missing: verification of output

**Clarity:**
- ✅ Clear step names
- ❌ Complex SQL queries not explained
- ❌ Error messages cryptic

**Reliability:**
- ✅ Retries on transient failures
- ❌ Doesn't handle partial failures well
- ❌ No alerting when silent failures occur

**Completeness:**
- ✅ Handles daily reports
- ❌ Missing: historical backfill capability
- ❌ Missing: manual trigger option

**Efficiency:**
- ❌ Processes all data every time (no incremental)
- ❌ Serial processing (could parallelize)
- Critical: Runtime increasing with data volume

**Testability:**
- ✅ Steps testable independently
- ❌ End-to-end testing requires production data
- ❌ No test fixtures for edge cases

**Result:** Prioritize efficiency (incremental processing critical), then reliability (alerting), then testability (test fixtures).

---

## Key Takeaways

- **Quality is multi-dimensional** - no single metric captures it all
- **Dimensions have tradeoffs** - maximizing one often degrades another
- **Context determines priorities** - what matters depends on use case
- **Assessment should be objective** - use specific questions, not feelings
- **Optimize what matters** - don't over-engineer dimensions that don't
- **Tradeoffs should be explicit** - document what you're optimizing and why
- **Progressive quality works** - start with correctness, add dimensions as needed
- **Perfect quality is impossible** - aim for appropriate quality, not maximum quality

---

## When to Apply This Protocol

- When designing new systems (identify which dimensions matter)
- When reviewing code or documentation (assess across relevant dimensions)
- When debugging quality issues (identify which dimension is failing)
- When refactoring (understand tradeoffs of different approaches)
- When setting quality standards (decide which dimensions to prioritize)
- When evaluating third-party tools (assess fit across dimensions)
- During architecture reviews (evaluate tradeoffs systematically)

---

## Tool-Based Assessment

Quality assessment must be objective and evidence-based. LLMs tend to over-evaluate quality with subjective assessments like "looks organized" or "no errors." Require tool output and specific evidence for all assessments.

### Modularity/Maintainability Tools

**Python:**
```bash
# Cohesion - check for too many methods/attributes
pylint --disable=all --enable=R0904,R0902 src/

# Coupling - count imports per file
find src/ -name "*.py" -exec grep -c "^import\|^from" {} \;

# Circular dependencies
pydeps src/ --show-deps

# Complexity
radon cc src/ -a -nb
# Target: A-B rating (complexity 1-10)
```

**JavaScript/TypeScript:**
```bash
# Complexity and maintainability
eslint src/ --ext .js,.ts
npx madge --circular src/

# Dependency analysis
npx madge --image graph.png src/
```

**Thresholds:**
| Metric | Good | Warning | Poor |
|--------|------|---------|------|
| Public functions per module | <5 | 5-10 | >10 |
| Direct dependencies | <5 | 5-10 | >10 |
| Cyclomatic complexity | <10 | 10-15 | >15 |
| Function length (lines) | <30 | 30-50 | >50 |
| Nesting depth | <3 | 3-4 | >4 |

### Test Coverage Tools

```bash
# Python
pytest tests/ --cov=src --cov-report=term-missing
# Target: >85% for production, >50% for prototype

# JavaScript
npm test -- --coverage
# Target: >85% for production, >50% for prototype
```

### Security Tools

```bash
# Python
bandit -r src/
safety check
pip-audit

# JavaScript
npm audit
npx snyk test

# General - check for hardcoded secrets
grep -r "password\|secret\|api_key" src/
```

### Evidence Requirements

**Avoid subjective assessments:**

| Bad (Subjective) | Good (Evidence-Based) |
|------------------|----------------------|
| "Looks organized" | "`pylint --enable=R0904` shows 0 violations. Each module has <5 public methods." |
| "No errors" | "Coupling check: `pydeps --max-bacon=2` shows max depth 2. No circular dependencies." |
| "Functions are small" | "`radon cc -a` shows average complexity 6.2 (Grade A). No functions >15 complexity." |
| "Tests exist" | "`pytest --cov` shows 87% coverage (meets 85% threshold). See report: [output]" |

### Assessment Template

```markdown
# Quality Assessment: [Feature/Module]

**Date:** YYYY-MM-DD
**Repo Type:** [Prototype/Production/Library]

## Modularity
**Tool:** `pylint --enable=R0904,R0902 src/module.py`
**Result:** [output]
**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor

## Test Coverage
**Tool:** `pytest tests/ --cov=src`
**Result:** [N]% coverage
**Rating:** ✅ PASS / ❌ FAIL

## Security (Production only)
**Tool:** `bandit -r src/`
**Result:** [output]
**Rating:** ✅ PASS / ❌ FAIL

## Overall: ✅ PASS / ❌ FAIL
**Required Actions:** [None / List of fixes]
```

### Customization by Repo Type

**Prototype:**
- Focus: Functional correctness, basic readability
- Coverage: >50% acceptable
- Skip: Security scans, performance optimization, comprehensive documentation

**Production:**
- Full modularity assessment (all metrics)
- Coverage: ≥85% required
- Security scans required
- Performance requirements verified

**Library:**
- API correctness (especially public APIs)
- Coverage: ≥90% for public APIs
- Backward compatibility checks
- Semantic versioning compliance

---

## References

- [ISO 25010 Software Quality Model](https://iso25000.com/en/iso-25000-standards/iso-25010)
- [CISQ Code Quality Standards](https://www.it-cisq.org/standards/code-quality-standards/)
